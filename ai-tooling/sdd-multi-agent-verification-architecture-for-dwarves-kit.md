---
title: "SDD multi-agent verification architecture for dwarves-kit"
date: 2026-03-29
captured: 2026-03-29T10:26:03.395Z
tags: ["claude-code", "sdd", "multi-agent", "dwarves"]
source: "Claude.ai chat"
---
## Overview

Architecture design for extending dwarves-kit v1.1's single-orchestrator `/execute` command into a multi-agent SDD pipeline with dedicated verification agents, self-correction loops, and bootstrapping. Addresses 5 gaps in the current kit:

1. No verification agents (orchestrator does manual checklist, no dedicated verifier)
2. No parallel execution (philosophy says "dispatches tasks sequentially")
3. No bootstrapping/detection layer ("detect, don't dictate" principle stated but not implemented beyond context-readiness)
4. No agent team coordination (no handoffs, shared state, or conflict resolution)
5. No self-correction loop (current flow: report failure, ask user; wanted: detect > fix > re-verify > escalate after N retries)

Key constraint: stay within Claude Code's native capabilities (Task tool, custom subagents in `.claude/agents/`, hooks, slash commands). No external runtimes. Respect "bash over binaries" and "synthesize, don't originate" principles.

## Agent topology and message flow

![SDD multi-agent topology showing 5 phases: Think, Spec, Build, Review, Ship with verification agents and retry loops at each boundary](https://assets.han-ws.workers.dev/i/2026/03/sdd-multi-agent-topology.png)

The pipeline runs through 5 phases. Teal nodes are verifier agents (read-only subagents that audit output). Purple nodes are commands. Blue nodes are worker subagents. Gray nodes are human gates.

The critical addition is the **verify-fix-retry loop** in Phase 3 (Build): after each worker completes a task, a task-verifier subagent checks spec compliance, test results, and scope. If it fails, a fix-agent gets dispatched with the verifier's feedback. Max 2 retries before escalating to human.

## Phase 1: Verification pipeline (build first)

Highest-value change. Two new custom subagents + modified execute command.

### task-verifier.md (new subagent)

Custom subagent at `.claude/agents/task-verifier.md`. Read-only tools only (Read, Grep, Glob, Bash for test runners). Runs after each worker completes. Checks three things:

1. Acceptance criteria from SPEC.md are met
2. Test suite passes
3. No files modified outside the task's declared scope

Outputs structured verdict: `PASS`, `FAIL:fixable` (with specific fix instructions), or `FAIL:escalate` (ambiguous, needs human).

Source pattern: OMC's "architect verification" in the Ralph loop, adapted to run as read-only subagent.

### fix-agent.md (new subagent)

At `.claude/agents/fix-agent.md`. Gets spawned when verifier returns `FAIL:fixable`. Receives verifier feedback + original task context. Makes targeted fixes only.

Source pattern: Smart Ralph's "fail > fix > re-verify" loop.

### execute.md (modified orchestrator logic)

The dispatch flow changes from "dispatch > review > continue" to:

```
dispatch task (worker subagent)
  -> run task-verifier on output
  -> if PASS: mark done, continue
  -> if FAIL:fixable AND retry_count < 2:
       dispatch fix-agent with verifier feedback
       increment retry_count
       re-run task-verifier
  -> if FAIL:escalate OR retry_count >= 2:
       stop, show failure details, ask human
```

Retry cap at 2 is important. Without it, infinite fix loops burn tokens. Two retries catches most "forgot to import" or "test assertion wrong" issues. Anything deeper needs human judgment.

No new hooks needed. Verification happens inside the `/execute` command prompt logic, not as a separate hook.

## Phase 2: Bootstrap + routing (build second)

### start.md (new command)

Implements "detect, don't dictate" principle. Reads project state and suggests the right command:

- No `.planning/` exists -> "Run /user:think then /user:spec"
- SPEC.md status = DRAFT -> "Run /user:spec-validate"
- SPEC.md status = APPROVED -> "Run /user:execute"
- All tasks marked done -> "Run /user:review"
- REVIEW.md exists, no issues -> "Run /user:ship"
- Git has uncommitted changes -> "Run /user:ship or stash"

Source pattern: CCGS's `/start` router (detects project stage and routes).

### context-readiness.sh (upgraded)

Add command suggestion injection via `additionalContext`. Even without running `/start`, SessionStart hook nudges the user toward the right next command based on the same state detection logic.

### Path-scoped rules

Move `rules/backend-go.md` and `rules/frontend-ts.md` to `.claude/rules/` with YAML frontmatter scoping:

```yaml
---
globs: src/api/**
---
All API handlers must validate input with...
```

Source pattern: CCGS's path-scoped rules (`.claude/rules/*.md` with YAML frontmatter).

## Phase 3: Parallel execution (build only after Phase 1+2 proven)

Only build after using sequential verification on 3+ real projects and confirming the bottleneck is speed, not quality.

### execute-parallel.md (new command)

Uses Claude Code's experimental Agent Teams for phases with 2+ independent tasks:

- Enable: `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`
- Spawn teammates in separate worktrees (`--worktree` flag)
- Each teammate gets: task context + SPEC.md section + CLAUDE.md
- Shared task list tracks progress
- After all complete: run task-verifier on each

Source pattern: Claude Code Agent Teams (shared task list, peer messaging, worktree isolation).

### review-team.md (new command)

Parallel review: spawn 3 reviewer subagents simultaneously (security, architecture, test coverage), each read-only. Merge findings and deduplicate.

Source pattern: Addy Osmani's Agent Teams code review pattern.

## Implementation phases

![Three implementation phases: verification pipeline (this week), bootstrap + routing (next week), parallel + coordination (month 2+)](https://assets.han-ws.workers.dev/i/2026/03/sdd-implementation-phases.png)

## What NOT to build

- **Don't build a "swarm."** Solo lead + 1-3 contractors doesn't need 5+ parallel agents. Coordination overhead will eat more time than it saves at this scale.
- **Don't build a custom orchestration runtime.** GSD v2's Pi SDK, Ruflo's Node.js runtime, OMC's execution modes are products, not kit components. Use Claude Code's native Task tool + Agent Teams.
- **Don't build persistent cross-session agent memory yet.** Pre-compact/post-compact backup hooks already handle compaction. Session-persistent memory is a v3 concern.
- **Don't build the Karpathy auto-eval loop yet.** Zero users, zero real session transcripts. The eval corpus doesn't exist. The logging added to anti-rationalization is the right move. Build eval after 4 weeks of real usage data.

## File change summary

| File | Action | Phase |
|---|---|---|
| `.claude/agents/task-verifier.md` | Create | 1 |
| `.claude/agents/fix-agent.md` | Create | 1 |
| `commands/execute.md` | Modify (add verify + retry loop) | 1 |
| `commands/start.md` | Create | 2 |
| `hooks/context-readiness.sh` | Modify (add command suggestions) | 2 |
| `.claude/rules/backend-go.md` | Move + add frontmatter | 2 |
| `.claude/rules/frontend-ts.md` | Move + add frontmatter | 2 |
| `docs/PHILOSOPHY.md` | Update (document verification agents) | 2 |
| `commands/execute-parallel.md` | Create | 3 |
| `commands/review-team.md` | Create | 3 |
| `settings.json` | Add AGENT_TEAMS env flag | 3 |

## Key principle

Verification first, parallelism second, coordination third. Most SDD failures come from building the wrong thing (no verification), not from building too slowly (no parallelism).

## Source patterns cited

- OMC (oh-my-claudecode): architect verification in Ralph loop, ai-slop-cleaner Stop hook
- CCGS (Claude-Code-Game-Studios): `/start` router, path-scoped rules, Collaborative Design Principle
- Smart Ralph: fail > fix > re-verify self-correction loop
- Trail of Bits: anti-rationalization hook, rm-rf blocker, statusline
- Claude Code Agent Teams: shared task list, peer messaging, worktree isolation
- Addy Osmani: parallel code review agent team pattern