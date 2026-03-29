---
title: "Claude Code ecosystem repo evaluations for kit building"
date: 2026-03-29
captured: 2026-03-29T07:20:17.973Z
tags: ["claude-code", "sdd", "tooling", "evaluation", "gsd", "gstack", "trail-of-bits"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
---
Studied 7 repos to identify the best patterns for building a Claude Code workflow kit. Each repo excels at a specific layer but none covers the full lifecycle.

## Repo evaluations (scored with /eval-tool rubric, 15 max)

### Tier 1: Extract patterns from

| Repo | Score | Best for | Extract |
|------|-------|----------|---------|
| GSD (gsd-build/get-shit-done) | 13/15 | Spec generation | .planning/ convention, atomic task breakdown, fresh subagent contexts per task, decision IDs |
| gstack (garrytan/gstack) | 12/15 | Review + QA | /office-hours 6 forcing questions, /review paranoid reviewer with completeness scoring, /ship release flow, /qa headless browser testing |
| Trail of Bits (trailofbits/claude-code-config) | 12/15 | Safety hooks | rm-rf blocker, push-to-main blocker, anti-rationalization Stop hook (prompt-type, Haiku evaluator), CLAUDE.md quality rules |
| ClaudeKit (mrgoonie/claudekit-skills) | 10/15 | Plan validation | /ck:plan validate interview gate, /ck:plan red-team (4 adversarial reviewers), /ck:bootstrap scaffolding |
| Context Hub (andrewyng/context-hub) | 11/15 | API docs | chub CLI for curated docs, annotation persistence across sessions, feedback loop to doc authors |

### Tier 2: Extract structural patterns only

| Repo | Score | Best for | Extract |
|------|-------|----------|---------|
| oh-my-claudecode (Yeachan-Heo) | 10/15 | Multi-agent orchestration | HUD/statusline (context budget display), ai-slop-cleaner (auto-simplify bloated code), notepad (compaction-resilient memory) |
| Claude-Code-Game-Studios (Donchitos) | 8/15 | Domain-specific template | Path-scoped rules (.claude/rules/ with YAML frontmatter), Collaborative Design Principle (Question > Options > Decision > Draft > Approval) |

## Key insight

The repos cluster into two categories: **methodology tools** (GSD, gstack, ClaudeKit) that provide commands for workflow phases, and **hardening tools** (Trail of Bits, OMC) that provide hooks for enforcement. No single repo covers both. The opportunity is the integration layer: commands for workflow + hooks for enforcement + one shared data format (.planning/SPEC.md).

## What to skip

| Repo | Why skip |
|------|----------|
| BMAD | Too heavy for solo dev. 12+ agent personas, sprint ceremonies. |
| Superpowers | Good but tightly coupled to its own skill framework. Subagent-driven-development is its core innovation but hard to extract. |
| Continuous-Claude | Impressive engineering (ledgers, handoffs, 20+ agents) but author admits "likely too many agents." |
| Conductor, Nimbalyst, Ruflo | L5 orchestration. Not needed until 3+ concurrent sessions. |