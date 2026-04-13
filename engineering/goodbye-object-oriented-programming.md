---
title: "goodbye, Object Oriented Programming"
date: 2016-07-27
captured: 2016-07-27T17:26:25Z
tags: [functional, paradigm]
source: "GitHub issue tieubao/til#246"
aliases: []
status: refined
---

**Source:** [Goodbye, Object Oriented Programming - Charles Scalfani](https://medium.com/@cscalfani/goodbye-object-oriented-programming-a59cda4c0e53)

## Context

Charles Scalfani systematically dismantles the three foundational promises of OOP - inheritance, encapsulation, and polymorphism - arguing each has fundamental flaws that undermine the paradigm. The article advocates for functional programming and composition as alternatives.

## Inheritance problems

**The Banana-Gorilla-Jungle problem.** When reusing a class, you must include all parent classes and every object those classes contain. As Joe Armstrong put it: "You wanted a banana but what you got was a gorilla holding the banana and the entire jungle."

**The Diamond problem.** Multiple inheritance creates ambiguity when two parent classes implement the same method. Most OO languages simply prohibit multiple inheritance rather than solving it.

**Fragile Base Class problem.** Changes to a parent class can silently break derived classes without warning. Modifying internal implementation details, while maintaining the interface contract, can cause child classes to malfunction. This requires knowledge of parent class internals, which defeats the purpose of abstraction.

**The Categorical Hierarchy fallacy.** Real-world hierarchies are containment-based (folders within folders), not categorical (classifying objects by type). OOP misapplies an unrealistic organizational model to software design.

## Encapsulation limitations

Objects passed by reference to constructors remain accessible to the caller. Protecting encapsulation requires expensive deep cloning of all contained objects, which is impossible for objects with OS resource dependencies (file handles, sockets, etc.).

## Polymorphism is not unique to OOP

Interfaces provide polymorphic behavior without OOP's overhead. Polymorphism through interfaces or type classes does not require inheritance or encapsulation. FP languages achieve polymorphism cleanly without the baggage.

## The alternative

Composition over inheritance. Functional programming provides the tools to build composable, reusable abstractions without the systemic problems that plague OOP's core pillars.

## Related

