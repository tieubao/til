---
title: "what makes code Swifty"
date: 2020-02-28
captured: "2020-02-28T06:37:36Z"
tags: [swift, code-style, programming-languages]
source: "GitHub issue tieubao/til#483"
aliases: []
status: refined
---

## Context

John Sundell explores what makes code "Swifty" - meaning well-aligned with Swift's core goals and idioms. The answer comes down to three pillars that define the language's design philosophy.

**Source:** [What makes code "Swifty"? | Swift by Sundell](https://www.swiftbysundell.com/articles/what-makes-code-swifty/)

**Attachment:** [What makes code Swifty - Swift by Sundell.pdf](https://github.com/tieubao/til/files/4265548/What.makes.code.Swifty.Swift.by.Sundell.pdf)

## The three pillars

**Clarity through strong type safety.** Swift's type system is designed to catch errors at compile time rather than runtime. Swifty code leverages enums, optionals, and generics to make invalid states unrepresentable. The compiler becomes a collaborator, not an obstacle.

**The path to performance.** Swift is designed so that writing clear, idiomatic code also tends to produce performant code. Value types (structs), copy-on-write semantics, and the optimizer work together so that "the obvious way" is often the fast way.

**Clear, expressive naming.** Swift's API design guidelines emphasize clarity at the point of use. Method names read like sentences, parameter labels provide context, and the overall style favors explicitness over brevity.

## Key takeaway

"Swifty" code is not about using every language feature available. It is about aligning with these three goals so that code is safe, fast, and readable by default. When evaluating whether code feels Swifty, check it against these pillars rather than chasing syntactic novelty.

## Related
