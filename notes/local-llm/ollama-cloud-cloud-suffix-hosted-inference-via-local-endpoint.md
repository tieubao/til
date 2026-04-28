---
title: "Ollama Cloud :cloud suffix: hosted inference via local endpoint"
date: 2026-04-28
captured: 2026-04-28T18:07:18.078Z
tags: ["ollama", "cloud", "inference", "hybrid"]
source: "Claude.ai chat"
---
## Definition

The `:cloud` suffix on Ollama model names (e.g., `deepseek-v4-pro:cloud`, `deepseek-v4-flash:cloud`) routes inference to Ollama's hosted infrastructure (NVIDIA Blackwell GPUs in the US) instead of running the model locally. The local Ollama daemon acts as an authorized proxy: requests still go to `localhost:11434`, but get forwarded to Ollama Cloud servers when the model name ends with `:cloud`.

## Context

This matters for hybrid local + cloud LLM setups. The same Ollama daemon serves both routes through the same endpoint, so client code (LangChain, Hermes Agent, OpenAI SDK pointed at localhost) doesn't need to know the difference. Switching from `qwen3.6:35b` (local) to `deepseek-v4-pro:cloud` (hosted) is just a model name change.

**Auth flow:** Sign in once with `ollama signin`, which pairs your machine via SSH key. The local daemon attaches auth headers when proxying `:cloud` requests. No API key juggling.

**Privacy guarantee:** Ollama states that cloud models are hosted through NVIDIA Cloud Providers (NCPs) with zero logging and zero data retention. Prompts are not stored or used for training.

**Pricing model:** Flat subscription, not per-token. Three tiers as of April 2026: Free ($0, daily quotas), Pro ($20/month, "day-to-day" usage), Max ($100/month, production-ish). Each tier has session limits that reset every 5 hours and weekly limits that reset every 7 days. No surprise overage bills, important when running Hermes Agent loops unattended.

**Why this is architecturally interesting:** Open-weight models like DeepSeek-V4 (MIT license) can be hosted by any provider. Ollama Cloud hosts them on US Blackwell while DeepSeek direct hosts them on Huawei Ascend in China. Same model, two infrastructure stacks, two jurisdictions, driven entirely by the open weights.

## Example

Available cloud models in Ollama as of April 2026:
- `deepseek-v4-flash:cloud`: 284B total / 13B active, 1M context
- `deepseek-v4-pro:cloud`: 1.6T total / 49B active, 1M context
- `qwen3.6:35b-a3b-coding-bf16:cloud`
- (other tags as Ollama adds them)

Usage in code:
```bash
# Same endpoint as local
curl http://localhost:11434/api/chat -d '{
  "model": "deepseek-v4-pro:cloud",
  "messages": [{"role": "user", "content": "Hello!"}]
}'
```

Usage with Hermes Agent (built-in launch shortcut):
```bash
ollama launch hermes --model deepseek-v4-pro:cloud
```

This pattern means you can A/B between local and cloud routes without changing application code. Only the model name string changes. Useful for hybrid setups where local handles 80% and cloud handles escalations.

## When to choose Ollama Cloud over OpenRouter

- Heavy V4-Pro usage where flat $20-100/month beats pay-per-token (breakeven ~10M tokens/month at post-May-5 pricing)
- Want unified `localhost:11434` endpoint with no separate API key management
- Privacy concerns about routing through OpenRouter's middleware layer
- Avoiding surprise bills from runaway agent loops

## When to stay on OpenRouter

- Light usage where $20/month subscription wastes money
- Need access to models not on Ollama Cloud (Gemma 4, Claude, GPT, Qwen3.6-35B-A3B as a paid route)
- Want a single billing dashboard across many providers
- Need provider-specific features (function calling parsers, JSON mode, prefix caching)

Source: Ollama documentation, ollama.com/pricing, model library pages on ollama.com, verified April 28, 2026.

## Related

- [[local-llm-hybrid-stack-ollama-ollama-cloud-openrouter-for-hermes-agent]] - the architecture decision where this `:cloud` suffix is the escalation lever
- [[llm-api-pricing-comparison-deepseek-direct-vs-ollama-cloud-vs-openrouter-april-2]] - where Ollama Cloud sits in the broader pricing matrix
- [[qwen3-6-35b-a3b-on-m4-pro-memory-budget-and-context-sizing]] - the local-side counterpart: same daemon, no `:cloud` suffix
- [[hermes-agent-fixed-overhead-13-9k-tokens-per-api-call]] - the workload pattern that values flat-rate over per-token