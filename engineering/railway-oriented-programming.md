---
title: "Railway Oriented Programming"
date: 2018-06-24
captured: 2018-06-24T13:43:35Z
tags: [functional]
source: "GitHub issue tieubao/til#378"
aliases: [ROP]
status: refined
---

**Source:** [Railway Oriented Programming - F# for fun and profit](https://fsharpforfunandprofit.com/rop/)

## Context

Scott Wlaschin's approach to error handling in functional programming, using a railway track analogy. Deliberately avoids monad terminology to make the concept accessible to developers new to FP.

## The concept

ROP treats program flow as two parallel tracks: a "happy path" (success track) and an "error path" (failure track). Functions can switch between tracks, but once on the error track, subsequent functions on the happy path are bypassed.

This is implemented using a two-track type system, typically `Either` (or `Result`) with custom error types on both the success and failure sides.

## Core patterns

- **Bind operation** - integrates monadic functions into the two-track model, letting a one-track function (that might fail) connect to the two-track pipeline
- **Kleisli composition** - chains monadic functions together cleanly
- **Map function** - adapts non-monadic (pure) operations to work on the two-track model
- **Parallel validation** - combines multiple validation results, collecting all errors rather than failing on the first one
- **Custom error types** - supports domain-driven design by making errors explicit and typed

## Why it matters

ROP provides a complete recipe that combines error mapping, logging integration, compensating transactions, and domain events into a consistent pattern. The result is that "there is basically only one way to write the code," which reduces cognitive load and makes codebases more uniform.

## Relationship to monads

ROP is essentially the Either monad in practice. Wlaschin chose the railway metaphor over monad terminology because it makes the pattern more intuitive. The underlying mechanics are the same, but the mental model is easier to teach and apply.

## Related

