---
title: "Scaling the harness: six components of an agentic system"
date: 2026-07-04
captured: 2026-07-04T05:30:00.000Z
tags: ["ai", "agents", "architecture", "evaluation", "telemetry"]
source: "From Model Scaling to System Scaling: Scaling the Harness in Agentic AI - Shangding Gu (UC Berkeley), https://arxiv.org/abs/2605.26112"
status: refined
---

# Scaling the harness: six components of an agentic system

## Context

The paper argues the next bottleneck in agentic AI is SYSTEM scaling, not model scaling: once the model is good enough, long-horizon performance is set by the harness, the structured layer around the model. It benchmarks Claude Code against OpenClaw and a Python reference harness, and its framework matches what practitioners keep rediscovering ad hoc. Captured because it gives names to failure modes I had been hitting without a vocabulary.

![Six components of an agent harness](https://assets.han-ws.workers.dev/i/2026/07/harness-six-components.svg)

## The decomposition

An agent is not a model with a prompt; performance over a horizon is a function of six interacting components: `P = f(R, M, C, S, O, G)`, reasoning substrate, memory store, context constructor, skill router, orchestration loop, verification + governance. Model scaling improves only R. Everything else is engineering, and each component can be changed, disabled, or measured independently while holding the model fixed.

## The three bottlenecks and their named failure modes

| Component | Canonical failure | The system move |
|---|---|---|
| Context governance | **exposure without access**: the model sees more tokens but attends to the wrong ones; long context is not good context | treat each turn's context as the output of a selection policy: relevance-weighted, budget-penalized, provenance-tracked, freshness-refreshed |
| Memory trust | **stale-but-confident**: a note true at write time whose target drifted; retrieval still ranks it highly; acting on it is destructive | re-establish trust AT RETRIEVAL: staleness penalty, treat recall as a hypothesis, re-verify against the live environment (persistent priors + just-in-time grep is the hybrid that works) |
| Skill routing | **confident-but-unchecked**: a specialized subagent returns plausible output that no downstream layer validates; the symmetric twin of stale-but-confident | post-condition checks as a first-class part of every skill spec; skill quality without verification scaling = faster but less reliable |

Complementary temporal layers: **prompt** controls now (brittle over horizons), **skill** controls this-class-of-things (fails by wrong routing or bad composition), **memory** controls what survives (fails by drift, over-generalization, pollution).

## The evaluation agenda (the part most benchmarks miss)

Single-score task success mixes model capability with harness design. The paper's proposal, which matches my experience running an instrumented SDD pipeline:

- **Process metrics beside outcomes**: tokens, tool calls, retries, failed edits, human interventions, auditability. Two agents that both solve a task can differ enormously here.
- **Longitudinal dimensions**: memory retrieval precision, memory hygiene, minimal-context efficiency, communication fidelity between agents, long-session drift, verification-aware recovery, safety under tool access. One-shot evals cannot see whether accumulation makes an agent better or quietly worse.
- **An evolution standard, four questions**: what persists? what updates online vs needs review? what is measured? what is auditable? Without answers, a "learning agent" is an opaque accumulation of prompts and heuristics.

## Hard-won corollaries from applying it

- **Measure per-mechanism yield, not framework-vs-none.** The useful control arm is "did this gate ever catch anything," queryable from your own ledgers, not an A/B you will never run.
- **A telemetry emitter must ship with its reader.** Write-only ledgers are the recurring disease; an emit whose reader lands "later" stays unread for months.
- **Communication artifacts (handoffs, summaries) deserve validation.** The paper calls it communication fidelity; in practice it means existence-claims in a handoff get verified before they are written, because a wrong summary poisons the next session's context.

## Related

- [[finding-your-unknowns-agentic-coding]] - the operator-level counterpart: unknowns are the map-territory gap; the harness decides whether they surface early or explode late
- [[memory-systems-as-agent-harness-plugins]] - deep-dive on the M component; the before-turn/after-turn hook pattern is how memory plugs into the O loop
- [[llm-memory-benchmarks-and-evaluation-crisis]] - the memory-evaluation gap this paper's longitudinal agenda targets
