---
title: "Go proverbs"
date: 2016-04-01
captured: "2016-04-01T07:57:13Z"
tags: [Go, programming, proverbs, better-dev]
source: "GitHub issue tieubao/til#199"
aliases: []
status: refined
---

## Context

Rob Pike's Go Proverbs, presented as "Simple, Poetic, Pithy" guidelines for writing idiomatic Go. These capture the language's design philosophy in memorable one-liners.

**Source:** [go-proverbs.github.io](http://go-proverbs.github.io)

## The proverbs

- Don't communicate by sharing memory, share memory by communicating.
- Concurrency is not parallelism.
- Channels orchestrate; mutexes serialize.
- The bigger the interface, the weaker the abstraction.
- Make the zero value useful.
- interface{} says nothing.
- Gofmt's style is no one's favorite, yet gofmt is everyone's favorite.
- A little copying is better than a little dependency.
- Syscall must always be guarded with build tags.
- Cgo must always be guarded with build tags.
- Cgo is not Go.
- With the unsafe package there are no guarantees.
- Clear is better than clever.
- Reflection is never clear.
- Errors are values.
- Don't just check errors, handle them gracefully.
- Design the architecture, name the components, document the details.
- Documentation is for users.
- Don't panic.

## Key themes

**Simplicity over cleverness.** "Clear is better than clever" and "reflection is never clear" push toward straightforward code.

**Composition over abstraction.** Small interfaces are stronger abstractions. A little copying avoids unnecessary coupling.

**Concurrency model.** Share memory by communicating (channels), not by sharing memory (mutexes). Channels orchestrate, mutexes serialize.

**Error handling.** Errors are first-class values to be handled, not checked and ignored.

## Related
