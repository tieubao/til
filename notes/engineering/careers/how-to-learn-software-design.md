---
title: "how to learn software design and architecture"
date: 2019-11-17
captured: 2019-11-17T16:49:15Z
tags: [architecture, software-engineering, design-patterns, learning]
source: "GitHub issue tieubao/til#463 + https://khalilstemmler.com/articles/software-design-architecture/full-stack-software-design/"
aliases: []
status: refined
---

## Context

Khalil Stemmler presents software design and architecture as a progressive nine-stage learning stack. The fundamental goal of software is to continually produce solutions that satisfy user needs while minimizing effort required to maintain and evolve them.

**Source:** [How to Learn Software Design and Architecture](https://khalilstemmler.com/articles/software-design-architecture/full-stack-software-design/)

## The nine-stage stack

**1. Clean code.** Developer mindset (empathy and craftsmanship), coding conventions (naming, testing, refactoring), and pattern knowledge. Code is written to serve end users, but also for teammates and future maintainers.

**2. Programming paradigms.** Master OOP, functional, and structured paradigms. OOP for boundary crossing and architectural flexibility. Functional for data management at application edges. Structured for algorithm implementation.

**3. Object-oriented programming.** Enables plugin architectures and flexible designs. Core value: creating rich domain models that encapsulate business logic.

**4. Design principles.** SOLID, composition over inheritance, DRY. These act as railguards preventing common OOP pitfalls. Apply thoughtfully, not dogmatically.

**5. Design patterns.** Creational (object instantiation), structural (component relationships), behavioral (communication). Warning: patterns add complexity; use only when genuinely needed (YAGNI).

**6. Architectural principles.** Component design, policy vs. implementation detail separation, and boundary identification.

**7. Architectural styles.** Structural (component-based, monolithic, layered), messaging (event-driven, pub-sub), distributed (client-server, peer-to-peer).

**8. Architectural patterns.** DDD, MVC, Event Sourcing. Implement specific styles with tactical detail.

**9. Enterprise patterns.** Advanced patterns for large-scale systems.

## Key mindset shifts

- Write for future maintainers, not just current functionality
- Simplicity first; add complexity only when justified
- Align architecture style with domain complexity and system requirements
- Climb methodically; each stage builds foundational understanding for the next
- Apply YAGNI aggressively; defer complexity until requirements demand it

## Related

- [[programming-practices-principles]] - overlapping principles on software craft
- [[mastering-programming]] - related perspective on progressive skill development
