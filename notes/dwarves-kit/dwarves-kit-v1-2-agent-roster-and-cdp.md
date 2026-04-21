---
title: "dwarves-kit v1.2 agent roster and CDP"
date: 2026-03-29
captured: 2026-03-29T16:05:51.221Z
tags: ["dwarves-kit", "agents", "collaborative-design"]
source: "Migrated from ai-tooling/ (March 2026)"
aliases: []
status: refined
---
# dwarves-kit v1.2: agent roster and collaborative design protocol

## 8 agents

| Agent | Purpose | Tools | Model | Dispatched by |
|-------|---------|-------|-------|--------------|
| task-verifier | Read-only verification after each task | Read, Grep, Glob, test runners | sonnet | /execute |
| fix-agent | Targeted fixes on FAIL:fixable | Read, Write, Edit, test runners | sonnet | /execute |
| security-auditor | Deep security review (OWASP) | Read, Grep, Glob, npm audit, go vet | sonnet | /review-team |
| reviewer | Parameterized with 3 lenses | Read, Grep, Glob, test runners | sonnet | /review-team |
| research-stack | Map technology stack (brownfield) | Read, Grep, Glob | haiku | /spec |
| research-features | Map existing features in target area | Read, Grep, Glob, git log | sonnet | /spec |
| research-architecture | Map architecture patterns | Read, Grep, Glob, git log | sonnet | /spec |
| research-pitfalls | Find landmines before building | Read, Grep, Glob, git log | sonnet | /spec |

## Collaborative Design Protocol (CDP)

Five steps: Question > Options (2-3 with tradeoffs) > Recommendation > Decision (mode-dependent) > Record.

Three decision modes: lead (human picks), coder (orchestrator picks), autonomous (proceed + log, verifier checks after).

Referenced by: worker subagents, task-verifier, security-auditor, reviewer agent.

Source: CCGS Collaborative Design Principle, adapted for subagent autonomy modes.

## Related

- [[dwarves-kit-v1-2-verification-pipeline-architecture]] - how task-verifier and fix-agent work together in the verify-fix-retry loop
- [[sdd-multi-agent-verification-architecture]] - the broader architectural design these agents implement
- [[multi-agent-coding-brain-rot-scan-design-externalized-state-clean-handoffs]] - patterns for staying coherent while running multiple agents