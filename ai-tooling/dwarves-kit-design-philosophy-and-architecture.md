---
title: "dwarves-kit design philosophy and architecture"
date: 2026-03-29
captured: 2026-03-29T07:19:30.284Z
tags: ["claude-code", "dwarves-kit", "sdd", "workflow", "philosophy"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
---
## Core thesis

dwarves-kit is a Claude Code workflow kit that covers the full SDLC: Think > Spec > Validate > Execute > Review > Docs > Ship > Retro. 9 hooks + 9 commands + 1 skill = 30 files. Designed for a solo tech lead managing contractors AND a full-time coder using Claude Code 6-8 hours/day.

## 7 design principles

1. **Guardrails over guidance** -- hooks (100% enforcement) beat CLAUDE.md rules (70%). Safety-critical rules get hooks, not documentation.

2. **Synthesize, don't originate** -- every pattern traces to a proven source repo. No novel inventions. Test standalone for 3+ months before merging.

3. **One kit, whole cycle** -- `.planning/SPEC.md` is the shared contract. Data flows through the cycle without re-entry. This is the core differentiator vs. using 3-4 separate tools.

4. **Shallow and wide beats deep and narrow** -- covering 7 phases at 70% depth is better than 2 phases at 100%. Biggest failures come from skipped phases, not insufficient depth.

5. **Bash over binaries** -- every hook is a readable shell script with jq. One carve-out: HUD/statusline may use Node.js for per-turn JSON parsing performance.

6. **External tools are dependencies, not features** -- check for Context Hub/Context7/codebase-memory-mcp and warn if missing. Never rebuild their functionality.

7. **Detect, don't dictate** -- suggest the right action based on project state (SessionStart context injection). Never block workflow progression except for safety (rm-rf, push-to-main).

## Hard limits

- Maximum 35 files
- Maximum 500ms per hook execution
- No compiled binaries
- No paid dependencies
- No LLM API calls in v1 hooks

## Source repos

| Repo | What was extracted |
|------|-------------------|
| GSD | .planning/ convention, atomic task breakdown |
| gstack | /office-hours forcing questions, /review paranoid reviewer, /ship release flow |
| Trail of Bits | rm-rf + push-to-main hooks, anti-rationalization, CLAUDE.md quality rules |
| ClaudeKit | /ck:plan validate interview gate, red-team 4-reviewer pattern |
| Context Hub | chub CLI skill, annotation persistence |

## Architecture

Three mechanism types, each serving a different purpose:

- **Commands** (human-triggered): /think, /spec, /spec-validate, /execute, /next, /review, /docs, /ship, /retro. For workflow phases where human judgment matters.
- **Hooks** (event-triggered): safety-gate, context-readiness, anti-rationalization, auto-format, spec-drift-guard, pre-compact-backup, post-compact-reinject, notification, permission-auto-approve. For enforcement that must be deterministic.
- **Skills** (Claude-triggered): get-api-docs. For behaviors Claude should adopt autonomously.

## Differentiation

Not better than GSD at specs, gstack at reviews, or Trail of Bits at security. The value is lifecycle continuity: one format, one directory, one install. A contractor clones the repo, runs install.sh, and gets the full workflow. No format translation between tools, no learning 3 different systems.