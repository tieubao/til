---
title: "Synthetic ledger identities must never reach a receipt lookup"
date: 2026-09-04
captured: 2026-09-04T09:40:00Z
tags: [ledger, evm, deposits, debugging]
source: "Claude Code session"
aliases: [opening balance credit identity, eth_getTransactionReceipt invalid params]
status: refined
---

## Question

A custodial deposit ledger keys every credit by its on-chain transaction hash. An opening-balance pass then credits historical holdings with a synthetic identity such as `opening:<chain>:<address>:<token>`. From that moment the chain's detection pass reports zero scans and one error per tick. What happened?

## Root cause

The reorg checker walks every credited row and asks the RPC for the transaction receipt behind its identity. A synthetic identity is not a hash, so `eth_getTransactionReceipt` answers `-32602 invalid params`. The per-chain catch treats that as a chain failure, the whole pass is discarded after the credits are written, and the chain's summary line disappears. Credits and the scan cursor still advance, which is why nothing looked stuck.

Two compounding faults hid it for a day:

- The chain error log printed only the error's name, a bare `Error`, so a malformed parameter looked identical to a throttle.
- The opening pass had no test that fed its identities through the reorg path.

## Fix

- Guard at the boundary that consumes identities as hashes: skip or branch on any identity that is not `0x`-prefixed before calling the receipt RPC.
- Log a redacted reason with the HTTP status and the JSON-RPC error code, never the endpoint (it carries the key), so `-32602` and `429` read differently.
- Give every identity producer a test that runs its output through every consumer that assumes a hash.

## Key Takeaway

Any field that is usually a transaction hash needs a type at the boundary, not a naming convention. When a new producer mints a different shape, every reader that assumed the old shape breaks quietly.
