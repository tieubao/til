---
title: "why big tech companies are so slow"
date: 2025-05-06
captured: 2026-04-13T00:00:00Z
tags: [better-dev, big-tech, complexity]
source: "GitHub issue tieubao/til#614 + https://seangoedecke.com/difficulty-in-big-tech"
aliases: []
status: refined
---

## Context

Sean Goedecke explains why large tech companies ship slowly. The answer is not incompetence, bad processes, or laziness. It is the mathematical consequence of feature accumulation: each new feature potentially interacts with all the features before it.

**Source:** [Why are big tech companies so slow?](https://seangoedecke.com/difficulty-in-big-tech)
**Attachment:** [Why are big tech companies so slow? - sean goedecke.pdf](https://github.com/user-attachments/files/20054669/Why.are.big.tech.companies.so.slow.sean.goedecke.pdf)

## The core argument

Big tech slowness stems from feature complexity and interactions, not from people or process failures. As companies accumulate features, each new addition must be checked against all existing ones to prevent interference. A feature that takes one weekend at a startup may require a full week once dozens of other features exist.

This creates a combinatorial explosion of cognitive load that no amount of hiring or process improvement can fully offset.

## Common misconceptions (all wrong)

- Engineers are incompetent
- Agile processes are inherently wasteful
- Engineers are lazy
- Coordination problems dominate
- Scale requires fundamentally different rules

**Reality:** big tech companies continue hiring engineers because it remains profitable despite the apparent inefficiency.

## Wicked features

Certain features interfere with every other feature in the system. Adding new user types, deployment environments, or authentication models are classic examples. These typically unlock enterprise customers and drive major revenue, making them unavoidable despite their complexity costs.

## Revenue incentives explain everything

"1% of Google Ads or AWS S3 revenue is a lot of money." Marginal features matter enormously at scale. Unlike startups chasing product-market fit, established companies compete for incremental revenue gains through dozens of small features. The incentive structure guarantees complexity accumulation.

## How to spot this

When someone complains that Big Tech is slow, ask: how many existing features does each new feature need to be compatible with? The answer usually explains the timeline perfectly.

## Related

- [[hidden-dividends-of-microservices]] - one strategy for managing feature interaction complexity
- [[monorepo-advantages]] - how code organization affects cross-feature coordination
