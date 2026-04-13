---
title: "the Zen of Go"
date: 2020-02-23
captured: 2020-02-23T13:40:18Z
tags: [golang, philosophy, best-practices]
source: "GitHub issue tieubao/til#479 + https://dave.cheney.net/2020/02/23/the-zen-of-go"
aliases: []
status: refined
---

## Context

Dave Cheney draws parallels between Python's PEP-20 and Go's engineering values, proposing that Go should be understood through explicit design principles rather than vague notions of "idiomatic" code.

**Source:** [The Zen of Go - Dave Cheney](https://dave.cheney.net/2020/02/23/the-zen-of-go)
**Attachment:** [The Zen of Go | Dave Cheney.pdf](https://github.com/tieubao/til/files/4241295/The.Zen.of.Go.Dave.Cheney.pdf)

## Core principles

**Each package fulfills a single purpose.** Package names should be nouns describing what they provide. A clear purpose enables easier maintenance and replacement.

**Simplicity enables reliability.** Avoid clever code in favor of readable, understandable implementations. Simple is better than complex.

**Minimize global state.** Encapsulate state in types rather than using package-level variables. This reduces coupling and eliminates hidden dependencies.

**Handle errors first.** Design robust code by handling failure cases before success paths. Errors should never pass silently. Explicit error handling at the point of occurrence prevents production surprises.

**Keep control flow flat.** Avoid deeply nested indentation. Use guard clauses to return early. Keep the successful code path visible and left-aligned.

**Goroutine discipline.** Before launching a goroutine, answer three questions: Under what condition will it stop? What is required for that condition? How will you know it has stopped? Leave concurrency decisions to the caller when writing libraries.

**Performance requires evidence.** Do not optimize based on assumptions or dogma. Use benchmarking tools to identify actual bottlenecks. Measure before claiming something is slow.

**Maintainability over cleverness.** Code outlives its author. Write for future maintainers. Ask: "Can this be maintained after I'm gone?"

## Actionable takeaways

1. Name packages carefully as single-purpose units
2. Avoid package-level state entirely where possible
3. Handle errors explicitly at each call site
4. Keep functions flat with minimal nesting
5. Test API contracts to lock in expected behavior
6. Benchmark performance claims before optimization
7. Use moderation with Go's powerful features (goroutines, channels, embedding)

## Related

- [[zen-of-python]] - the original inspiration for this talk
- [[go-proverbs]] - Rob Pike's complementary set of Go guidelines
- [[code-for-readability]] - related principle about writing maintainable code
