---
title: "what to know before debating type systems"
date: 2017-07-23
captured: 2017-07-23T16:27:08Z
tags: [type-systems, programming-languages, cs-fundamentals]
source: "GitHub issue tieubao/til#311"
aliases: []
status: refined
---

## Context

A guide to understanding type systems before engaging in the perennial static vs dynamic typing debate. Most arguments about type systems fail because participants use terms imprecisely or conflate different properties.

**Attachment:** [What To Know Before Debating Type Systems - Literate Programming.pdf](https://github.com/tieubao/til/files/1168197/What.To.Know.Before.Debating.Type.Systems.-.Literate.Programming.pdf)

## Key distinctions

**Static vs dynamic typing.** Static typing checks types at compile time. Dynamic typing checks types at runtime. This is about *when* checking happens, not *how much* checking happens. Dynamically typed languages are not "untyped"; they simply defer checks.

**Strong vs weak typing.** Strong typing means the language enforces type rules strictly (no implicit coercions between unrelated types). Weak typing allows implicit conversions. These terms are on a spectrum, not binary. Python is strongly and dynamically typed. C is weakly and statically typed.

**Type inference.** A language can be statically typed without requiring explicit type annotations. Haskell and ML infer types at compile time. The amount of annotation is a usability choice, not a property of static vs dynamic.

**Structural vs nominal typing.** Nominal typing (Java, C#) requires explicit type declarations to match. Structural typing (Go interfaces, TypeScript) matches based on shape. Two types are compatible if they have the same structure, regardless of name.

## Common fallacies in type debates

- **"Static typing means verbose code."** Type inference disproves this. Haskell code is often more concise than Python.
- **"Dynamic typing means no safety."** Runtime checks, tests, and contracts provide safety. The tradeoff is when errors are caught, not whether they are caught.
- **"Static typing catches all bugs."** Type systems catch a specific class of errors (type mismatches). They say nothing about logic errors, race conditions, or business rule violations.
- **"Types slow you down."** Depends entirely on the type system's expressiveness and the tooling built on top of it. A good type system with IDE support accelerates development.

## The real tradeoff

The useful question is not "which is better" but "what guarantees do I need, and what am I willing to pay for them?" Static types trade upfront annotation cost for earlier error detection and better tooling. Dynamic types trade later error detection for faster prototyping and more flexible metaprogramming. Both are legitimate engineering choices depending on context.

## Related
