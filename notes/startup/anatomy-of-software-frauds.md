---
title: "anatomy of software frauds"
date: 2020-03-09
captured: 2020-03-09T16:16:21Z
tags: [startup, tech, management]
source: "GitHub issue tieubao/til#484"
aliases: []
status: refined
---

## Context

A deep analysis of how startup fraud operates in the software industry, using Cloud Foundry as a case study. The article identifies a three-layer architecture common to tech fraud and offers practical protection strategies.

**Source:** [Anatomy of a Fraud](https://matt.sh/anatomy-of-a-fraud)

**Attachment:** [Anatomy of Startup Frauds.pdf](https://github.com/tieubao/til/files/4307728/Anatomy.of.Startup.Frauds.pdf)

## The three-layer architecture of tech fraud

**Software with unlimited scapegoats:** Complexity becomes cover for incompetence. Companies claim "software is hard" while selling products they know won't work, isolating customers to prevent them from discovering patterns of failure.

**Sales-driven culture (S.C.R.E.A.M.):** Revenue through aggressive selling trumps product quality. Sales commissions reach 5-10x employee salaries, incentivizing deception over delivery.

**Deceptive founding structures:** Companies leverage established brands, investor credibility, and manufactured proof-of-use (investors acting as customers) to create false legitimacy.

## Warning signs

- **Delayed live demos**: sales resistance to showing working products suggests hidden dysfunction
- **"Training required"**: if extensive consulting must follow purchase before assessing functionality, the product likely doesn't work independently
- **Routine reboots**: systems requiring regular full restarts indicate architectural failure
- **Complex abstractions**: layers of unnecessary interfaces increase customer confusion and vendor blame-shifting
- **Isolated customer base**: preventing customers from communicating enables hiding systemic failures

## Protection strategy

1. **Pre-sales skepticism**: demand live demos with your own changes; test failover repeatedly; require installation from scratch
2. **Post-sales documentation**: log every error; don't accept "one-time mistakes"
3. **Enforcement with deadlines**: escalate unresolved issues; threaten payment withholding and legal recovery

The core insight: "Only you can prevent software fraud" by replacing trust with documented evidence.

## Related

