---
title: "Claude Code cost mechanics corrected for Opus 4.7 (April 2026)"
date: 2026-04-19
captured: 2026-04-19T09:49:23.540Z
tags: ["claude-code", "cost", "caching"]
source: "Claude.ai chat"
---
## TL;DR

The cost hierarchy in Claude Code is **cache hit rate > context size > model choice > effort level**, tuned top-down. The single rule worth internalizing: watch whether your cache survived, not your context size. Most older cost advice (the "stay under 200k" heuristic, "1M tier tax") is obsolete on Opus 4.7 and Sonnet 4.6 because both models now include 1M context at flat standard pricing.

![Cost hierarchy ranked by impact](https://assets.han-ws.workers.dev/i/2026/04/claude-code-cost-hierarchy.svg)

## The four levers, ranked

### 1. Cache hit rate — 10x swing, the dominant lever

Over 90% of tokens in heavy Claude Code sessions are cache reads. A cache hit costs 10% of the standard input price. Caching math:

- Cache write, 5-min TTL: 1.25x base input cost (pays off after 1 read)
- Cache write, 1-hour TTL: 2x base input cost (pays off after 2 reads)
- Cache read: 0.1x base input cost

Default TTL is 5 minutes. Idle gaps longer than that reset the cache and you rebuild from scratch on the next turn — this is the single most expensive mistake in a typical session.

**What invalidates cache:** `/compact`, `/clear`, model switches, and idle-out. Effort level changes do NOT invalidate cache.

### 2. Context size — linear, not stepped

Every turn re-reads the entire conversation as input. So per-turn input cost scales roughly linearly with context size.

**Important correction:** On Opus 4.7 and Sonnet 4.6, there is no 200k "1M tier tax." A 900k-token request bills at the same per-token rate as a 9k-token request. The old advice to "stay under 200k" was specific to pre-4.6 models and is obsolete as a pricing rationale, though it still works as a rough cache-survival heuristic (bigger contexts correlate with longer sessions and more likely cache loss).

### 3. Model choice — 5x fixed multiplier

| Model | Input $/MTok | Output $/MTok |
|---|---|---|
| Opus 4.7 | $5 | $25 |
| Sonnet 4.6 | $3 | $15 |
| Haiku 4.5 | $1 | $5 |

Opus is ~5x Sonnet across both input and output. Haiku is ~5x cheaper than Sonnet. Every current Claude tier keeps the same 5x output-to-input ratio.

### 4. Effort level — 1-5x on output only

Only affects output and thinking tokens, not input replay. Opus 4.7 introduced a new `xhigh` tier between `high` and `max`. **Claude Code now defaults to xhigh on all plans**, which means default per-output-token cost is higher than it was pre-4.7. If a task doesn't need deep reasoning, drop effort explicitly.

## When to switch to Sonnet (the power-user move)

Opus on default is quality-first. Sonnet is 5x cheaper with slightly less reasoning depth. Switch to Sonnet when:

- **Mechanical operations.** File moves, renames, running commands, applying a known diff. No reasoning novelty.
- **Batch/repetitive tasks.** Running `/commit-messages` 5 times in a row, reformatting N files, QA sweeps on a known flow.
- **Exploratory drafts that will be iterated.** First draft of a component, sketching an API shape, throwaway prototyping. Use Opus on the final version only.
- **Long sessions (context > 300k).** Routine work in a large context stacks context cost + Opus premium painfully. Sonnet here can cut costs 5x with minimal quality loss on routine work.
- **Subagents.** Subagents do focused, scoped work. Sonnet handles 90% of agent tasks fine.

Stay on Opus for architecture decisions, code review with subtle invariants, writing prose that matters (commit messages people will read, READMEs), debugging novel failures, and high-stakes decisions where "good enough" is expensive to fix.

**The power-user workflow:** Opus to plan, Sonnet to execute, Opus to review. Use `/model` mid-session to transition.

## Hidden gems worth knowing

1. **The Opus 4.7 tokenizer change.** Opus 4.7 ships with a new tokenizer that can produce up to 35% more tokens for the same input text. Rate card unchanged from 4.6, but real bill per request can go up. Training-data-based cost estimates miss this.

2. **Default effort is now xhigh in Claude Code.** Anthropic raised the default on all plans when 4.7 launched (April 16, 2026). If you don't need xhigh reasoning, downgrade explicitly.

3. **No 200k tier tax on 4.6/4.7.** 1M context included at standard pricing on both models. Cost function is smooth and linear in context size, not stepped.

4. **1-hour cache TTL is a real tool.** At 2x write cost, breakeven is 2 reads. If you know you'll be away for 10+ minutes and come back, the 1-hour TTL beats rebuilding.

5. **Subagents run on their own context budget.** Dispatching an explore agent for a codebase grep doesn't cost your main context — only the summary returns. For research-heavy tasks, this is a 10x+ lever.

6. **Workspace-level cache isolation (new Feb 5, 2026).** Caches are now isolated per workspace rather than per organization. Running Claude Code across multiple workspaces means caches don't cross.

7. **Batch API: 50% off on top of everything.** Not usable for interactive Claude Code, but free money for async subagent pipelines, bulk content generation, or scheduled processing.

8. **The ominous corner.** Opus + 1M context + max effort + poor cache hits + long session = easily $10-20 in a single session. Don't run all four knobs hot simultaneously without reason.

## The one rule

Watch whether your cache survived. Idle gaps > 5 minutes are the enemy. Context size only matters once the cache is cold. Everything else flows from that one awareness.

## Sources

- Anthropic pricing docs (platform.claude.com/docs/en/about-claude/pricing)
- Opus 4.7 launch announcement (April 16, 2026)
- Prompt caching docs (platform.claude.com/docs/en/build-with-claude/prompt-caching)