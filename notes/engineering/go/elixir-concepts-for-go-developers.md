---
title: "Elixir concepts for Go developers"
date: 2015-12-20
captured: 2015-12-20T08:30:23Z
tags: [golang, elixir, concurrency, erlang]
source: "GitHub issue tieubao/til#95 + https://texlution.com/post/elixir-concepts-for-golang-developers/"
aliases: []
status: refined
---

## Context

A comparison of Elixir and Go from the perspective of a Go developer. Elixir runs on the BEAM (Erlang VM) with Ruby-like syntax, created by Jose Valim to combine "the power of Erlang with the joy of Ruby."

**Source:** [Elixir Concepts for Go Developers](https://texlution.com/post/elixir-concepts-for-golang-developers/)

## Core language differences

**Soft immutability** - Elixir allows variable name reuse through syntactic sugar. When you reassign a variable, the runtime treats it as a new binding, not mutation.

**Pattern matching** - the killer feature. The `=` operator performs matching, binding variables when patterns align. Functions can define multiple clauses matching different input patterns, functioning as "switch statements on steroids."

**Atoms** - equivalent to Go's iota-indexed constants. Functions commonly return atoms like `:ok` or `:error` for result handling.

## Concurrency: actors vs CSP

| Aspect | Elixir (Actor Model) | Go (CSP) |
|--------|---------------------|----------|
| Processes | Directly addressable via PIDs | Anonymous goroutines |
| Communication | Message passing to processes | Channels are primary |
| Distribution | Transparent across networked VMs | Manual setup required |
| Fault tolerance | Built-in supervisors and supervision trees | Manual error handling |

Elixir's supervisors are built-in processes that monitor and restart failed processes. Organized in "supervision trees," they provide application-level resilience without manual recovery logic.

## Notable features

- **Pipe operator** (`|>`) improves readability by threading values through function chains
- **Macros** enable compile-time AST manipulation for domain-specific languages
- **Phoenix Framework** provides high-productivity web development with built-in WebSocket support
- **OTP Library** offers distributed systems building blocks originally developed for telecom

## BEAM VM characteristics

Low memory footprint, minimal garbage collection pauses, performs well on constrained systems. Sits performance-wise between Java/Go and Python/Ruby.

## Critical perspective

While pattern matching and pipes enable declarative code, Elixir's macro system represents power that could encourage problematic metaprogramming practices if not used with restraint.

## Related
