---
title: "which programming languages are functional"
date: 2016-01-06
captured: 2016-01-06T17:53:16Z
tags: [functional]
source: "GitHub issue tieubao/til#109"
aliases: []
status: refined
---

**Source:** [Which Programming Languages Are Functional? - Kris Jenkins](http://blog.jenkster.com/2015/12/which-programming-languages-are-functional.html)

## Context

Companion piece to Jenkins' "What Is Functional Programming?" that applies his side-effects-based definition to categorize real programming languages. The framework is simple: how well does a language help you eliminate side effects where possible and control them where necessary?

## The criterion

A single overarching measure: **side-effects management**. Not lambdas, not map/reduce, not static typing. The question is whether the language actively helps you minimize and control side effects.

## Language categorization

| Category | Language | Reasoning |
|----------|----------|-----------|
| Not functional | JavaScript | Actively encourages side effects through mechanisms like implicit `this` |
| Not functional | Java | Mandates localized side effects as a design cornerstone; stateful objects are essential |
| Partially functional | Scala | Bridges OO and FP, but reconciling "side effects mandatory" with "side effects forbidden" is questionable |
| Partially functional | Clojure | ~80% functional; targets time-related side effects through pervasive immutability and controlled mutation wrappers |
| Genuinely functional | Haskell | Most rigorous approach; type system makes all side effects explicit and visible in function signatures |

## Key insight

Awareness of side effects transforms programming perspective "from individual functions right up to overall systems-architecture." This is the fundamental distinction between truly functional languages and those that merely borrow functional syntax.

Having lambdas does not make a language functional. Having immutable-by-default data and a type system that tracks effects does.

## Related

