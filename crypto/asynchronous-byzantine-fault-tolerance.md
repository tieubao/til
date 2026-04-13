---
title: "asynchronous Byzantine Fault Tolerance"
date: 2021-05-02
captured: 2021-05-02T14:38:34Z
tags: [blockchain, decentralization, algorithm, consensus]
source: "GitHub issue tieubao/til#549"
aliases: [aBFT]
status: refined
---

## Context

Byzantine Fault Tolerance (BFT) is the foundational consensus problem for decentralized systems. Computing evolved from single machines to connected networks to centralized services to decentralized systems without central authority. The internet is inherently decentralized, which creates the security problem that BFT attempts to solve.

**Source:** [Asynchronous Byzantine Fault Tolerance - Crypto Insights](https://medium.com/@crytpol_25852/asynchronous-byzantine-fault-tolerance-a-time-independent-future-proof-byzantine-fault-f6f1a4d1f17a)

**Attachment:** [Asynchronous Byzantine Fault Tolerance.pdf](https://github.com/tieubao/til/files/6411570/Asynchronous.Byzantine.Fault.Tolerance.A.Time-independent.Future-proof.Byzantine.Fault.Tolerance.for.the.Brave.New.World.by.Crypto.Insights.Medium.pdf)

## Evolution of BFT consensus

**Classic BFT** - Could only reach deterministic consensus if fewer than 1/3 of nodes were malicious. This constraint held for three decades and was only practical for permissioned networks.

**Bitcoin consensus (2009)** - Introduced probabilistic consensus in a permissionless setting, tolerating up to 50% Byzantine nodes. This was a massive improvement over the 1/3 threshold of classical BFT.

**Synchronous BFT variants** - Protocols like optimized pBFT (Zilliqa), delegated BFT (Neo), and PoS BFT (EOS) improved on classic BFT but still assume synchronous network conditions. This makes them unsuitable for truly permissionless blockchains where "network links can be unreliable, network speeds change rapidly, and network delays may even be adversarially induced."

**Asynchronous BFT (aBFT)** - Removes timing assumptions entirely. Works even when network conditions are unpredictable or adversarially manipulated. This is the strongest fault tolerance model for permissionless blockchain settings.

## Key comparison

| Model | Byzantine tolerance | Network assumption | Setting |
|-------|--------------------|--------------------|---------|
| Classic BFT | < 1/3 nodes | Synchronous | Permissioned |
| Bitcoin PoW | < 50% hash power | Probabilistic | Permissionless |
| Sync BFT variants | < 1/3 nodes | Synchronous | Semi-permissioned |
| Async BFT (aBFT) | < 1/3 nodes | None (time-independent) | Permissionless |

## Why it matters

The old internet achieved security through permissioned access. Decentralized systems cannot rely on this approach. aBFT provides the strongest theoretical foundation for consensus in adversarial, permissionless environments because it makes no assumptions about message delivery timing.

## Related
