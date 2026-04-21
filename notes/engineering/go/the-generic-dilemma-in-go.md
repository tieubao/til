---
title: "the generic dilemma in Go"
date: 2016-02-26
captured: 2016-02-26T11:14:32Z
tags: [golang, better-dev]
source: "GitHub issue tieubao/til#173"
aliases: []
status: refined
---

## Context

Russ Cox's concise framing of the fundamental tradeoff every language faces when deciding how to handle generic data structures (vectors, queues, maps, trees). This was one of the most frequently asked questions about Go's design in its early years.

**Source:** [The Generic Dilemma](http://research.swtch.com/generic)

## The three approaches

| Approach | Example | Cost |
|----------|---------|------|
| Leave generics out | C | Slows programmers (manual specialization, code duplication) |
| Compile-time specialization | C++ templates | Slows compilation, bloats binaries, poor instruction cache usage |
| Implicit boxing | Java | Slows execution, inflates memory usage (a vector of bytes uses far more than one byte per byte) |

Each approach trades pain in a different dimension:

**The C approach** adds no language complexity. Programmers write type-specific code by hand or use `void*` with casts. The burden is entirely on the developer.

**The C++ approach** generates specialized code for each type at compile time. This produces efficient individual specializations but the program as a whole suffers. Cox noted cases where eliminating templates shrank text segments from megabytes to tens of kilobytes. The linker must work hard to eliminate duplicate copies.

**The Java approach** wraps everything in objects. The generated code is smaller than C++ output but less efficient in both time and space due to constant boxing and unboxing. Hiding this overhead can also complicate the type system. On the positive side, it likely makes better use of the instruction cache.

## The dilemma stated plainly

Do you want slow programmers, slow compilers and bloated binaries, or slow execution times?

## Historical note

Go eventually added generics in Go 1.18 (March 2022) using a type parameters approach with constraints. The implementation uses a hybrid strategy - GC shape stanzas with dictionaries - that attempts to balance compile time, binary size, and runtime performance. It is neither pure monomorphization (C++) nor pure boxing (Java), aiming to avoid the worst of each tradeoff.

## Related
