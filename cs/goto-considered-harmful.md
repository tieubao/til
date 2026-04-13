---
title: "Go To statement considered harmful - Dijkstra"
date: 2017-08-09
captured: 2017-08-09T16:57:16Z
tags: ["programming-languages", "structured-programming", "computer-science", "history"]
source: "GitHub issue tieubao/til#318"
aliases: []
status: refined
---

## Context

Edsger W. Dijkstra's seminal 1968 letter to Communications of the ACM, one of the most influential papers in computer science history. It argued for abolishing the `go to` statement from all higher-level programming languages.

**Source:** [Go To Statement Considered Harmful](http://www.u.arizona.edu/%7Erubinson/copyright_violations/Go_To_Considered_Harmful.html)

## The core argument

Dijkstra observed that the quality of programmers is a decreasing function of the density of `go to` statements in their programs. His reasoning:

**The programmer's real subject is the process, not the program.** A program is static text; the process it generates is dynamic behavior over time. We should minimize the conceptual gap between the two.

**We need independent coordinates to describe process progress.** With sequential statements, a single "textual index" (a pointer to a place in the code) suffices. With conditionals, this still works. With procedures, we need a sequence of textual indices (the call stack). With loops, we add dynamic indices (iteration counters).

**`go to` destroys this coordinate system.** With unbridled `go to`, the only way to describe progress is by counting actions since program start, which is "utterly unhelpful." It becomes extremely complicated to reason about what any variable means at any given point.

## The alternative

Dijkstra did not call for eliminating all flow control, but for using structured constructs (conditionals, loops, procedures) that maintain a meaningful coordinate system. These constructs "bridle" the use of jumps so that programs remain intellectually manageable.

He acknowledged the exercise of mechanically converting arbitrary flow diagrams to jump-free ones is not recommended, as the result would not be more transparent than the original.

## Historical note

The idea was not new even in 1968. Dijkstra credits Heinz Zemanek (1959), C.A.R. Hoare, and the work of Wirth, Hoare, Bohm, and Jacopini in establishing the theoretical superfluousness of `go to`. Jacopini proved that any program with `go to` can be expressed without it.

## Related

- [[turing-completeness]] - foundational concept about what computation is possible
