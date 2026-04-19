---
title: "token emission models"
date: 2024-09-25
captured: 2024-09-25T04:12:44Z
tags: [cryptocurrency, tokenomics]
source: "GitHub issue tieubao/til#594"
aliases: []
status: refined
---

## Context

Overview of common token emission models used in cryptocurrency projects. These models govern how new tokens enter circulation over time, based on factors like initial supply, emission rate, and decay functions.

![Token emission models chart](https://github.com/user-attachments/assets/f1eb6178-9eec-4bb3-9e1e-0be6ac42bef9)

## Models

**Fixed supply with halving.** The emission rate halves after each halving period. Bitcoin is the canonical example. Creates a predictable, decelerating supply schedule that approaches a hard cap asymptotically.

**Exponential decay emission.** Emission decreases exponentially over time following a decay curve. Similar to halving but smoother, without the discrete step-function drops. Results in front-loaded distribution.

**Linear decay emission.** Emission decreases linearly over time until it reaches zero. Creates a more gradual reduction compared to exponential decay. Total supply is reached in finite time.

**Fixed supply with no emission.** After an initial distribution (via sale, airdrop, or other mechanism), there is no further emission. All tokens exist from genesis. Common in utility tokens and governance tokens.

**Constant emission.** A flat, constant rate of token emission over time. Supply grows indefinitely at a fixed rate. Inflation rate decreases as a percentage of total supply over time, even though absolute issuance stays constant.

**Bonding curve.** A mathematical relationship between supply and price. Token price changes as supply increases, typically following a curve where price rises with more tokens minted. Used in automated market makers and continuous token models.

## Design considerations

Each model creates different incentive structures. Halving and decay models reward early participants. Constant emission models are more egalitarian over time. Bonding curves create price discovery mechanisms. The choice of emission model fundamentally shapes a token's economic properties: inflation rate, wealth concentration, miner/validator incentives, and long-term sustainability.

## Related

- [[ethereum-token-standards-and-security-tokens]] - the standards on which most emission models are implemented
- [[cobie-on-33-and-crypto-incentives]] - tokenomics incentive critique that emission schedules directly shape
- [[undercollateralized-loans-in-defi]] - lending protocol tokenomics interact with emission curves
