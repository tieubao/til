---
title: "functional thinking"
date: 2015-12-28
captured: 2015-12-28T01:19:31Z
tags: [functional]
source: "GitHub issue tieubao/til#97"
aliases: []
status: refined
---

**Source:** [Functional Thinking - Neal Ford](http://nealford.com/functionalthinking.html)

## Context

Neal Ford's series on transitioning from object-oriented to functional mindsets. Aimed at developers already comfortable with OO who want to understand how functional languages handle common problems like iteration, concurrency, and state.

## Core idea

Learning a functional paradigm requires a genuine perspective shift, not just new syntax. The series focuses on thinking patterns rather than language-specific features.

## Key topics

**Immutability.** Central to functional programming. Ford explores creating immutable classes even in Java and discusses when this approach is appropriate versus when mutability is acceptable.

**Composition over inheritance.** Functional building blocks emphasize list transformations and portable code rather than OO inheritance hierarchies. Functions compose; objects inherit. This is a fundamental shift in how you structure programs.

**Advanced patterns:**
- Partial application and currying for code reuse
- Error handling via Either and Option types (instead of exceptions)
- Pattern matching across JVM languages
- Memoization and caching techniques

**Language diversity.** The series examines functional features across Java, Groovy, Scala, and Clojure, showing how the same example can be implemented differently while maintaining core functional principles. This cross-language view helps isolate the paradigm from any single language's syntax.

## Key takeaway

Functional thinking is about recognizing functional constructs across languages and applying them to solve problems more elegantly. The paradigm transcends any single language; once you see the patterns, they appear everywhere.

## Related

