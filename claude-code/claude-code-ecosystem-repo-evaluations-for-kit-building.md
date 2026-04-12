---
title: "Claude Code ecosystem repo evaluations for kit building"
date: 2026-03-29
captured: 2026-03-29T08:04:39.329Z
tags: ["claude-code", "sdd", "tooling", "evaluation"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
aliases: []
status: refined
---
Evaluated 7 repos to identify patterns for building a unified Claude Code workflow kit. Scored each with a 5-criterion rubric (layer fit, credibility, adoption cost, pain match, timing), max 15 points.

## Tier 1: Extract patterns from (scored 10+)

| Repo | Score | What to extract | What to skip |
|------|-------|----------------|-------------|
| GSD (gsd-build/get-shit-done) | 13/15 | `.planning/` directory convention, atomic task breakdown (each fits 50% context), decision IDs for traceability | GSD v2's Pi SDK runtime, model routing profiles |
| gstack (garrytan/gstack) | 12/15 | `/office-hours` 6 forcing questions, `/review` paranoid reviewer with 0-10 scoring, `/ship` release flow | `/qa` browser testing (requires Playwright + Bun) |
| Trail of Bits (trailofbits/claude-code-config) | 12/15 | rm-rf blocker, push-to-main blocker, anti-rationalization Stop hook, CLAUDE.md quality rules | Full security audit skills (install separately) |
| Context Hub (andrewyng/context-hub) | 11/15 | `chub` CLI skill for curated API docs, annotation persistence | The CLI implementation itself (it's a dependency, not to rebuild) |
| ClaudeKit (mrgoonie/claudekit-skills) | 10/15 | `/ck:plan validate` interview gate, `/ck:plan red-team` 4 adversarial reviewers | Paid Engineer Kit features, 50+ commands (too broad) |

## Tier 2: Extract structural patterns only (scored 8-9)

| Repo | Score | Patterns worth extracting | Why not Tier 1 |
|------|-------|--------------------------|---------------|
| oh-my-claudecode (Yeachan-Heo, 9.7k stars) | 10/15 | HUD/statusline, ai-slop-cleaner, notepad memory | 37 skills + 28 agents is too much. Extract 3 specific patterns. |
| Claude-Code-Game-Studios (Donchitos, 7k stars) | 8/15 | Path-scoped rules (.claude/rules/), Collaborative Design Principle | Game-dev specific. 48 agents for a domain we don't need. |

## The insight that shaped the kit

These repos cluster into two categories: **methodology tools** (GSD, gstack, ClaudeKit) providing commands for workflow phases, and **hardening tools** (Trail of Bits, OMC) providing hooks for enforcement. No single repo covers both. The integration gap between them is where dwarves-kit lives: commands for workflow + hooks for enforcement + one shared data format (.planning/SPEC.md).

## What to skip entirely

| Repo | Why skip |
|------|----------|
| BMAD | 12+ agent personas, sprint ceremonies. Overkill for solo dev + contractors. |
| Superpowers | Tightly coupled to its own skill framework. Hard to extract. |
| Continuous-Claude v3 | 20+ agents. Author admits "likely too many." Study the patterns, don't adopt. |
| Conductor, Nimbalyst, Ruflo | L5 orchestration. Not needed until 3+ concurrent sessions. |

## The evaluation rubric (reusable)

| Criterion | 3 points | 2 points | 1 point |
|-----------|----------|----------|---------|
| Layer fit | Fills a weak layer | Partial overlap | Heavy overlap |
| Credibility | Reputable org or 5k+ stars | Known dev, 1k+ | Unknown, <500 |
| Adoption cost | Under 15 min to try | 15-60 min | 1+ hours |
| Pain match | Fixes a known failure | Addresses a theoretical gap | No current pain |
| Timing | Working in this area now | Relevant next month | Not relevant for 30+ days |

Score: 12-15 = ADOPT, 8-11 = BOOKMARK, 5-7 = SKIP. Never adopt more than 1 tool per week.

## Related

- [[tool-evaluation-5-question-rubric]] - the 5-question scoring rubric used to evaluate these repos
- [[building-dwarves-kit-from-extracted-patterns]] - the synthesis process that consumed these evaluations
- [[claudekit-evaluation-and-unique-features]] - detailed evaluation of one of the Tier 1 repos (ClaudeKit)
- [[ai-dev-stack-8-layer-model-march-2026]] - the 8-layer stack model that frames where each repo sits