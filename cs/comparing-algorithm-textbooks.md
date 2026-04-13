---
title: "comparing algorithm textbooks: CLRS, Tardos, Skiena, Sedgewick"
date: 2019-05-05
captured: "2019-05-05T21:28:50Z"
tags: [algorithms, books, learning, computer-science]
source: "GitHub issue tieubao/til#430"
aliases: []
status: refined
---

## Context

A comparison of four major algorithm textbooks from someone who used them in different contexts: graduate school, reference, and interview prep for Big 4 companies.

**Source:** [Quora answer on algorithm book differences](https://www.quora.com/What-are-the-differences-between-the-following-algo-books-CLRS-Eva-Tardos-The-Algorithm-Design-Manual-by-Skiena-Algorithms-and-Algorithms-in-C%2B%2B-by-Sedgewick/answer/Chandan-S-81)

## The four books

**CLRS (Cormen, Leiserson, Rivest, Stein)**
- Geared for computer scientists. Intense mathematical rigor with challenging exercises.
- Cannot be read end to end. Best used as a gold-standard reference text.
- Deep dives into algorithms will cement your understanding.

**Kleinberg/Tardos (Algorithm Design)**
- Superb coverage of network flow algorithms with an insane number of exercises on that topic.
- Good coverage of NP class problems and approximation algorithms.
- Best pick if your work demands specialized knowledge for network flow problems.

**Skiena (The Algorithm Design Manual)**
- First 8 chapters can be ingested in 2-3 weeks. Tailored for software engineering interviews.
- Deliberately avoids stressing mathematical nuances. Very approachable.
- Fantastic chapter on dynamic programming. War stories break the monotony.
- All examples in C, not pseudocode.
- The hitchhiker's guide section is a pragmatic catalog of well-known problems.

**Sedgewick (Algorithms)**
- Implementation-specific book. Does not cover dynamic programming, greedy algorithms, or NP problems.
- Two-part Coursera modules from Princeton accompany the book.
- Very little mathematical rigor. Uses its own computation model instead of Big-O.
- Official site has working code snippets you can trace through in an IDE.

## How to choose

| Goal | Best pick |
|------|-----------|
| Reference / deep theory | CLRS |
| Network flow / NP problems | Kleinberg/Tardos |
| Interview prep / practical | Skiena |
| Implementation-focused learning | Sedgewick |

If you stay in software engineering for a decade, you will likely visit most of these books eventually.

## Related
