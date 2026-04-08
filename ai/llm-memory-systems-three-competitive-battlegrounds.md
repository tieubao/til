---
title: "LLM memory systems three competitive battlegrounds"
date: 2026-04-08
captured: 2026-04-08T02:00:45.892Z
tags: ["ai", "memory", "agents", "competition", "architecture"]
source: "Claude.ai chat"
---
## Overview

All LLM memory systems compete on variations of "use an LLM to decide what to remember and how to update it." The real differentiation is in the engineering around that core idea. The competition concentrates on three specific technical battlegrounds, with the write/update loop absorbing roughly 80% of the innovation.

## Battleground 1: The write/update loop (where 80% of the competition is)

This is the hottest area. The question: when new information arrives that may conflict with existing memory, who decides what happens?

**Mem0 (LLM-as-judge):** Runs the AUDN cycle. For each extracted candidate fact, vector search finds top-k similar memories, then the LLM picks a tool: ADD, UPDATE, DELETE, or NOOP. The entire conflict resolution is delegated to the LLM via a tool-calling prompt. No product-level orchestration wraps this decision. If the LLM picks wrong (NOOP instead of UPDATE), the contradiction persists silently. Fast and simple.

**Letta (agent self-management):** The agent itself manages memory through tool calls (`core_memory_replace()`, `archival_memory_search()`). There's no separate memory pipeline. More transparent because you can inspect memory blocks, but completely dependent on the model's tool-calling ability. The Letta Leaderboard exists precisely to test which models can reliably do this. Weaker models forget to call the update tool.

**A-Mem (never delete, link everything):** Zettelkasten-inspired. New facts create new notes. Related notes get bidirectional links. A memory evolution module generates higher-order notes that synthesize related facts. Nothing gets deleted. Better for temporal questions ("when did you change your diet?") but worse for token efficiency since memory grows indefinitely with no pruning.

**MemOS (temporal priority):** Newer fragments with overlapping semantic keys supersede older ones in the MemCube module. Lifecycle metadata (created, updated, validity) tracks freshness. Most production-grade approach to staleness, but requires model-level integration.

The pattern across all four: everyone delegates the hard decision to an LLM. The differentiation is in how much structure wraps that decision.

## Battleground 2: Retrieval strategy (the latency vs accuracy tradeoff)

This is where the business case lives. The hard numbers matter.

**Single-hop vector search (Mem0 default):** Embed the query, cosine similarity, return top-k. Handles "what's my diet preference?" type questions. Mem0's selective pipeline accepts a 6-percentage-point accuracy trade against full-context in exchange for 91% lower p95 latency (1.44s vs 17.12s) and 90% fewer tokens. The 17-second tail latency of full-context means one in twenty users waits 17 seconds, at 14x the token cost.

**Graph traversal (Mem0g, Zep):** For multi-hop queries like "what restaurant did my friend's colleague recommend?" where you need to hop across entity relationships. Mem0g's graph variant scores 68.4% LLM Score vs 66.9% for vector-only on complex questions, but costs 2.59s p95 vs 1.44s. Enable graph when use case involves complex entity relationships (medical contexts, enterprise hierarchies, technical system interdependencies). For simpler personalization, vector-only is adequate.

**Agent-driven iterative search (Letta):** The agent decides what to search for next based on what it found. A simple agent using `search_files` iteratively achieved 74% on LoCoMo with GPT-4o mini, beating Mem0's reported 68.5% for their top-performing graph variant. More expensive per query, but catches things that single-pass retrieval misses. This works because models post-trained on coding tasks are highly effective at filesystem operations.

**Multi-stage rerank (MemPalace, others):** Embed first, then LLM rerank. MemPalace's claimed 100% was achieved by setting top_k=50 against max 32 sessions (retrieving everything then asking Claude Sonnet to pick), which is just reading comprehension, not retrieval. Honest rerank numbers are much lower.

## Battleground 3: Context injection (the emerging frontier)

How retrieved memories enter the LLM's context window determines both token cost and reasoning quality.

**System prompt prepend (most common):** Just paste facts before the user message. Simple, universal, but eats context window tokens. Full-context approaches use 26,000+ tokens per query.

**Structured memory blocks (Letta):** Discrete, labeled blocks that the agent can read and write. Always in-context (like RAM). The agent sees them every turn. More structured than raw prepend, supports self-editing.

**Semantic triples + summaries (Memori):** Extracts structured triples from conversations, embeds them, and injects only high-signal content. Requires an average of only 1,294 tokens to ground each LLM response, reducing context cost by more than 20x versus full-context prompting.

**KV injection into attention layers (MemOS):** The most architecturally ambitious approach. Encodes external knowledge as sparse key-value pairs injected directly into the model's self-attention layers during inference. Memory doesn't consume prompt tokens at all. But requires tight model-level integration and isn't portable across LLM providers.

## What nobody has solved

| Gap | Why it's hard | Who's closest |
|-----|--------------|---------------|
| Memory decay | "In Tokyo" from 6 months ago is stale; "allergic to peanuts" is forever. No system distinguishes temporal vs permanent facts. | MemOS (lifecycle metadata) |
| Confidence scoring | LLM might misinterpret sarcasm as preference. No system scores extraction confidence. | Nobody |
| Multi-agent memory sharing | Agent A learns something, Agent B needs it. Most systems are single-agent. | Letta (sub-agent state passing), OpenClaw namespace isolation |
| Benchmark validity | LoCoMo has ~99 wrong answers. Cross-paper scores are not comparable due to evaluation harness disputes. | Letta Leaderboard (live, dynamic) |

## The bottom line

The entire field is competing on variations of "use an LLM to decide what to remember and how to update it." The real differentiation is: how much structure wraps that decision (Mem0 = minimal, Letta = agent loop, A-Mem = never delete, MemOS = temporal metadata), what storage backends support it (vector, graph, hybrid, filesystem), what retrieval strategies optimize cost vs accuracy, and how much you trust the LLM versus building deterministic guardrails around it.