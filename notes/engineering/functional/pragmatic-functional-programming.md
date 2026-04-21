---
title: "pragmatic functional programming"
date: 2017-07-13
captured: 2017-07-13T19:29:11Z
tags: [functional, clojure]
source: "GitHub issue tieubao/til#308"
aliases: []
status: refined
---

**Source:** [Pragmatic Functional Programming - Uncle Bob](http://blog.cleancoder.com/uncle-bob/2017/07/11/PragmaticFunctionalProgramming.html)

## Context

Uncle Bob (Robert C. Martin) weighs in on why functional programming matters, even if the multi-core apocalypse never fully arrived. Written in 2017 when the FP movement was maturing beyond its initial hype cycle.

## The original motivation: concurrency

The move to FP began around a decade before 2017. Moore's law held from the 1960s until ~2000, then clock rates hit 3GHz and plateaued. Hardware designers shifted to adding cores instead of increasing speed. The expectation was that core counts would keep doubling, making concurrency the dominant problem.

FP strongly discourages changing variable state once initialized. No mutable state means no race conditions, no concurrent update problems. This seemed like the perfect answer to the multi-core future.

But the freight train never came. Laptops stayed at 4 cores for years. So is FP still worth learning?

## Why FP matters regardless of cores

**Concurrency safety.** Even without 32,768-core chips, systems with threads and processes benefit enormously from immutable state. Abandoning FP discipline would be as big a mistake as rampant `goto` usage.

**Simplicity.** You don't track system state because variables can't change. Push an element onto a stack in FP and you get a new stack; the old one remains. Fewer balls to juggle means code that is easier to write, read, understand, and test.

**Familiarity curve.** Maps, reduces, and tail recursion feel hard at first, but the learning curve is short. Once familiar, programming gets meaningfully simpler.

## FP and OO are not enemies

A common misconception is that FP and OO are mutually incompatible. They are not. In FP, calling a method that adjusts an object's value returns a new object instead of mutating the old one. OO's most useful architectural feature - dynamic polymorphism - works perfectly within FP.

Clojure example of a polymorphic interface on the JVM:

```clojure
(defprotocol Gateway
  (get-internal-episodes [this])
  (get-public-episodes [this]))

(deftype Gateway-imp [db]
  Gateway
  (get-internal-episodes [this]
    (internal-episodes db))
  (get-public-episodes [this]
    (public-episodes db)))
```

This produces identical JVM byte-code to a Java interface. Java can implement Clojure interfaces and vice versa.

## Why Clojure specifically

Uncle Bob recommends Clojure because it is a dialect of Lisp, which is beautifully simple. The syntax transformation from Java to Lisp: `f(x)` becomes `(f x)` - move the first parenthesis left. That covers ~95% of Lisp syntax.

Clojure improvements over classic Lisp:
- More punctuation, fewer parentheses
- `first`, `rest`, `second` replace `CAR`, `CDR`, `CADR`
- Built on the JVM with full Java library interop
- Full access to OO features of the JVM
- **Homoiconic** - code is data the program can manipulate. All function calls are lists, and lists can be constructed and executed at runtime.

## Key takeaway

Functional programming is worth learning not just for concurrency, but because immutability makes code fundamentally simpler. FP and OO complement each other. The recommendation: start with Clojure for its simplicity and JVM interop.

## Related

