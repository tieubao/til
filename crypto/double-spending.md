---
title: "double spending in cryptocurrency"
date: 2018-08-17
captured: 2018-08-17T17:42:06Z
tags: [blockchain, decentralization, security]
source: "GitHub issue tieubao/til#386"
aliases: [double-spend, double-spend attack]
status: refined
---

## Context

Double spending is the fundamental problem that all digital currency systems must solve. It is the digital equivalent of counterfeiting: spending the same money more than once. Unlike physical cash, digital data can be copied, making this problem unique to digital currencies.

**Source:** [Double-spending - Wikipedia](https://en.wikipedia.org/wiki/Double-spending)

## How it works

Double spending creates inflation by generating additional copies of existing funds, damaging currency value and user confidence. The core challenge: how do you prevent someone from sending the same digital token to two different recipients?

**Centralized solution** - A trusted third party (like a bank) verifies every transaction against a ledger. Simple but creates a single point of failure for security and availability.

**Decentralized solution** - Multiple nodes maintain compatible copies of a public transaction ledger. When conflicting transactions appear, the network uses consensus protocols (proof-of-work, proof-of-stake) to determine which is valid.

## Attack scenarios

**Race attack** - An attacker sends two conflicting transactions simultaneously. Without waiting for confirmations, a merchant might accept a transaction that later gets reorganized out of the canonical chain. Mitigation: wait for multiple block confirmations before considering a transaction final.

**51% attack** - An entity controlling over 50% of network hash power can secretly mine a longer chain, then broadcast it to cause massive reorganization. This reverts legitimate transactions and allows the attacker to re-spend those funds on their chain.

## Notable incidents

| Year | Network | Impact |
|------|---------|--------|
| 2013 | Bitcoin | Bug in v0.8.0 caused chain split; $10,000 double-spend as proof-of-concept |
| 2018 | Bitcoin Gold | 51% attack, $18M in losses to exchanges |
| 2019-2020 | Ethereum Classic | Multiple 51% attacks; $1.1M attempted on Coinbase, $200K stolen from Gate.io |
| 2020 | Bitcoin Gold | Second 51% attack, $72K in losses |

## Key insight

Bitcoin uses probabilistic finality - transactions are never technically "final" because a conflicting chain can always outgrow the current one. More confirmations exponentially reduce the probability of a successful reorganization attack. This is why exchanges require multiple confirmations (often 6+ for Bitcoin) before crediting deposits.

## Related

- [[asynchronous-byzantine-fault-tolerance]] - the consensus theory that frames double-spending as a Byzantine generals problem
- [[stellar-vs-nano-comparison]] - two non-PoW protocols that prevent double-spending without 51% attack exposure
- [[runtime-verification-for-blockchain-security]] - smart contract bugs as a separate attack surface beyond consensus-level double-spending
