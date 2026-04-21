---
title: "functional programming for the rest of us"
date: 2017-01-26
captured: 2017-01-26T21:41:30Z
tags: [functional]
source: "GitHub issue tieubao/til#286"
aliases: []
status: refined
---

**Source:** [Functional Programming For The Rest of Us - defmacro.org](http://www.defmacro.org/ramblings/fp.html)

## Context

Slava Akhmechet argues that functional programming appears difficult only because of how it has been historically presented, not because of inherent complexity. The article serves as an accessible introduction to FP's core ideas and practical benefits.

## Core concepts

**Immutability.** All variables are final/immutable. Functions replace traditional state modification. State lives on the stack through recursion and function parameters rather than through mutable variable assignments.

**No side effects.** Since symbols cannot change, functions depend solely on their arguments and produce only return values. This eliminates hidden dependencies between code components.

**Functions as units of abstraction.** Functional languages offer a different kind of abstraction that makes immutability practical, unlike imperative languages where immutability feels like a constraint.

## Practical benefits

| Benefit | Why |
|---------|-----|
| Unit testing | Function outputs depend only on inputs; no hidden state to mock |
| Debugging | Bugs are reproducible; examining stack arguments reveals complete context |
| Concurrency | Data immutability eliminates deadlocks and race conditions without locks |
| Hot deployment | Erlang-style systems can upgrade live without stopping service |
| Mathematical optimization | Compilers can prove code equivalence and optimize safely |

## Advanced features explained

- **Higher-order functions** - passing functions as parameters eliminates extensive class hierarchies
- **Currying** - automatically creating function wrappers through partial application
- **Lazy evaluation** - delaying computation until needed; enables infinite data structures
- **Continuations** - generalizing function returns to arbitrary program locations, useful for web applications

## Historical context

Church's lambda calculus (1930s) formalized computation as a mathematical system. McCarthy implemented it as Lisp in 1958, demonstrating that functional programming was practical on von Neumann architecture. The theory preceded the hardware by decades.

## Related

