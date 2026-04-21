---
title: "TurboQuant KV cache compression"
date: 2026-04-18
captured: 2026-04-18T16:23:46.201Z
tags: ["llm-infra", "inference", "quantization", "kv-cache"]
source: "Claude.ai chat"
---
TurboQuant is an LLM inference memory compression algorithm from Google Research, published at ICLR 2026 by Zandieh, Daliri, Hadian, and Mirrokni (arXiv 2504.19874). It compresses the KV cache from 16 bits per value down to 3-4 bits with near-zero quality loss. Roughly 5x compression.

Nothing to do with Hermes Agent, OpenClaw, or any agent framework. Pure inference-infrastructure breakthrough. But it's big: Jensen Huang spent much of GTC 2026 calling KV cache memory the number one bottleneck for long-context inference, and TurboQuant is the most-talked-about answer. The "cache" in the name is the KV cache specifically, not a generic cache layer.

## Why KV cache matters

When a transformer generates text, every attention layer stores a key vector and a value vector for each token it's seen so it doesn't have to recompute them. This is the KV cache. It scales linearly with context length, and for long contexts it can exceed the memory footprint of the model weights themselves.

| Config | KV cache size |
|--------|---------------|
| 8B model at 32K context | ~4.6 GB |
| 70B model at 32K context | 20-40 GB |
| Very long contexts (128K, 1M tokens) | Serious hardware territory |

This is the bottleneck that stops you from running long-context models locally, serving more concurrent users per GPU, or extending context windows cheaply.

## The insight in plain terms

Existing quantization approaches (FP8, INT4) round the numbers in the cache. Problem: KV values are spiky. Most are small, a few are huge outliers. When you round everything to the same low-bit grid, the outliers get massively distorted and attention quality collapses.

The obvious fix ("use more bits for big numbers, fewer for small numbers") breaks because GPUs want uniform bit widths to run efficiently.

**TurboQuant's trick**: apply a random orthogonal rotation to the vectors first. Pure linear algebra: rotating a vector doesn't change its length or relationships with other vectors, but it redistributes values across dimensions. Spiky outliers get smeared evenly across coordinates. The rotated values now follow a concentrated Beta distribution, which is well-behaved and quantizes cleanly with standard scalar quantizers.

**Second trick for inner product preservation**: MSE-optimal quantizers (good for distance) introduce bias in dot products (what attention actually uses). TurboQuant's two-stage design applies an MSE quantizer first, then a 1-bit Quantized JL (QJL) correction on the residual. The combined estimator is unbiased for inner products.

**Key property for deployment**: data-oblivious and online. No calibration dataset needed, no offline preprocessing step. The random rotation is fixed ahead of time, quantization happens per-vector as the cache fills. This is why it can be a drop-in replacement rather than a retraining project.

## What this unlocks in practice

From community implementations and benchmarks:

- 5-7x compression at 3.5 bits per value with near-identical quality to FP16
- At 2.5 bits per value, still holds up and is *faster* than FP16 at decode (less memory bandwidth moving the cache around)
- Works on top of existing architectural improvements (GQA, DeltaNet) because they address different parts of the memory budget
- Extends beyond LLMs: the same algorithm works for vector search and nearest-neighbor retrieval on high-dimensional embeddings

## Adoption path

- Paper: arXiv 2504.19874, ICLR 2026 poster April 25, 2026
- Google hasn't released an official implementation
- Community implementations: `turboquant` on PyPI (HuggingFace drop-in), `0xSero/turboquant` (Triton kernels + vLLM), `OnlyTerp/turboquant`, `tonbistudio/turboquant`
- SGLang PR #21954 in flight, composes with NVFP4 on Blackwell GPUs
- llama.cpp fork with ROCm support: `--cache-type-k turbo4 --cache-type-v turbo4`
- LMCache blog published layman's explanation April 15

## One credibility asterisk

Public academic dispute. Jianyang Gao (ETH Zurich, formerly NTU) publicly complained that TurboQuant's core technique overlaps significantly with his RaBitQ work (SIGMOD 2024 and 2025), that the TurboQuant authors were informed but moved the RaBitQ discussion from the main text to the appendix in the final ICLR version. Google promoted TurboQuant heavily, reaching tens of millions of views on social media. The accusation isn't that TurboQuant is fake, it's that the novelty framing is overclaimed and prior art was under-credited.

Doesn't change whether the algorithm works. Does mean the "most significant AI breakthrough of the year" framing on X is inflated. Treat TurboQuant as a genuine and useful piece of infrastructure, not a miracle.

## Relevance to consumer-API-driven stacks

Directly: almost none. If you run Claude through the API or MiMo through Nous Portal, those providers handle inference and will adopt KV cache quantization internally when it makes commercial sense. You won't be configuring TurboQuant flags on your end.

Indirectly, three reasons to track it:

1. **Local model viability improves.** TurboQuant and similar techniques (KIVI, KVQuant, KVTC, PolarQuant, BalanceKV) are the reason a 70B-class model might actually fit in consumer VRAM with long context. Relevant for privacy-sensitive or offline use cases.
2. **Inference cost drops.** API prices for long-context usage should come down as providers deploy these techniques.
3. **The math is approachable for a CS408-level path.** Built on random rotations, Beta distributions, and Johnson-Lindenstrauss projections. If following linear algebra and probability into computational finance, this is a clean example of theoretical CS tools (vector quantization, JL transform, discrepancy theory) getting weaponized for practical LLM infra. Good ICLR read.

## Related

- [[transformer-internals-for-software-engineers-ffn-as-graph-database-larql]] - counterpart that targets FFN weights; this note targets attention KV cache
- [[llm-agent-memory-systems-landscape-2026]] - downstream use case: long-context agent memory becomes cheap when KV quantization holds
- [[llm-memory-systems-three-competitive-battlegrounds]] - why infra improvements like this reshape what memory architectures are viable