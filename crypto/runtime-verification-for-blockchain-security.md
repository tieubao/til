---
title: "runtime verification for blockchain security"
date: 2021-07-08
captured: 2021-07-08T03:52:16Z
tags: [blockchain, testing, formal-verification, security]
source: "GitHub issue tieubao/til#563"
aliases: []
status: refined
---

## Context

As DeFi TVL surpassed $52 billion by mid-2021, smart contract security became critical. In 2020 alone, $3.8 billion in crypto was stolen across 122 attacks. Traditional security audits catch many bugs but cannot guarantee completeness. Formal verification offers a fundamentally stronger approach.

**Source:** [IOSG Ventures](https://iosg.us19.list-manage.com/track/click?u=6da2cb3cdb401a25edb0c37d9&id=e82a700673&e=b7535d6284)

## Formal verification vs traditional audits

Traditional audits look for known vulnerability patterns. Formal verification uses mathematical models to prove or disprove that code behaves exactly as specified. It explores all possible system states systematically, yielding security guarantees that are orders of magnitude stronger than manual review.

Formal verification has historically been used in hardware design, embedded systems, aerospace, and military applications - domains where post-deployment fixes are impossible and failures are catastrophic. Blockchain smart contracts share these properties: once deployed, they handle real money and are difficult to patch.

## K Framework - the core technology

Runtime Verification built the K Framework, a rewrite-based executable semantic framework created by Grigore Rosu (UIUC professor, former NASA researcher). The key innovation: rather than requiring developers to rewrite code in a verification-specific language, K lets users formally verify code written in any programming language.

How it works:
- **Configurations** organize program state into labeled, nestable cells
- **Computations** carry computational meaning as sequentialized task structures
- **K rewrite rules** specify which parts of state are read-only, write-only, read-write, or irrelevant

This makes K suitable for concurrent languages and control-intensive features like exceptions and continuations.

## Why Runtime Verification stands out

**vs other formal verification teams** - Runtime Verification verifies at the bytecode level (directly on EVM), not source code level. This means the deployed bytecode is exactly what was verified, eliminating compiler bugs as a risk vector. Their "dynamic verification" provides higher safety than "static verification" approaches. K Framework is also more user-friendly and automated than Coq-based alternatives.

**vs traditional audit firms** (Trail of Bits, ConsenSys Diligence, Quantstamp) - These firms still rely primarily on traditional auditing methods. Some have started experimenting with formal verification elements (e.g., Trail of Bits' Manticore), but formal verification remains a fundamentally stronger approach.

**Notable clients** - Ethereum Foundation (Ethereum 2.0 beacon chain Gasper protocol), IOHK (KEVM and IELE), Algorand, and numerous DeFi protocols.

**Competitors in formal verification** - Certora, CertiK, Hevm. The open nature of blockchain development means competition accelerates progress across the field.

## Related

- [[asynchronous-byzantine-fault-tolerance]] - consensus protocols that can themselves be formal-verification targets (RV verified Ethereum 2.0's Gasper)
- [[double-spending]] - one class of attack formal verification can rule out at the protocol level
- [[undercollateralized-loans-in-defi]] - DeFi lending contracts are prime targets for formal verification given the dollar amounts at risk
- [[ethereum-token-standards-and-security-tokens]] - high-value token contracts where bytecode-level verification matters most
