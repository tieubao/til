---
title: "AutoResearch: the Karpathy loop pattern"
date: 2026-03-26
captured: 2026-03-26T23:09:49.978Z
tags: ["autoresearch", "karpathy", "optimization", "ai-tooling"]
source: "Claude iOS session - SDD research session 2"
---
## The three-file contract

AutoResearch by Andrej Karpathy (released March 7, 2026, 42k+ GitHub stars) is an autonomous optimization loop. The core pattern:

| File | Who controls it | What it does |
|------|----------------|-------------|
| `program.md` | Human only | Research direction. Goals, constraints, what to explore. |
| `train.py` (or whatever is being optimized) | Agent only | The artifact being improved. Agent modifies freely. |
| `prepare.py` (or eval script) | Nobody (frozen) | Scoring function. Measures progress. Cannot be gamed. |

## The ratchet mechanism

1. Agent reads program.md + current best code + experiment log
2. Agent proposes a change and modifies the code
3. Run for fixed time budget (5 min in Karpathy's setup)
4. Evaluate against frozen metric (val_bpb for ML, but generalizable)
5. If score improved: git commit (new baseline). If not: git revert.
6. Loop. ~100 experiments overnight.

## Why it matters beyond ML

The pattern works on anything you can score: skill files, prompt templates, marketing copy, SEO, document generators. The three ingredients are: something to modify, a metric to score it, and a loop to keep winners.

For skill file optimization: use LLM-as-judge with binary eval criteria (10 yes/no questions scored by a second LLM call). Run 5 test profiles, max score = 50. Cost: ~$10-15 for 200 experiments overnight on Sonnet.

## When NOT to build this

For most single-artifact optimizations, doing 3-5 manual iterations with Claude is faster and cheaper than setting up the full loop. The pattern pays off when you have many things to optimize (20+ skill files) or when the scoring function already exists (test suites).

#autoresearch #karpathy #optimization #ai-tooling