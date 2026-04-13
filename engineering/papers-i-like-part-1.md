---
title: "papers I like (part 1) - Fabian Giesen"
date: 2017-08-12
captured: 2017-08-12T19:45:51Z
tags: [papers, cs-fundamentals, reading-list]
source: "GitHub issue tieubao/til#321 + https://fgiesen.wordpress.com/2017/08/12/papers-i-like-part-1/"
aliases: []
status: refined
---

## Context

Fabian Giesen (ryg), a well-known systems programmer, shares 10 papers he considers essential reading. The selection spans concurrency, systems failures, data compression, code generation, computational geometry, image compositing, audio synthesis, and rendering. Each recommendation includes why the paper matters and what makes it stand out.

**Source:** [Papers I Like (Part 1)](https://fgiesen.wordpress.com/2017/08/12/papers-i-like-part-1/)

## The papers

**1. Lamport - "State the Problem Before Describing the Solution" (1978)**
Problem definition must precede solution design. A clear, independent problem statement enables proper evaluation through proofs and invariants.

**2. Herlihy - "Wait-free synchronization" (1991)**
Establishes that compare-and-swap primitives can achieve universal concurrent operations. Defines wait-freedom: any process completes operations within finite steps regardless of other processors' speed.

**3. Cook - "How complex systems fail" (1998)**
A four-page synthesis of incident research. Applicable across domains (medicine, computing). Practical insights into failure mechanisms in complex systems.

**4. Moffat & Turpin - "On the Implementation of Minimum Redundancy Prefix Codes" (1997)**
Canonical Huffman coding eliminates tree-based decoding in favor of code-length approaches. Improves both compression efficiency and practical implementation.

**5. Dybvig et al. - "Destination-Driven Code Generation" (1990)**
One-pass code generation balancing speed with simplicity. Avoids redundant stack manipulation while remaining feasible for JIT compilers under time constraints.

**6. Valmari - "Fast brief practical DFA minimization" (2012)**
Achieves O(n + m log m) DFA minimization in a readable 130-line implementation. Finally resolves decades of inadequate explanations of Hopcroft's algorithm.

**7. Sarnak & Tarjan - "Planar Point Location Using Persistent Search Trees" (1986)**
Introduces fat-node persistent data structures for geometric queries. Demonstrates superior design through structural efficiency rather than algorithmic complexity.

**8. Porter & Duff - "Compositing Digital Images" (1984)**
Premultiplied alpha representation enables commutative filtering and compositing operations. Eliminates edge fringing and simplifies calculations versus non-premultiplied approaches.

**9. Brandt - "Hard Sync Without Aliasing" (2001)**
MinBLEP technique cancels aliasing artifacts by inserting bandlimited steps at discontinuities. Enables practical real-time audio synthesis.

**10. Veach - "Robust Monte Carlo Methods for Light Transport Simulation" (1997)**
Comprehensive PhD thesis combining theory with practical rendering algorithms for unbiased light simulation. Essential for advanced graphics work.

## Key takeaway

The list is notable for valuing clarity of exposition as much as novelty of results. Several papers are recommended not for groundbreaking theory but for finally explaining a known technique well enough to implement (Valmari on DFA minimization, Moffat on Huffman coding). Good writing in CS papers is rare and worth celebrating.

## Related
