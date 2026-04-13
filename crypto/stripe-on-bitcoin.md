---
title: "Stripe on Bitcoin as the IP layer of payments"
date: 2019-02-07
captured: 2019-02-07T11:26:51Z
tags: [blockchain, bitcoin, payments]
source: "GitHub issue tieubao/til#407"
aliases: []
status: refined
---

## Context

In 2014, Stripe became the first major payments company to support Bitcoin. They published a vision for Bitcoin as global payments infrastructure, then in 2018 dropped Bitcoin support due to rising fees and confirmation times. These two blog posts together tell a compelling story about crypto's payments promise and its practical limitations.

**Source:** [Stripe - Bitcoin the Stripe Perspective](https://stripe.com/blog/bitcoin-the-stripe-perspective) and [Ending Bitcoin Support](https://stripe.com/blog/ending-bitcoin-support)

## The vision: Bitcoin as the IP layer of payments

Money has three functions: store of value, unit of account, and medium of exchange. Stripe argued Bitcoin's strongest potential was as a medium of exchange - specifically as infrastructure for moving value globally.

**The gateway model** - Users interact with local gateways that speak their native currency. Behind the scenes, gateways convert to Bitcoin, transmit, and convert back. This creates a global interconnected payments network without requiring each participant to navigate foreign regulatory landscapes.

The analogy: just as the internet connected isolated local networks via a common protocol (IP), Bitcoin could connect isolated financial systems via a common value transfer protocol. Users would have payment addresses like email addresses (alice@cad-gateway.com), while Bitcoin addresses handle the plumbing underneath.

**Why Bitcoin over existing rails** - Swift requires ad-hoc fee negotiation and currency conversion between peered banks. A closed network like PayPal lacks structural pressure to improve. Bitcoin's open ecosystem enables rapid iteration and compounding improvements from any participant.

## The trust problem

Comparing to Visa, the gateway network provides the transaction layer but not the trust layer. Visa's value proposition to consumers: "if you see the Visa logo, you are safe." The Bitcoin ecosystem would need centralized "trust providers" offering chargeback mediation, consumer protection, and merchant vetting - essentially recreating the card network trust layer on top of crypto rails.

This creates a paradox: the decentralized payment network naturally produces centralized trust providers who set rules and regulations.

## The retreat (2018)

After four years, Stripe dropped Bitcoin support because:
- Block size limits caused transaction confirmation times to rise substantially
- Fiat-denominated transaction failure rates increased (price fluctuates during confirmation)
- Fees rose to tens of dollars per transaction, comparable to bank wires
- Customer demand to accept Bitcoin declined; revenues from Bitcoin payments dropped

Stripe remained optimistic about Lightning Network, Ethereum, Stellar, and potential Bitcoin variants that could keep fees low and settlement fast.

## The lesson

Bitcoin evolved to prioritize being an asset (store of value) over being a medium of exchange. This was not a failure - the Bitcoin community made deliberate trade-offs. But it meant the "IP layer of payments" vision required different technology than Bitcoin's base layer could provide.

## Related
