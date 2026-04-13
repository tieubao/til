---
title: "DevOps team topologies"
date: 2018-01-07
captured: 2018-01-07T11:07:18Z
tags: [management, devops, engineering]
source: "GitHub issue tieubao/til#346"
aliases: []
status: refined
---

## Context

Matthew Skelton's framework for understanding which team structures help DevOps flourish and which are anti-patterns. This became the foundation for the influential "Team Topologies" book.

**Source:** [What Team Structure is Right for DevOps to Flourish?](https://blog.matthewskelton.net/2013/10/22/what-team-structure-is-right-for-devops-to-flourish/)

**Source:** [DevOps Topologies](http://web.devopstopologies.com)

## Anti-patterns

**Separate silos (Anti-Type A):** Traditional "throw it over the wall" approach where dev and ops remain completely isolated. Harms software operability.

**DevOps silo (Anti-Type B):** Creating a distinct DevOps team that ironically widens the Dev-Ops gap rather than bridging it. Only acceptable as a temporary intervention.

**"We don't need Ops" (Anti-Type C):** Developers dismissing operational complexity as irrelevant, typically resulting in avoidable production failures.

## Recommended topologies

**Type 1 - Smooth collaboration:** The "promised land" of DevOps. Smooth collaboration between Dev and Ops teams. Requires strong technical leadership and shared organizational goals. High effectiveness.

**Type 2 - Fully embedded:** Operations personnel integrated directly into product teams. Best suited for organizations with a single main product (Netflix, Facebook model). High effectiveness.

**Type 3 - Infrastructure-as-a-Service:** Operations provides elastic infrastructure like AWS EC2. Pragmatic for traditional operations departments or cloud-dependent organizations. Medium effectiveness.

**Type 4 - DevOps-as-a-Service:** External service providers assist with infrastructure automation and operational guidance. Suitable for smaller teams. Medium effectiveness.

**Type 5 - Temporary DevOps team:** Time-limited team with explicit mandate to bridge Dev-Ops divide and become obsolete. Success depends on trusted leadership and clear transitional goals.

## Key success factors

- Product complexity and count
- Technical leadership strength
- Organizational culture change readiness
- Operational capability ownership

## Related

- [[conways-law]] - organizational structure shapes system design, directly relevant to team topologies
