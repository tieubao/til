---
title: "Hermes Agent v0.13.0 release evaluation: top features ranked for 3-tier ecosystem"
date: 2026-05-09
captured: 2026-05-09T17:48:19.314Z
tags: ["hermes", "agents", "evaluation", "architecture", "ai-tooling"]
source: "Claude.ai chat"
---
# Hermes Agent v0.13.0 release evaluation: top features ranked for 3-tier ecosystem

Evaluation of NousResearch Hermes Agent v0.13.0 (v2026.5.7, "The Tenacity Release") features ranked by impact for a 3-tier Hermes ecosystem (ops/briefing/trading agent on Vultr VPS, Telegram interface, Go-based tooling).

## Rubric

Each feature scored across three dimensions (1-10):
- **Architectural impact** — does it change how agents are designed (state, memory, control flow)?
- **Operational impact** — does it reduce token cost, increase reliability, or reduce ops burden?
- **Strategic relevance** — should this be adopted/cloned into a custom 3-tier Hermes ecosystem?

## Top 5 features

![Hermes v0.13.0 impact matrix](https://assets.han-ws.workers.dev/i/2026/05/hermes-v0-13-impact-matrix.svg)

| Feature | Architect | Ops | Strategic | Action |
|---------|-----------|-----|-----------|--------|
| 1. Multi-agent Kanban (durable) | 9 | 9 | 10 | CLONE |
| 2. `/goal` Ralph loop | 10 | 7 | 10 | ADOPT |
| 3. Security wave (8 P0 closures) | 5 | 10 | 9 | STUDY |
| 4. Session auto-resume | 7 | 9 | 9 | ADOPT |
| 5. Cron `no_agent` mode | 6 | 10 | 9 | CLONE |

## Deep dive

### 1. Multi-agent Kanban (durable) — CLONE

Mechanism: durable board, multiple workers pick tasks, heartbeat per worker, board reclaims tasks from dead workers, zombie detection (darwin), auto-block on incomplete exit, per-task `max_retries` budget, hallucination gate that rejects worker-claimed cards without evidence.

This is the highest-substance feature in the release. The kanban concept is old but the combo *durable + heartbeat + zombie reclaim + retry budget* turns it into a job queue with observability. It is the design pattern a custom 3-tier Hermes ecosystem most needs.

Architectural shift it enables:
- Current model: 3 monolithic tiers (ops/briefing/trading), each handling its own queue.
- With Kanban pattern: 3 worker pools reading a shared board. Tasks tagged `tier:ops` go to ops workers. Heartbeat + reclaim solves Vultr VPS restart and OOM scenarios.

Reference PRs: [#17805](https://github.com/NousResearch/hermes-agent/pull/17805), [#21183](https://github.com/NousResearch/hermes-agent/pull/21183), [#21214](https://github.com/NousResearch/hermes-agent/pull/21214).

Implementation note: pattern is portable. Postgres + Go workers (existing stack) + Notion as UI is sufficient. No need to use Hermes code directly.

### 2. `/goal` Ralph loop — ADOPT

Persistent cross-turn goals via file-based state (Ralph loop pattern). Already covered in detail in [[ralph-loop-pattern-explained-persistent-goals-via-file-based-state]].

Release additions:
- PR [#21287](https://github.com/NousResearch/hermes-agent/pull/21287) adds turn budget cap to prevent unbounded token spend.
- This is the foundation for everything else. Implement first before Kanban so tasks don't drift mid-execution.

### 3. Security wave (8 P0 closures) — STUDY

Critical for any agent deployed on a public VPS with messaging integrations. Concrete impacts:

- **Redaction default ON** ([#21193](https://github.com/NousResearch/hermes-agent/pull/21193)) — prevents secrets leaking into logs/chat. Audit own stack for similar patterns.
- **Discord guild-scoped allowlist (CVSS 8.1)** ([#21241](https://github.com/NousResearch/hermes-agent/pull/21241)) — `DISCORD_ALLOWED_ROLES` previously checked role name without scoping to originating guild. Cross-guild bypass possible. Audit any custom Discord role checks.
- **TOCTOU fixes in `auth.json` and MCP OAuth** ([#21176](https://github.com/NousResearch/hermes-agent/pull/21176), [#21194](https://github.com/NousResearch/hermes-agent/pull/21194)) — race condition between stat() and write() exploitable for credential injection. Pattern: any code path that checks then writes a file may have this bug.
- **Cloud metadata SSRF floor** ([#21228](https://github.com/NousResearch/hermes-agent/pull/21228)) — browser tool blocks requests to `169.254.169.254`. Critical for any agent with browser tool running on AWS/Vultr/cloud VMs.

Action: not adopt, but audit own stack for the same vulnerability classes.

### 4. Session auto-resume — ADOPT

Mechanism: gateway restart mid-conversation → on restart, session resumes from last checkpoint, agent replies as if nothing happened.

Tied to **Checkpoints v2** ([#20709](https://github.com/NousResearch/hermes-agent/pull/20709)) — single-store rewrite with real pruning + disk guardrails. Previous checkpoint system grew unbounded (orphan shadow repos), now has proper LRU.

Combine with `/goal`: goal lives in persistent storage, session lives in checkpoint store, gateway restart loses nothing.

### 5. Cron `no_agent` mode — CLONE

Mechanism: cron jobs can skip LLM entirely, run script only. Empty stdout = silent. Non-empty stdout = deliver verbatim via messaging platform.

This is the highest token-saving feature in the release. Watchdog patterns (disk space, endpoint ping, crypto price threshold) previously called LLM once for output formatting → ~5-10K tokens per run. With `no_agent`, bash script formats, agent only routes.

Concrete applications for trading/briefing/ops:
- **Trading watchdog**: `/cron 'every 5 minutes' bash check_btc_price.sh` → silent unless threshold crossed. Zero token cost on 99% of runs.
- **Briefing kickoff**: script pulls Notion data first, only feeds LLM when there is content to summarize.
- **VPS health checks**: integrate vps-mon style checks → Telegram alerts via Hermes routing.

Reference: PR [#19709](https://github.com/NousResearch/hermes-agent/pull/19709).

## Hype/marketing watch

Features that got marketing weight but have low real impact:

- **i18n 7 locales** — static gateway/CLI message translation only. Agent still operates in English. Marketing positions as "Hermes speaks your language" but agent behavior is unchanged.
- **Google Chat = 20th platform** — counter increment. Feature parity with 19 existing platforms, no architectural novelty.
- **100 new CLI tips + `default-large` theme** — cosmetic.
- **`video_analyze` tool** — wrapper on Gemini multimodal API. Direct Gemini call works equivalently.
- **xAI Custom Voices** — capability addition, no architectural change.
- **6 new optional skills** (Shopify, here.now, shop-app) — vendor-specific, only relevant if using those vendors.

## Verdict: Selective adoption

Do not migrate to Hermes Agent wholesale. Adopt patterns selectively:

1. **`/goal` Ralph pattern** — implement first. Foundation for goal persistence.
2. **Cron `no_agent` mode** — refactor watchdog patterns to bash + Telegram delivery, skip LLM. Highest token cost ROI.
3. **Kanban architecture** — refactor 3-tier into worker pools reading shared board. Larger project, do after `/goal` is stable.
4. **Security audit** — TOCTOU patterns and Discord guild-scope checks if Discord integration exists.
5. **Session auto-resume** — depends on checkpoint v2 design. Implement after persistent goal storage.

Skip or low priority: i18n, Google Chat, video tool, voice cloning, dashboard cosmetics.

## Source

Release notes: https://github.com/NousResearch/hermes-agent/releases/tag/v2026.5.7

Release date: May 7, 2026
Stats: 864 commits, 588 merged PRs, 829 files changed, 282 issues closed (13 P0, 36 P1).

## Related

- [[hermes-agent-comprehensive-briefing-april-2026]] - the prior baseline; what Hermes was before v0.13.0
- [[hermes-agent-fixed-overhead-13-9k-tokens-per-api-call]] - cost analysis that makes the "cron no_agent mode = highest ROI" verdict concrete
- [[ralph-loop-pattern-explained-persistent-goals-via-file-based-state]] - foundational pattern behind the `/goal` feature; implement first
- [[hermes-vs-openclaw-competitive-scene-april-2026]] - competitive context for the selective-adoption stance
- [[why-developers-migrate-to-hermes-ranked-real-vs-hype]] - adoption framing; aligns with "do not migrate wholesale"