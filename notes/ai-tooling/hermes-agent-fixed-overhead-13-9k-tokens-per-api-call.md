---
title: "Hermes Agent fixed overhead: 13.9K tokens per API call"
date: 2026-04-28
captured: 2026-04-28T18:05:27.744Z
tags: ["hermes", "tokens", "cost", "agentic"]
source: "Claude.ai chat"
---
Hermes Agent v0.6+ adds **~13,935 tokens of fixed overhead per API call**, regardless of which model or provider is used. This was measured by analyzing request dumps from `~/.hermes/sessions/` on a real deployment.

**Per-call breakdown:**

| Component | Tokens | % of avg request |
|---|---|---|
| Tool definitions (31 tools) | 8,759 | 46.1% |
| System prompt (SOUL.md + skills catalog) | 5,176 | 27.2% |
| Messages (conversation context) | 3,000 to 8,775 | 26.7% avg |
| **Total per request** | **~17,000 to 23,000** | |

The 73% fixed-overhead ratio means a one-line message ("list files") still requires the model to process ~17K tokens before responding.

**What this implies for cost:**

Token volume scales with **call count, not session count**. Every Hermes tool call is a fresh API round trip carrying the full overhead. A 10-call coding task costs the same in fixed overhead as 10 separate one-shot questions.

Real-world burn from one user's monitoring dashboard:
- 3 active gateway sessions in one evening: ~3.9M input tokens across ~207 API calls
- Feature implementation (100 calls): ~4M tokens
- Large refactor (500 calls): ~25M tokens
- Full project build (1000 calls): ~60M tokens

**Mitigation patterns:**

1. **Trim toolset per platform.** All 31 core tools load on every platform by default. CLI keeps full surface, but messaging gateways (Telegram, WhatsApp) don't need 11 `browser_*` tools (saves ~1,258 tokens/call).
2. **Drop skills catalog from system prompt.** Skills are accessible on-demand via `skill_view`/`skills_list`. Lazy loading saves ~2,200 tokens/call.
3. **Tighten compression defaults.** `compression.threshold: 0.5` is conservative for messaging. Drop to `0.3` with `protect_last_n: 10` to compress earlier.
4. **Pick cheap-input models.** Hermes overhead is mostly input. The cheaper-input models (Gemma 4 at $0.06/M, V4-Flash at $0.14/M) dominate Qwen3.6-27B ($0.33/M input) for Hermes specifically, even though output ratios suggest otherwise.
5. **Use prompt caching.** The 13.9K fixed overhead is identical across calls in a session. DeepSeek's cache-hit pricing drops to 1/10 ($0.014/M). Conversational workloads typically achieve 65-70% cache-hit rates.

**The bigger lesson:** before optimizing model choice, measure actual token consumption. The hermes-dashboard utility (built into v0.11) uses `/usage` and `/insights` to surface real numbers. Optimize what you measure, not what you guess.

Source: GitHub issue NousResearch/hermes-agent#4379, monitoring dashboard analysis April 2026.