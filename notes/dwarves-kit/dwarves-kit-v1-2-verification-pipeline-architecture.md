---
title: "dwarves-kit v1.2 verification pipeline architecture"
date: 2026-03-29
captured: 2026-03-29T16:05:28.828Z
tags: ["dwarves-kit", "sdd", "multi-agent", "verification"]
source: "Migrated from ai-tooling/ (March 2026)"
aliases: []
status: refined
---
# dwarves-kit v1.2: Verification pipeline architecture

## What changed from v1.1

v1.1 had `/execute` dispatching worker subagents per task but no automated verification. The orchestrator did a manual checklist review. v1.2 adds a structured verify-fix-retry loop.

## Three new components

### 1. task-verifier subagent (`.claude/agents/task-verifier.md`)
- Read-only custom subagent (tools: Read, Grep, Glob, test runners)
- Runs after each worker completes a task
- Checks: acceptance criteria met, test suite passes, scope compliance, spec drift, quality spot-check
- Three verdicts: PASS, FAIL:fixable (with fix instructions), FAIL:escalate (needs human)
- Source: OMC's architect verification in Ralph loop, adapted to read-only

### 2. fix-agent subagent (`.claude/agents/fix-agent.md`)
- Write-capable subagent scoped to specific files from verifier report
- Applies minimum changes to resolve FAIL:fixable issues
- Does NOT add features, refactor, or change files outside the verifier's report
- Source: Smart Ralph's fail-fix-re-verify loop

### 3. /start bootstrapping router (`commands/start.md`)
- Detects project state (no spec, draft spec, approved spec, tasks done, review needed, ready to ship)
- Suggests the right next command
- Implements the "detect, don't dictate" philosophy principle
- Source: CCGS's /start router

## The verification loop in /execute

```
dispatch worker subagent (Task tool)
  -> dispatch task-verifier (read-only)
  -> PASS: mark done, continue
  -> FAIL:fixable AND retries < 2:
       dispatch fix-agent with verifier feedback
       re-run task-verifier
  -> FAIL:escalate OR retries >= 2:
       stop, show details, ask human
```

Max 2 retries. Most fixable issues (missing import, wrong assertion) resolve in 1-2 cycles. Deeper issues need human judgment, not more token burn.

## Key design decisions

- **Verification agents are subagents, not hooks.** Hooks add latency to every tool use. Verification only runs at task boundaries.
- **Read-only verifier.** task-verifier cannot modify code. Separation of concerns.
- **Max 2 retries, not infinite.** Prevents runaway token consumption.
- **FAIL:escalate is never retried.** If it needs human judgment, that's final.
- **No new hooks needed.** Everything runs inside /execute command prompt logic.

## Related

- [[sdd-multi-agent-verification-architecture]] - the broader multi-agent architecture this pipeline implements
- [[dwarves-kit-v1-2-agent-roster-and-cdp]] - the full agent roster including task-verifier and fix-agent
- [[commands-vs-hooks-vs-skills-decision-framework]] - why verification uses subagents instead of hooks
- [[security-gates-for-mcp-tools-that-bridge-private-to-public]] - a similar gate pipeline pattern (ordered by cost ascending)