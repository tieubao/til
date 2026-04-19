---
title: "Rob Pike's 5 rules of programming"
date: 2016-04-30
captured: "2016-04-30T04:12:21Z"
tags: [programming, performance, Go, better-dev]
source: "GitHub issue tieubao/til#218"
aliases: []
status: refined
---

## Context

Rob Pike's five rules of programming, widely cited in the Go community and beyond. These rules emphasize measurement over intuition, simplicity over cleverness, and data structures over algorithms.

**Source:** [Rob Pike's 5 Rules](http://users.ece.utexas.edu/~adnan/pike.html) | [HN discussion](https://news.ycombinator.com/item?id=7994102)

## The five rules

1. **You can't tell where a program is going to spend its time.** Bottlenecks occur in surprising places. Don't put in a speed hack until you have proven where the bottleneck is.

2. **Measure.** Don't tune for speed until you have measured, and even then don't unless one part of the code overwhelms the rest.

3. **Fancy algorithms are slow when n is small, and n is usually small.** Fancy algorithms have big constants. Until you know n is frequently going to be big, don't get fancy. (Even if n does get big, use Rule 2 first.)

4. **Fancy algorithms are buggier than simple ones, and much harder to implement.** Use simple algorithms as well as simple data structures.

5. **Data dominates.** If you have chosen the right data structures and organized things well, the algorithms will almost always be self-evident. Data structures, not algorithms, are central to programming.

## Connections to other wisdom

- Rules 1 and 2 restate Tony Hoare's maxim: "Premature optimization is the root of all evil."
- Ken Thompson rephrased rules 3 and 4 as: "When in doubt, use brute force."
- Rules 3 and 4 are instances of KISS.
- Rule 5 was previously stated by Fred Brooks in The Mythical Man-Month, often shortened to: "Write stupid code that uses smart objects."

## Related
