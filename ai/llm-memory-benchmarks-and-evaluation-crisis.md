---
title: "LLM memory benchmarks and evaluation crisis"
date: 2026-04-07
captured: 2026-04-07T17:53:19.727Z
tags: ["ai", "memory", "benchmarks", "evaluation"]
source: "Claude.ai chat"
aliases: []
status: refined
---
## Overview

The LLM agent memory space has multiple competing benchmarks but no single "ImageNet of memory." The benchmarks are themselves contested, with documented methodology disputes between major players. Understanding what each benchmark actually measures, and what it doesn't, is essential before trusting any claimed scores.

## Benchmark landscape

| Benchmark | Venue | What it measures | Format | Key metric |
|-----------|-------|-----------------|--------|-----------|
| LoCoMo | Snap Research 2024 | Long-term conversational memory | 300-turn convos, 9k tokens, 35 sessions | QA accuracy (single-hop, multi-hop, temporal, adversarial) |
| LongMemEval | arXiv 2024 | 5 memory abilities | 500 questions across scalable chat histories | Accuracy judged by GPT-4 |
| MemBench | ACL 2025 Findings | Factual + reflective memory | Participation and observation scenarios | Effectiveness, efficiency, capacity |
| MemoryAgentBench | ICLR 2026 | 4 core competencies | Multi-turn incremental injection | Retrieval, test-time learning, long-range understanding, forgetting |
| Evo-Memory | arXiv 2025 | Test-time self-evolving memory | MMLU-Pro, GPQA-Diamond, AIME | Answer accuracy, success rate, step efficiency, sequence robustness |
| Letta Leaderboard | Letta (live) | Agentic memory management | Fictional QA, core + archival memory | Read/write/update scores per model |

## Four core competencies (MemoryAgentBench framework)

MemoryAgentBench (ICLR 2026) identifies the most complete set of what memory agents need:

1. **Accurate retrieval** - finding the right fact from a large memory store
2. **Test-time learning** - improving from new information without retraining
3. **Long-range understanding** - connecting facts across distant conversation turns
4. **Selective forgetting** - knowing when to discard or update outdated info

No existing benchmark covers all four. Most focus heavily on retrieval and neglect forgetting.

## The LoCoMo crisis

LoCoMo is the de facto standard: most papers benchmark against it. But it has serious problems.

**Ground truth errors:** An independent audit (Penfield Labs) found roughly 99 wrong, hallucinated, or misattributed answers across the dataset's ten conversations. A 100% score is mathematically impossible on the published version.

**The evaluation harness itself is broken:** The LoCoMo LLM-judge scores up to 63% of intentionally wrong answers as correct.

**Cross-paper scores are not comparable:** Mem0, Zep, and Letta have all published conflicting claims about each other's LoCoMo scores. Zep accused Mem0 of running a misconfigured Zep version during benchmarking. Mem0's CTO claimed Zep's real score is 58.44% vs their self-reported 84%. Letta reached similar conclusions about reproducibility independently.

## The MemPalace case study (April 2026)

A concentrated example of everything wrong with memory benchmarking.

**What happened:** A GitHub repo (milla-jovovich/mempalace) launched April 5, 2026 claiming 100% on LoCoMo and a perfect 500/500 on LongMemEval. The launch tweet credited actress Milla Jovovich as co-author and reached 1.5M people, earning 5,400 stars in under 24 hours.

**The LoCoMo fraud:** The runner used top_k=50 against a candidate pool of max 32 sessions. This retrieves the entire conversation every time. The "memory architecture" contributes nothing; it's just "dump everything into Claude Sonnet and ask." The project's own BENCHMARKS.md admits this.

**The LongMemEval metric category error:** The runner does retrieval only. It never generates an answer and never invokes a judge. The "100%" is recall@5 (did the right session appear in top 5?), not end-to-end QA accuracy. Calling it a "perfect score on LongMemEval" conflates a substantially easier task with the actual benchmark. The 100% was achieved by writing three targeted patches for three specific wrong answers (teaching to the test, which the docs admit).

**Missing features:** The launch claimed "contradiction detection" but the codebase contains zero occurrences of "contradict." The "30x lossless compression" claim was contradicted by the project's own benchmarks showing a 12.4 percentage point quality drop.

**The lesson:** Celebrity-driven launches exploit the broken benchmark layer. Most developers won't read BENCHMARKS.md. The MemPalace docs were actually honest about the limitations, but the launch tweet stripped every caveat.

## The Letta Leaderboard (most practically useful)

The only live, continuously updated benchmark. It tests actual agentic memory management: can the model use tools to manage its own context?

Tests cover:
- **Core memory read:** Can the model extract info from in-context memory blocks?
- **Core memory write:** Can it update memory blocks with new facts?
- **Core memory update:** Can it handle contradicting facts by rewriting blocks?
- **Archival memory:** Can it search and retrieve from external storage?

Uses fictional QA datasets to minimize contamination from LLM training data. Reports both score and cost per model. As of March 2026, top models: Claude 4 Sonnet (88%), GPT-5.2-codex (93% on filesystem tasks), Gemini 3 Flash (82%).

## What to actually trust

The multi-dimensional evaluation framework from Mem0's ECAI 2025 paper is the most honest: it measures BLEU score, F1 score, LLM-judge accuracy, token consumption, AND latency together. This prevents optimizing one axis at the expense of others. A system that scores well on accuracy but requires 26,000 tokens per query is not production-viable.

For model selection: use the Letta Leaderboard.
For system comparison: be skeptical of any single-benchmark claims. Look for papers that report multiple metrics including latency and token cost.

## Related

- [[llm-agent-memory-systems-landscape-2026]] - the production systems and research systems whose benchmark claims this note scrutinizes
- [[llm-memory-systems-three-competitive-battlegrounds]] - the retrieval strategy battleground where latency vs accuracy numbers from benchmarks actually matter
- [[tool-evaluation-5-question-rubric]] - a practical evaluation framework that sidesteps benchmark gaming by asking "what failure would this have prevented?"