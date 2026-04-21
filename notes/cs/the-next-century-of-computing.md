---
title: "the next century of computing"
date: 2022-10-10
captured: 2022-10-10T00:00:00Z
tags: [computing, hardware, architecture, future, predictions]
source: "GitHub issue tieubao/til#578"
aliases: []
status: refined
---

## Context

Charles Rosenbauer's essay predicting how computing will transform over the next century as Moore's Law ends. The central thesis: the end of easy transistor scaling will trigger a "Cambrian Explosion of bizarre hardware" as the industry moves beyond architectures designed around 1940s computational theory.

**Source:** [The Next Century of Computing](https://bzolang.blog/p/the-next-century-of-computing)

**Attachment:** [The Next Century of Computing - by Charles Rosenbauer.pdf](https://github.com/tieubao/til/files/9747511/The.Next.Century.of.Computing.-.by.Charles.Rosenbauer.pdf)

## Hardware architecture transformation

Traditional architectures like x86 and ARM will give way to more exotic models that conform to physics constraints (heat, energy, space, signal propagation) rather than legacy abstractions. Computing shifts toward tiled architectures - grids of small, simple cores - making systems "geometrically intuitive."

## Memory and performance

Data locality becomes paramount as moving data is relatively expensive compared to computation on tiled systems. Traditional garbage collection becomes impractical. Memory arenas and region-based management dominate, with memory divided into dedicated regions with defined spatial layout.

Superlinear algorithms (O(n^2), O(n^3)) become attractive when compute-to-I/O ratios favor expensive searches. Tools like persistent homology and dimensionality reduction become ubiquitous.

## Software development

Programmers must understand CPU caches and data geometry. Code synthesis and superoptimization tools handle manual layout optimization. SAT and SMT solvers become commonplace across domains.

## Key predictions

- AR proves economically valuable for technical work; VR "immersion" wastes billions
- Quantum computing benefits materials science simulation more than cryptography
- Reversible computing becomes more practical than quantum approaches
- ML's "black box" approach ends; models must be interpretable
- Cryptography becomes "the only real defense" against powerful ML
- New tools enable non-text programming (audio, haptic, visual)
- Human-level AI emerges from theoretical neuroscience, not deep learning

## Related

- [[history-of-regular-expressions]] - foundational CS concepts evolving over decades
