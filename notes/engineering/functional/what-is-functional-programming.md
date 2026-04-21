---
title: "what is functional programming"
date: 2016-01-06
captured: 2016-01-06T17:59:09Z
tags: [functional]
source: "GitHub issue tieubao/til#110"
aliases: []
status: refined
---

**Source:** [What Is Functional Programming? - Kris Jenkins](http://blog.jenkster.com/2015/12/what-is-functional-programming.html)

## Context

Kris Jenkins offers a clean, practical definition of functional programming that cuts through the academic fog. The key insight is framing FP around side effects rather than language features.

## Definition

Functional programming is writing pure functions - removing hidden inputs and outputs as far as possible, so that as much code as possible describes a relationship between inputs and outputs.

## Hidden inputs and outputs

Every function has two sets of inputs/outputs:

1. **Declared** - parameters and return values visible in the function signature
2. **Hidden** - side effects and side causes; dependencies and changes not declared in the API

Example: a function reading from a queue has hidden input (queue state) and hidden output (queue modification), even though the signature shows neither.

## Pure functions

A pure function has:
- All inputs explicitly declared as parameters
- All outputs explicitly declared as return values
- No hidden dependencies or undocumented effects

## Why side effects matter

Side effects create a "complexity iceberg" where real dependencies are hidden beneath the surface. They prevent black-box testing and isolated verification. Debugging becomes difficult since changes could originate anywhere in the system.

The solution: surface complexity by explicitly declaring all inputs and outputs, making code behavior verifiable from its signature alone.

## What makes a language functional

A functional language "actively helps you eliminate side-effects wherever possible, and tightly control them wherever it's not." The distinction is not about having lambdas or map/reduce, but about how the language treats side effects.

## Related

