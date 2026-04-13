---
title: "Martin Fowler's software architecture guide"
date: 2019-08-25
captured: "2019-08-25T08:10:23Z"
tags: [architecture, software-design, martin-fowler]
source: "GitHub issue tieubao/til#445"
aliases: []
status: refined
---

## Context

Martin Fowler's curated guide to software architecture, serving as an entry point to his extensive writing on the topic. The guide frames architecture not as upfront design but as the shared understanding expert developers maintain about system design.

**Source:** [Software Architecture Guide](https://martinfowler.com/architecture/)

## Core thesis

"Architecture is about the important stuff. Whatever that is." The definition is deliberately subjective: architecture is whatever the senior developers on a project agree needs careful attention. It is not a separate discipline from programming; it is deeply intertwined with it.

## Key ideas

**Quality drives speed.** Poor architecture creates "cruft" (accumulated technical debt) that slows feature delivery. Investing in good internal quality pays off within weeks, not months. This is counterintuitive to managers who see architecture work as gold-plating.

**Architecture evolves.** Good architecture supports change rather than trying to predict it. The goal is to make future decisions easier, not to make all decisions upfront.

**Application vs enterprise scope.** Application architecture addresses design within a single deployable unit. Enterprise architecture coordinates across teams, codebases, and business units. Enterprise architects should work embedded in teams and advocate for decentralized decision-making rather than imposing top-down control.

**Social boundaries matter.** Applications are defined by organizational structure as much as by technical boundaries. This echoes Conway's Law: the system mirrors the communication structure of the organization that builds it.

## Topics the guide covers

- Microservices architecture and when (not) to adopt it
- Serverless and event-driven patterns
- Frontend architecture including micro frontends
- Domain-driven design as organizational tool
- Evolutionary architecture and fitness functions

## Related

- [[conways-law]] - social boundaries shaping technical architecture
- [[choose-boring-technology]] - pragmatic counterpoint to architectural ambition
- [[why-big-tech-is-slow]] - what happens when architecture accumulates feature interactions
