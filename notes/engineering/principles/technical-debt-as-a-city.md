---
title: "technical debt as a city metaphor"
date: 2018-07-12
captured: 2018-07-12T09:10:59Z
tags: [technical-debt, architecture, metaphor]
source: "GitHub issue tieubao/til#381 + https://ebaytech.berlin/a-city-of-technical-debt-58568747b12"
aliases: []
status: refined
---

## Context

eBay Tech Berlin published this piece using the metaphor of a city to explain how technical debt accumulates, spreads, and eventually makes a codebase uninhabitable. The original article is no longer available online.

**Source:** [A City of Technical Debt - eBay Tech Berlin](https://ebaytech.berlin/a-city-of-technical-debt-58568747b12) (original site defunct)
**Attachment:** [A city of technical debt - eBay Tech Berlin.pdf](https://github.com/tieubao/til/files/2187898/A.city.of.technical.debt.eBay.Tech.Berlin.pdf)

## The city metaphor

A codebase is like a city. Individual buildings (modules) are built at different times, with different standards. Streets (interfaces) connect them. Over time, some neighborhoods decay while others are renovated. Nobody planned the city as a whole - it grew organically.

Technical debt is like deferred city maintenance: ignoring it long enough transforms manageable upkeep into structural collapse.

## How debt accumulates

- **Rushed construction** - shipping features without proper design, like building without permits
- **Changing requirements** - what was a good design becomes a poor fit as the city grows
- **Knowledge loss** - original architects leave, and new builders do not understand the foundations
- **Patch culture** - fixing symptoms instead of causes, adding workarounds on top of workarounds

## Why it spreads

Debt is contagious. A poorly designed module forces every module that depends on it to work around its limitations. One bad "building" degrades the entire "neighborhood." Teams start avoiding certain areas of the code, which accelerates decay.

## Managing debt

1. **Make debt visible** - track it explicitly, not as vague backlog items
2. **Allocate capacity** - dedicate a consistent percentage of engineering time to debt reduction
3. **Prioritize by impact** - focus on debt that blocks the most teams or causes the most incidents
4. **Prevent new debt** - code reviews, design docs, and standards reduce the rate of accumulation
5. **Refactor incrementally** - do not attempt full rewrites; renovate one building at a time

## Related

- [[write-code-easy-to-delete]] - complementary philosophy on managing code lifecycle
- [[benefits-of-continuous-delivery]] - CI/CD practices that help contain debt
