---
title: "pattern - Backends for Frontends (BFF)"
date: 2015-12-16
captured: "2015-12-16T10:49:28Z"
tags: [patterns, architecture, API, microservices]
source: "GitHub issue tieubao/til#92"
aliases: [BFF pattern]
status: refined
---

## Context

Sam Newman's description of the Backend for Frontend pattern, which addresses the problem of a single general-purpose API backend becoming a bottleneck when serving diverse client types (mobile, web, third-party).

**Source:** [Pattern: Backends For Frontends](http://samnewman.io/patterns/architectural/bff/)

## The problem

A single API backend serving mobile, desktop, and third-party clients accumulates conflicting requirements. Mobile needs minimal payloads and different call patterns than desktop. When multiple teams need changes to the same API, coordination overhead grows. The API becomes bloated, unfocused, and slow to evolve.

## How BFF works

Instead of one API for all clients, create dedicated backend services aligned to specific user interfaces. Each BFF is a server-side component tightly coupled to its corresponding frontend. It handles:

- Aggregation of downstream microservice calls
- Data transformation appropriate for that specific UI
- Parallel vs sequential call orchestration
- Graceful degradation when partial data is unavailable

## Design decisions

**One experience, one BFF.** If iOS and Android experiences differ substantially, use separate BFFs. If they share the same interaction patterns, one mobile BFF suffices. Team structure should drive this: aligned teams can share a BFF more easily than separate teams.

**BFF vs general-purpose API gateway.** API gateways consolidate concerns from multiple clients and tend toward bloat. BFFs are "single-purpose edge services" that keep each component focused and maintainable.

## When to use

- Multiple client platforms with distinct interaction patterns
- Microservice architectures requiring significant aggregation
- Teams separated between frontend and backend development
- External third-party integrations needing customized APIs

## Tradeoffs

| Advantage | Disadvantage |
|-----------|--------------|
| Reduced coupling between UI and backend teams | Potential duplication across BFFs |
| Smaller, focused codebases | Additional deployment complexity |
| Team autonomy in deployment | Extra network hops adding latency |

## Related

- [[creating-a-microservice-ten-questions]] - BFF is itself a microservice that needs these answers
- [[hidden-dividends-of-microservices]] - BFF enables frontend team autonomy
- [[conways-law]] - BFF pattern explicitly aligns service boundaries to team structure
