---
title: "functional vs imperative vs declarative programming"
date: 2015-12-28
captured: 2015-12-28T01:48:32Z
tags: [functional]
source: "GitHub issue tieubao/til#98"
aliases: []
status: refined
---

**Source:** [StackOverflow - functional vs imperative](http://stackoverflow.com/questions/17826380/what-is-difference-between-functional-and-imperative-programming-languages), [StackOverflow - functional, declarative, imperative](http://stackoverflow.com/questions/602444/what-is-functional-declarative-and-imperative-programming), [MSDN - functional vs imperative](https://msdn.microsoft.com/en-us/library/bb669144.aspx)

## Context

Quick reference card for the three major programming paradigms. Useful when explaining paradigm differences to junior developers or when deciding which style fits a problem domain.

## The three paradigms

**Imperative programming** specifies a series of instructions that the computer executes in sequence. "Do this, then do that." You explicitly describe the control flow and state mutations.

**Declarative programming** declares a set of rules about what outputs should result from which inputs. "If you have A, then the result is B." An engine applies these rules to inputs and produces outputs. You describe the desired result, not the steps to get there.

**Functional programming** is a type of declarative programming. It declares mathematical/logical functions that define how input is translated to output. For example, `f(y) = y * y`. Functions are first-class, state is immutable, and side effects are avoided.

## The relationship

```
Declarative
├── Functional (Haskell, Clojure, Elm)
├── Logic (Prolog)
└── Query (SQL)

Imperative
├── Procedural (C, Pascal)
└── Object-Oriented (Java, C++)
```

Most modern languages are multi-paradigm. Python, JavaScript, Scala, and Kotlin support both imperative and functional styles. The distinction is about the dominant style and what the language encourages, not a hard boundary.

## Related

