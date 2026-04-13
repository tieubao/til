---
title: "Conway's law"
date: 2019-03-31
captured: 2019-03-31T09:51:56Z
tags: [management, engineering]
source: "GitHub issue tieubao/til#416"
aliases: [Conway's Law]
status: refined
---

## Context

Conway's law is one of the most cited observations in software engineering, describing the relationship between organizational structure and system design.

**Source:** [Conway's law - Wikipedia](https://en.wikipedia.org/wiki/Conway%27s_law)

## Definition

> "Organizations which design systems are constrained to produce designs which are copies of the communication structures of these organizations." - Melvin E. Conway, 1967

In other words, the architecture of a software system will mirror the communication patterns of the team that built it. If you have four teams working on a compiler, you will get a four-pass compiler.

## Why it matters

Conway's law is not just an observation; it is a constraint. You cannot fight it. If your org chart does not match your desired architecture, the architecture will lose. This is why many companies deliberately restructure teams to match the system architecture they want (the "inverse Conway maneuver").

## How to spot this

- When system boundaries suspiciously align with team boundaries
- When integration problems cluster at the seams between teams
- When refactoring a system requires reorganizing teams first
- When a microservices migration stalls because the org is still structured as a monolith team

## Related

