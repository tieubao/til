---
title: "comparing Elixir and Go"
date: 2017-01-27
captured: 2017-01-27T05:21:50Z
tags: [golang, elixir, language-comparison, concurrency]
source: "GitHub issue tieubao/til#287 + https://blog.codeship.com/comparing-elixir-go/"
aliases: []
status: refined
---

## Context

A comparison of Go and Elixir as concurrent programming languages, originally published on the Codeship blog. Both languages are popular choices for building concurrent systems but take fundamentally different approaches.

**Source:** [Comparing Elixir and Go](https://www.cloudbees.com/blog/comparing-elixir-go/)

## Language foundations

**Go** (Google, 2009): compiles to native binary, balances development speed with performance and stability. C-style syntax with static typing.

**Elixir** (Plataformatec, 2011): runs on BEAM/Erlang VM, designed for extensibility and compatibility with the Erlang ecosystem. Functional programming with immutable data and pattern matching.

## Concurrency models

| Aspect | Go | Elixir |
|--------|-----|--------|
| Scheduling | Cooperative | Preemptive |
| Units | Goroutines (~2 KB each) | Processes (~0.5 KB each) |
| Communication | Channels | Process mailboxes |
| Memory | Shared, application-wide GC | Isolated heaps per process |
| Mutability | Allows mutable shared memory | Enforces immutability |

The key architectural difference: Elixir enforces immutability through message passing, eliminating entire classes of concurrency bugs by design. Go allows shared mutable state, giving developers more control but more responsibility.

## Error handling

**Go:** explicit error handling at every call site. Panics crash the entire application.

**Elixir:** supervisor pattern automatically restarts failed processes without affecting others. "Let it crash" philosophy means individual failures are isolated and self-healing.

A useful mental model: "Elixir is an operating system and Go is a specialized program."

## When to choose which

**Choose Go for:**
- High-performance, focused microservices
- Portable system-level tools and CLIs
- Simplicity and fast onboarding
- CPU-bound workloads

**Choose Elixir for:**
- Full-stack web applications requiring resilience
- Distributed systems and clustering
- Real-time systems (chat, streaming, live updates)
- Applications requiring zero-downtime deployments via hot reloading

**Decision factor:** does your application need to run indefinitely across distributed infrastructure, or serve a specific, well-defined purpose?

## Takeaways

- Go optimizes for simplicity and raw performance; Elixir optimizes for fault tolerance and distribution
- Elixir's BEAM VM provides preemptive scheduling and process isolation that Go developers must build manually
- Both are excellent for concurrent systems but serve different operational profiles
- The "let it crash" philosophy is Elixir's biggest advantage for long-running systems

## Related
- [[between-golang-and-elixir]] - another perspective on Go vs Elixir
- [[elixir-concepts-for-go-developers]] - bridging the two languages conceptually
- [[good-and-bad-elixir]] - Elixir-specific code quality patterns
- [[go-vs-swift-comparison]] - another Go language comparison
