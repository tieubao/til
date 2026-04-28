---
title: "Local LLM hybrid stack: Ollama + Ollama Cloud + OpenRouter for Hermes Agent"
date: 2026-04-28
captured: 2026-04-28T18:05:00.494Z
tags: ["ollama", "hermes", "apple-silicon", "architecture"]
source: "Claude.ai chat"
---
## Context

Running an agentic workflow (Hermes Agent) on a 64 GB Apple Silicon Mac mini. Need a setup that respects three priorities: privacy and offline capability for sensitive work, low cost on routine tasks, and access to frontier-class models for hard reasoning.

The competing approaches:
- Pure local: privacy + zero marginal cost, but slow tok/s and capability ceiling
- Pure API: fast and capable, but cost and privacy concerns
- Pure Claude/Sonnet: high quality, expensive at agentic-loop volume ($300+/month)

Hermes Agent fires 13-17K tokens of overhead per call (tool definitions + system prompt + skills catalog), so token volume scales with call count not session count. This makes the model choice meaningfully different from a chat assistant.

## Decision

Local-first hybrid with three escalation tiers:

| Priority | Backend | Model | Use case |
|---|---|---|---|
| Default | Local Ollama | Qwen3.6-35B-A3B Q4 | Daily work, privacy-sensitive, offline |
| Escalate | Ollama Cloud | DeepSeek-V4-Pro `:cloud` | Hard reasoning, when local fails tool calls or context fills |
| Specialized | OpenRouter | Gemma 4 26B-A4B | Multilingual, vision/OCR tasks |
| Volume fallback | OpenRouter | DeepSeek-V4-Flash | Cheap fast inference when Ollama Cloud rate-limits |
| Context fallback | OpenRouter | Qwen3.6-35B-A3B | Repo-scale reasoning beyond 128k local context |

Ollama 0.19+ uses MLX as the native Apple Silicon backend, so "Ollama vs MLX server" is no longer a meaningful choice. Ollama IS MLX now.

The `:cloud` suffix on Ollama models proxies to Ollama's hosted infrastructure (Blackwell GPUs, US-hosted, zero-retention) through the same `localhost:11434` endpoint. To Hermes, switching from local to cloud is just a model name change.

## Alternatives considered

**Pure local with multiple models loaded.** Rejected. With ~48 GB usable on 64 GB unified memory, two large chat models (24 GB + 17 GB) leave 7 GB headroom. Any GUI load triggers swap to SSD. Tok/s craters from 40 to 5. Stick to one chat model plus optional embeddings model.

**Local with llama.cpp/MLX directly instead of Ollama.** Rejected. Ollama 0.19+ runs MLX natively. Direct MLX server has worse OpenAI-compat, separate vision adapter (`mlx-vlm`), and triple the maintenance overhead. The 10-15% throughput gain isn't worth losing tool-call reliability and bundled vision.

**DeepSeek direct API instead of OpenRouter for V4-Flash.** Rejected. DeepSeek's hosted endpoint runs in China with documented 503 errors during peak demand. For agentic loops with 50-150 calls per session, even occasional 503s break the loop. OpenRouter routes through US-based providers with better reliability.

**Subscribe to Ollama Cloud Pro upfront.** Deferred. At light-to-moderate Hermes usage (40-150 calls/day), DeepSeek-V4-Pro on OpenRouter pay-per-token costs $10-37/month during the May 5 promo. Subscription only pays back at heavy usage. Start free tier, evaluate after a month.

**Pure Claude Sonnet 4.6 via Hermes.** Rejected. At moderate Hermes usage that's ~$300/month, vs ~$15/month for the hybrid path. Reserve Claude for direct Claude Code work, not Hermes loops.

## Consequences

**Positive:**
- ~$0-15/month total cost at moderate usage vs $200-400/month pure-Claude
- Privacy preserved by default. Local handles 80%+ of work
- Vision and multimodal available on the local path (Qwen3.6 has built-in mmproj)
- Single Ollama daemon orchestrates both local and cloud routes
- $50/month OpenRouter spending cap acts as a hard ceiling against runaway loops

**Negative:**
- Local Qwen tops out at ~40-50 tok/s vs 80+ on API. Interactive sessions feel slower.
- Cold-start prefill on a fresh Hermes session takes 60-90 seconds (model load plus ~13K overhead tokens to compute attention for).
- Need to remember escalation triggers (context exhaustion, tool-call failures) and switch manually.
- Five model routes is more cognitive overhead than one.

**Cloud-first contingency:** If local is unavailable (Mac in repair, traveling without it), subscribe to Ollama Cloud Pro $20/month and switch default to `pro-cloud`. Cancel when local is back online.

## Routing rules

Stay on local default unless one of these triggers fires:
- **Context exhaustion**: local hits 128k limit, switch to `pro-cloud` for the rest of the session
- **Tool-call failure**: two consecutive tool calls fail or produce malformed output, escalate to `pro-cloud`
- **Hard reasoning task**: spec-driven dev, repo-level analysis, multi-file refactor, start session on `pro-cloud` directly
- **Multilingual content**: Vietnamese, translation, multilingual work, use `gemma-multilingual` from start
- **Vision/OCR**: images, diagrams, screenshots, use `gemma-multilingual`
- **Sensitive code**: stay on local regardless of other factors

Principle: escalate early, escalate decisively. Don't fight local for 20 minutes when escalating after 5 minutes was the right call.

## Related

- [[qwen3-6-35b-a3b-on-m4-pro-memory-budget-and-context-sizing]] - the local default tier: memory math, context sizing, prefill cost
- [[ollama-cloud-cloud-suffix-hosted-inference-via-local-endpoint]] - the escalation mechanism: same `localhost:11434` endpoint serves both routes
- [[llm-api-pricing-comparison-deepseek-direct-vs-ollama-cloud-vs-openrouter-april-2]] - the cost reasoning behind the three-tier choice
- [[hermes-agent-fixed-overhead-13-9k-tokens-per-api-call]] - the overhead figure that makes input-cost the dominant variable
- [[hermes-agent-comprehensive-briefing-april-2026]] - the runtime consuming this stack