---
title: "Claude Code ecosystem repo evaluations for kit building"
date: 2026-03-29
captured: 2026-03-29T07:48:39.230Z
tags: ["claude-code", "sdd", "tooling", "evaluation"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
---
Evaluated 7 repos to identify patterns for building a unified Claude Code workflow kit. Scored each with a 5-criterion rubric (layer fit, credibility, adoption cost, pain match, timing), max 15 points.

## Tier 1: Extract patterns from (scored 10+)

| Repo | Score | What to extract | What to skip |
|------|-------|----------------|-------------|
| GSD (gsd-build/get-shit-done) | 13/15 | `.planning/` directory convention, atomic task breakdown (each fits 50% context), decision IDs for traceability, fresh subagent contexts per task | GSD v2's Pi SDK runtime (different product), model routing profiles |
| gstack (garrytan/gstack) | 12/15 | `/office-hours` 6 forcing questions, `/review` paranoid reviewer with completeness 0-10 scoring, `/ship` release flow, TODOS format | `/qa` browser testing (requires Playwright + Bun binary), design system features |
| Trail of Bits (trailofbits/claude-code-config) | 12/15 | rm-rf blocker, push-to-main blocker (both verbatim usable), anti-rationalization Stop hook (prompt-type, Haiku evaluator), CLAUDE.md quality rules ("no phantom features, replace don't deprecate") | Full security audit skills (install separately via plugin marketplace) |
| Context Hub (andrewyng/context-hub) | 11/15 | `chub` CLI skill for curated API docs, annotation persistence across sessions, feedback loop to doc authors | The full CLI implementation (it's a dependency, not something to rebuild) |
| ClaudeKit (mrgoonie/claudekit-skills) | 10/15 | `/ck:plan validate` interview gate, `/ck:plan red-team` 4 adversarial reviewers (security, failure mode, assumption destroyer, scope critic) | Paid Engineer Kit features, 50+ commands (too broad, unclear scope) |

## Tier 2: Extract structural patterns only (scored 8-9)

| Repo | Score | Patterns worth extracting | Why not Tier 1 |
|------|-------|--------------------------|---------------|
| oh-my-claudecode (Yeachan-Heo) | 10/15 | HUD/statusline (real-time context budget), ai-slop-cleaner (auto-simplify bloated code on Stop), notepad (compaction-resilient memory) | 37 skills + 28 agents is too much. Author admits "likely too many agents." Extract 3 specific patterns. |
| Claude-Code-Game-Studios (Donchitos) | 8/15 | Path-scoped rules (.claude/rules/ with YAML frontmatter), Collaborative Design Principle (Question > Options > Decision > Draft > Approval) | Game-dev specific. 48 agents for a domain we don't work in. Structural ideas are transferable. |

## The insight that shaped the kit

These repos cluster into two categories: **methodology tools** (GSD, gstack, ClaudeKit) providing commands for workflow phases, and **hardening tools** (Trail of Bits, OMC) providing hooks for enforcement. No single repo covers both. The integration gap between them is where dwarves-kit lives.

## What to skip entirely

| Repo | Why skip |
|------|----------|
| BMAD | 12+ agent personas, sprint ceremonies. Overkill for solo dev + contractors. |
| Superpowers | Good but tightly coupled to its own skill framework. Subagent-driven-development is hard to extract without adopting the whole system. |
| Continuous-Claude v3 | Impressive engineering (ledgers, 20+ agents). Too complex to maintain. Study the patterns, don't adopt the system. |
| Conductor, Nimbalyst, Ruflo | L5 orchestration. Not needed until 3+ concurrent Claude Code sessions. |

## The evaluation rubric

Reusable for any future tool evaluation:

| Criterion | 3 points | 2 points | 1 point |
|-----------|----------|----------|---------|
| Layer fit | Fills a weak layer in your stack | Partial overlap | Heavy overlap with existing tools |
| Credibility | Reputable org or 5k+ stars | Known dev, 1k+ stars | Unknown solo dev, <500 stars |
| Adoption cost | Under 15 min to try | 15-60 min | 1+ hours |
| Pain match | Fixes a known failure from last 3 projects | Addresses a theoretical gap | No current pain |
| Timing | Working in this area right now | Will be relevant next month | Not relevant for 30+ days |

Verdict: 12-15 = ADOPT, 8-11 = BOOKMARK, 5-7 = SKIP. Never adopt more than 1 tool per week.