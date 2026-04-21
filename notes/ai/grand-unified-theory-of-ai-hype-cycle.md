---
title: "grand unified theory of the AI hype cycle"
date: 2024-06-13
captured: 2024-06-13T00:00:00Z
tags: [ai, hype-cycle, history, machine-learning]
source: "GitHub issue tieubao/til#591"
aliases: []
status: refined
---

## Context

A blog post by Glyph (2024) that maps the recurring pattern of AI hype cycles across seven decades of computing history. Each cycle follows the same structural arc, driven by a novel mechanism that gets relabeled as "AI" once it shows commercial promise.

**Source:** [A Grand Unified Theory of the AI Hype Cycle](https://blog.glyph.im/2024/05/grand-unified-ai-hype.html)

## The cycle pattern

The history of AI goes in cycles. Each follows roughly this arc:

1. Scientists develop a promising novel mechanism `N` with a specific name. It typically requires about 3x the specs of the average computer available at the time.
2. R&D funding flows in based on hypothetical potential. The funding buys more compute, which produces immediate results since the tech was resource-constrained.
3. Initial successes hint at revolutionary possibilities, including automating a dimension of cognition not previously machine-automated.
4. Leaders (executives, lab administrators, not practitioners) recognize the sales potential of calling this "AI," speculating about science-fictional upheaval in 5-20 years.
5. Other tech leaders adopt `N` in increasingly unreasonable ways to access the AI funding pool.
6. The scope of "AI" balloons to include pretty much all of computing technology. Things that don't even include `N` get the label.
7. Massive economic boom in anything plausibly adjacent to `N` in a pitch deck or grant proposal.
8. Roughly 3 years pass. Gold owners grow skeptical slowly because their own machines can't run the tech to observe its limitations directly. Public critics appear.
9. Competent practitioners quietly stop calling their tools "AI" and start getting funding under other auspices. Users adopt more specific terms.
10. Moore's law catches up. Average computers can now run the software locally.
11. Investors upgrade their personal machines, finally run the software themselves, get disappointed. They stop writing checks and pivot to biotech.
12. "AI" becomes the label for unproductive uses of `N` (productive uses are marketed by application, not mechanism). Funding collapses. AI winter arrives.
13. Remaining researchers discover a new mechanism `M`. Go to step 1.

## Historical values of N

- Neural networks and symbolic reasoning (1950s)
- Theorem provers (1960s)
- Expert systems (1980s)
- Fuzzy logic and hidden Markov models (1990s)
- Deep learning (2010s)

Each cycle has been larger and lasted longer than the last. Each has produced genuinely useful technology. The problem is that everyone mistakes a sigmoid curve for an exponential one. There is an initial burst of rapid improvement, followed by gradual improvement, followed by a plateau.

## Where we are now (as of mid-2024)

- Research shows log-linear relationships between concept frequency and model performance, suggesting diminishing returns
- Actually-useful LLMs on personal devices are becoming pedestrian
- Consumer AI hardware products (Rabbit R1, Humane AI Pin) were disappointing
- Biotech is attracting attention again
- Everything is still being called "AI" (the label hasn't yet narrowed)

The author notes this cycle is unlike previous ones in scale: more money, more commercial (previous cycles were often confined to one DARPA department). But the fundamental pattern holds. Computers cannot think, and the problems of the current "AI" will not all be solved within "5 to 20 years."

## Key insight

The productive uses of each novel mechanism survive every AI winter. They just stop being called "AI" and get marketed under their application domain instead. What dies is the hype label, not the technology.

## Related

- [[predictive-history-and-the-ambition-of-psycho-history]] - pattern recognition in historical cycles
