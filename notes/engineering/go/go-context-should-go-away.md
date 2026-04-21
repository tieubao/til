---
title: "context should go away for Go 2"
date: 2017-08-11
captured: 2017-08-11T03:47:35Z
tags: ["golang", "concurrency", "api-design", "language-design"]
source: "GitHub issue tieubao/til#319"
aliases: []
status: refined
---

## Context

A critique by Michal Strba of Go's `context` package, arguing that while it solves a real problem (cancelation), it does so poorly and the language itself should address the issue in Go 2.

**Source:** [Context should go away for Go 2](https://faiface.github.io/post/context-should-go-away-go2/)

## Context spreads like a virus

At Google, every function on the call path between incoming and outgoing requests must accept a `context.Context` parameter. Every potentially slow function from other libraries must also accept context, or cancelation breaks. This means everyone has to deal with context, even those who do not need it.

For non-server Go programmers, this is a burden. Passing `context.TODO()` everywhere hurts readability and removes the fun of writing Go. A language designed to avoid `Foo foo = new Foo()` verbosity now has `ctx context.Context` everywhere.

## Problems with the package itself

**`ctx.Value` is an anti-pattern.** It is not statically typed, requires documentation for supported keys, is similar to thread-local storage (bad for composition and testing), and is prone to name collisions. It is error-prone magic.

**Inefficient linked list implementation.** `WithCancel`, `WithDeadline`, and `WithValue` create a linked list. `WithCancel` sometimes creates a goroutine that leaks if the context is never canceled. `ctx.Value` lookups are O(n) on the depth of the chain.

## What context actually solves

The only real problem context addresses is cancelation, which is genuinely hard in Go. The "Advanced Go Concurrency Patterns" talk discusses this at length. Simple channels do not scale well because: other libraries do not accept cancelation channels, and canceling a sub-tree of goroutines requires extra channels.

Context solves these problems "inefficiently and with numerous problems, but better than anything else out there."

## The proposal

Go 2 should address cancelation directly in the language with a solution that is: simple and elegant, optional and non-infectious, robust and efficient, and focused only on cancelation (not values or timeouts).

## Related
