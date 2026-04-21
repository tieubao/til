---
title: "undercollateralized loans in DeFi"
date: 2022-05-26
captured: 2022-05-26T15:44:36Z
tags: [blockchain, decentralization, defi, lending]
source: "GitHub issue tieubao/til#576"
aliases: []
status: refined
---

## Context

DeFi lending has evolved beyond overcollateralized models. Undercollateralized loans allow borrowers to access funds with minimal or no collateral, opening up use cases like personal borrowing and microfinance that traditional DeFi lending cannot serve.

## Types of undercollateralized loans

DeFi has produced eight distinct approaches, each with different trust assumptions and trade-offs.

**1. Flash loans** - Borrow and repay within the same transaction. Zero default risk since both must complete atomically. Ideal for arbitrage, collateral swaps, and liquidations. Not suitable for personal borrowing. Protocols: Aave, Uniswap, DeFi Saver, Equalizer, Furucombo.

**2. Third-party risk assessment** - A high-liquidity holder vouches for the borrower. If the borrower defaults, the third party loses their staked collateral. Enables personal loans and microfinance, but requires a substantial pool of wealthy guarantors. Protocols: Maple, Dharma, TrueFi, Goldfinch, Bloom.

**3. Crypto-native credit scores** - Uses on-chain history (loan repayments, yield farming, trading activity) to assess creditworthiness. Powerful in theory, but requires identity disclosure that conflicts with blockchain anonymity. Zero-knowledge proofs may solve this tension. Projects: LedgerScore, Wing, Zoracles.

**4. Off-chain credit integration** - Imports traditional credit data to underwrite loans on-chain. Avoids the on-chain identity problem but raises questions about long-term practicality and decentralization. Protocol: Teller.

**5. Personal network bootstrap** - Restricts lending pool access to invited participants, creating a trust layer through social connections. Lenders control who enters. Protocols: Aave, Union, Akropolis.

**6. Real-world asset loans** - Tokenizes physical assets as NFTs to serve as collateral. Still early-stage and faces liquidity challenges for high-value items. Protocols: Centrifuge, Open DAO.

**7. NFTs as collateral** - Uses blockchain NFTs as loan collateral. Benefits from the growing NFT market, but collateral value is fragile and depends on NFT market sentiment. Protocols: Aave, Helio, Lendroid.

**8. Digital asset loans** - The protocol holds custody of borrowed assets until repayment. If the borrower defaults, the contract liquidates the position to cover losses. Protocol: Lendefi.

## Key trade-offs

| Approach | Trust assumption | Main risk |
|----------|-----------------|-----------|
| Flash loans | Atomic execution | No personal borrowing |
| Third-party vouching | Social/financial trust | Guarantor pool size |
| Credit scores | On-chain history | Privacy loss |
| Off-chain credit | Traditional data | Centralization |
| Personal network | Social invitation | Limited access |
| Real-world assets | Asset tokenization | Liquidity |
| NFT collateral | Market valuation | Volatility |
| Custodial | Protocol custody | Smart contract risk |

## Related

- [[ethereum-token-standards-and-security-tokens]] - the token primitives DeFi lending builds on
- [[runtime-verification-for-blockchain-security]] - lending protocols are prime formal-verification targets given the dollar amounts at risk
- [[token-emission-models]] - protocol token incentives shape lending pool economics
- [[cobie-on-33-and-crypto-incentives]] - DeFi exemplifies the cooperate-vs-defect dynamic Cobie describes
