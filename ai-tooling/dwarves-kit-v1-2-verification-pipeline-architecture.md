---
title: "dwarves-kit v1.2 verification pipeline architecture"
date: 2026-03-29
captured: 2026-03-29T14:23:54.956Z
tags: ["dwarves-kit", "sdd", "multi-agent", "claude-code", "verification"]
source: "Claude.ai session on SDD multi-agent architecture design (March 2026)"
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

- **Verification agents are subagents, not hooks.** Hooks add latency to every tool use. Verification only runs at task boundaries. Subagents keep it scoped.
- **Read-only verifier.** task-verifier cannot modify code. Separation of concerns: verify and fix are different roles with different tool access.
- **Max 2 retries, not infinite.** Prevents runaway token consumption. If 2 fix cycles don't resolve it, the problem is architectural, not syntactic.
- **FAIL:escalate is never retried.** If the verifier says it needs human judgment, that's final. No silent workarounds.
- **No new hooks needed.** Everything runs inside the /execute command prompt logic. Stays within "bash over binaries."

## Phased rollout

- Phase 1 (done): Verification pipeline (task-verifier + fix-agent + updated /execute + /start)
- Phase 2 (next week): context-readiness v2 with command suggestions, path-scoped rules
- Phase 3 (month 2+): Agent Teams parallel execution, parallel review team. Only after Phase 1 is proven on 3+ real projects.

## File count

40 files total (was 37). 3 new files: agents/task-verifier.md, agents/fix-agent.md, commands/start.md. 1 modified: commands/execute.md. Within budget.

## What NOT to build

- Swarms: solo lead + 1-3 contractors doesn't need 5+ parallel agents
- Custom orchestration runtime: use Claude Code's native Task tool + Agent Teams
- Persistent cross-session agent memory: pre-compact/post-compact hooks handle compaction already
- Auto-eval loop: need 50+ real session transcripts first, currently at 0