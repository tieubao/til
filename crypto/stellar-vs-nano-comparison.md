---
title: "Stellar vs Nano comparison"
date: 2021-06-17
captured: 2021-06-17T20:12:42Z
tags: [blockchain, investment, cryptocurrency, stellar, nano]
source: "GitHub issue tieubao/til#559"
aliases: []
status: refined
---

**Source:** [Reddit - Stellar vs Nano?](https://www.reddit.com/r/Stellar/comments/k0f3ho/stellar_vs_nano/)

## Context

A comparison of Stellar (XLM) and Nano (NANO), two cryptocurrencies that share surface-level similarities in speed and throughput but differ fundamentally in scope and architecture.

## Technical comparison

| Feature | Nano | Stellar (XLM) |
|---------|------|----------------|
| Transaction speed | 1-2 seconds | 3-5 seconds |
| Fees | Feeless | ~0.00001 XLM (~1/5000th of a cent) |
| Throughput | ~1,000 TPS | ~1,000 TPS |
| Consensus | Block-lattice (each account has own chain) | Federated Byzantine Agreement (FBA) |
| Scope | Currency only | Platform (tokens, DEX, smart features) |

## Architecture differences

**Nano** uses a block-lattice design where each account has its own blockchain. Transactions update individual ledgers, then balances are distributed to the main chain. This is optimized for one thing: fast transfers of a single native token.

**Stellar** runs on decentralized servers with a distributed ledger updated every 2-5 seconds across all nodes. The FBA algorithm lets each node choose a set of "trustworthy" nodes. Once a transaction is approved by all nodes within a trust set, it is considered approved.

## Stellar's platform advantages

Stellar goes beyond simple transfers with several protocol-level features:

- **Fee bumps** - one account can pay transaction fees for another
- **Sponsored reserves** - one account can cover the minimum balance for another
- **Asset authorization** - asset creators can regulate who can hold their asset
- **Claimable balances** - send assets to accounts that do not exist yet
- **SEP-30** - trustless key management across accounts
- **Custom tokens** - create tokens on Stellar like ERC-20 on Ethereum

These features target real-world adoption. A bank could convert all client accounts to Stellar without end users noticing.

## Institutional positioning

Stellar Development Foundation is a non-profit (unlike Ripple, which is for-profit). The CEO has presented to the US House Financial Services Committee. Speakers at Stellar Meridian have included representatives from the IMF, World Bank, World Economic Forum, and US Congress. USDC launched on Stellar, and institutions are already using it for remittances.

## Related
