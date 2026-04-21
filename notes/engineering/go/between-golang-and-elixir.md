---
title: "between Go and Elixir"
date: 2021-06-22
captured: 2021-06-22T17:59:55Z
tags: [golang, elixir]
source: "GitHub issue tieubao/til#560"
aliases: []
status: refined
---

## Context

A practical exploration of combining Go and Elixir in production systems, written by Preslav Rachev. The argument centers on using each language where it excels rather than picking one for everything.

**Source:** [Between Golang and Elixir](https://preslav.me/2021/04/23/between-golang-and-elixir/)

## The complementary model

Most applications do not need Kubernetes or complex orchestration. A few Go binaries or a Phoenix app on a single VM can outperform setups costing thousands per month. This reality opens the door to mixing languages on a single machine.

The proposed split:

| Layer | Language | Strength |
|-------|----------|----------|
| Orchestration and user-facing plane | Elixir (BEAM) | Resilience, fault tolerance, live process supervision |
| Short-lived commands, compute-heavy tasks | Go | Speed of execution, extending beyond BEAM boundaries |

Elixir apps can communicate with Go binaries through two mechanisms:

- **Ports** - Elixir's built-in mechanism for managing external OS processes. The BEAM supervises the Go process and restarts it on crash.
- **Web services** - calling Go binaries as regular HTTP services, keeping the integration simple and language-agnostic.

The BEAM's supervision model is the key enabler. Go processes can crash freely while the BEAM maintains a stable face to the customer.

## Why not Rust instead?

Rust cooperates with Elixir even more tightly through Erlang NIFs (Native Implemented Functions) via the Rustler library. However, the author finds Rust's rigid memory safety model harder to think in when solving business requirements. Go's design choices let the programmer and compiler agree on things more easily while maintaining a high level of safety.

This is not a dismissal of Rust. It has its place, particularly in systems where memory safety guarantees are non-negotiable. But for typical business logic wrapped as short-lived commands, Go's simpler mental model wins on productivity.

## Key takeaway

Rather than debating which language is "better," the pragmatic approach is to use each where it shines. Elixir handles orchestration, concurrency, and fault tolerance. Go handles compute-intensive, short-lived tasks. The integration can be as simple as ports or HTTP calls, and the BEAM's supervision tree keeps the whole system resilient.

## Related
