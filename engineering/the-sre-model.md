---
title: "the SRE model"
date: 2017-07-31
captured: 2017-07-31T17:30:43Z
tags: [sre, devops, process, reliability]
source: "GitHub issue tieubao/til#314 + https://medium.com/@rakyll/the-sre-model-6e19376ef986"
aliases: []
status: refined
---

## Context

Jaana B. Dogan (rakyll) explains how the SRE model works at Google, distinguishing it from both traditional operations and the broader DevOps movement. The article clarifies when SRE support makes sense and how it scales with product maturity.

**Source:** [The SRE Model - Jaana B. Dogan](https://medium.com/@rakyll/the-sre-model-6e19376ef986)

**Attachment:** [The SRE model - Jaana B. Dogan - Medium.pdf](https://github.com/tieubao/til/files/1188334/The.SRE.model.Jaana.B.Dogan.Medium.pdf)

## What SRE actually is

SRE at Google is a fundamental rethinking of operations, not just a new title. It focuses on three pillars: velocity and productivity of engineers, performance and reliability of products, and health of the codebase and production environment.

The defining characteristic: **SRE support is voluntary, not mandatory.** Development teams initially own full responsibility, from design through deployment, monitoring, and on-call. SRE involvement increases only with product scale and criticality.

## Scalable support model

Rather than assigning SRE to every team, the model scales based on product maturity:

| Stage | SRE involvement |
|-------|----------------|
| New products | Development team handles all operations |
| Growing products | Part-time SRE support added |
| Large-scale systems | Full-time SRE integration |

## Infrastructure-first approach

Google provides a comprehensive infrastructure suite (networking, storage, auto-scaling, configuration management) staffed by software engineers with SRE support. This allows product teams to focus on their domain rather than operational basics.

## Key principles

- **Developer ownership persists.** Teams retain operational understanding even with SRE support. SRE does not absorb operational responsibility; it augments it.
- **Best practices sharing.** SRE contributes reusable tools, dashboards, and monitoring solutions that benefit the whole organization.
- **Sustainable focus.** SRE concentrates on impactful, scalable problems rather than supporting every product equally.
- **Cultural integration.** Engineers adopt SRE fundamentals through established infrastructure and mentorship, not mandates.

## DevOps vs SRE

The industry often conflates "everyone doing ops" (DevOps) with proper infrastructure engineering staffing. Effective SRE requires distinguishing between product teams and infrastructure teams while maintaining developer accountability for operational aspects. SRE is one specific implementation of DevOps principles, not a synonym.

## Related
