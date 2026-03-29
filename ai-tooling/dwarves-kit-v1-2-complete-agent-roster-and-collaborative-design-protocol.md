---
title: "dwarves-kit v1.2 complete agent roster and collaborative design protocol"
date: 2026-03-29
captured: 2026-03-29T15:50:01.075Z
tags: ["dwarves-kit", "sdd", "agents", "collaborative-design", "multi-agent"]
source: "Claude.ai session on agent roster + CDP (March 2026)"
---
# dwarves-kit v1.2 complete: agent roster + collaborative design protocol

## Final agent roster (8 agents)

| Agent | Purpose | Tools | Model | Dispatched by |
|-------|---------|-------|-------|--------------|
| task-verifier | Read-only verification after each task | Read, Grep, Glob, test runners | sonnet | /execute |
| fix-agent | Targeted fixes on FAIL:fixable | Read, Write, Edit, test runners | sonnet | /execute |
| security-auditor | Deep security review (OWASP checklist) | Read, Grep, Glob, npm audit, go vet | sonnet | /review-team |
| reviewer | Parameterized reviewer with 3 lenses | Read, Grep, Glob, test runners | sonnet | /review-team |
| research-stack | Map technology stack (brownfield) | Read, Grep, Glob | haiku | /spec |
| research-features | Map existing features in target area | Read, Grep, Glob, git log | sonnet | /spec |
| research-architecture | Map architecture patterns/conventions | Read, Grep, Glob, git log | sonnet | /spec |
| research-pitfalls | Find landmines before implementation | Read, Grep, Glob, git log | sonnet | /spec |

## Collaborative Design Protocol (CDP)

Formalized as `docs/COLLABORATIVE-DESIGN.md`. Five steps: Question > Options (2-3 with tradeoffs) > Recommendation > Decision (mode-dependent) > Record.

Three decision modes:
- **Lead mode**: pause for human approval
- **Coder mode**: orchestrator/verifier picks
- **Autonomous mode**: proceed with recommendation, log decision, verifier checks after

CDP is referenced by: worker subagents in /execute, task-verifier (checks protocol compliance), security-auditor, reviewer agent.

## New command: /review-team

Parallel 3-lens code review. Dispatches security, architecture, and test-coverage reviewers simultaneously. Merges findings, deduplicates, produces unified REVIEW.md. Use for PRs, contractor work, pre-release. Use /review for quick single-pass.

## Changes to existing files

- `/spec` Step 2: now dispatches 4 formal research agents instead of inline prompts
- `/execute` worker prompt: includes full CDP with decision mode
- `task-verifier`: added "decision protocol compliance" check (3b)
- `/spec-validate` Scope Critic: aggressive atomicity heuristic (>5 files, >3 sentences, >5 criteria = too large)
- `CLAUDE.md`: lists all 8 agents and /review-team command

## File count

49 total (was 41). 8 new files: 6 agents, 1 protocol doc, 1 command. No new hooks or bash scripts.

## Source citations

| Component | Source |
|-----------|--------|
| CDP | CCGS Collaborative Design Principle |
| Research agents | GSD v1 parallel researchers |
| Security auditor | Trail of Bits security review + OWASP Top 10 |
| Reviewer lenses | Addy Osmani parallel review + gstack /review |
| Aggressive atomicity | GSD "50% context window" principle |