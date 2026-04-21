---
title: "Go vs Swift comparison"
date: 2017-06-07
captured: 2017-06-07T19:20:32Z
tags: [golang, swift, language-comparison]
source: "GitHub issue tieubao/til#303 + https://github.com/jakerockland/go-vs-swift/blob/master/go-vs-swift.pdf"
aliases: []
status: refined
---

## Context

A side-by-side comparison of Go and Swift by Jake Rockland, examining how both languages approach common programming concepts. Both languages emerged in the 2010s with different goals: Go for systems and server-side programming, Swift for Apple platform development.

**Attachment:** [go-vs-swift.pdf](https://github.com/tieubao/til/files/1059066/go-vs-swift.pdf)

## Key differences

| Aspect | Go | Swift |
|--------|-----|-------|
| Typing | Static, inferred | Static, inferred |
| Paradigm | Procedural, concurrent | Multi-paradigm (OOP, functional, protocol-oriented) |
| Memory | Garbage collected | ARC (Automatic Reference Counting) |
| Concurrency | Goroutines + channels | GCD, async/await (added later) |
| Generics | Added in 1.18 (2022) | From the start |
| Error handling | Multiple return values | `throws`/`try`/`catch` |
| Null safety | Zero values, no null | Optionals |

## Shared design goals

Both languages prioritize:
- **Readability** over cleverness
- **Type safety** with inference to reduce verbosity
- **Fast compilation** compared to their predecessors (C++ for Go's context, Objective-C for Swift)
- **Modern tooling** built into the language ecosystem

## Where they diverge

**Go** favors simplicity and a small feature set. Composition over inheritance. No classes, no exceptions, no generics (until 1.18). The philosophy: fewer features means fewer ways to write bad code.

**Swift** favors expressiveness. Rich type system with enums, protocols, generics, optionals, and pattern matching. The philosophy: powerful abstractions help developers write correct code.

**Concurrency** is where the gap is largest. Go's goroutines and channels are first-class language features designed for concurrent server programming. Swift's concurrency story evolved over time, with structured concurrency arriving in Swift 5.5.

## Takeaways

- Choose Go for server-side, CLI tools, and infrastructure where simplicity and concurrency matter
- Choose Swift for Apple platforms and applications where rich type system features help model complex domains
- Both prove that modern languages can be fast, safe, and readable

## Related
- [[swifty-code]] - Swift-specific idioms and style
- [[swift-pattern-matching-case-let]] - Swift's pattern matching in practice
- [[comparing-elixir-and-go]] - another Go language comparison
