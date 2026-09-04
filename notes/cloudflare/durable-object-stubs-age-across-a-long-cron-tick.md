---
title: "Durable Object stubs age across a long cron tick"
date: 2026-09-04
captured: 2026-09-04T09:40:00Z
tags: [cloudflare, durable-objects, workers, debugging]
source: "Claude Code session"
aliases: [durable object reset because its code was updated]
status: refined
---

## Question

A Cloudflare Workers cron tick calls a Durable Object several times over a few minutes. The last call fails on every tick with `Durable Object reset because its code was updated`, yet nothing was deployed. Why, and what is the fix?

## Cause

A Durable Object stub is a client bound to one live instance of the object. When that instance is reset (a deploy, an eviction, a runtime move), every later call through the old stub answers with that message. The error text names one common trigger, a code update, but the same failure follows any reset.

The trap is stub age. A tick that obtains one stub at the start and reuses it minutes later, after a paced pass with retries, hands the runtime a stale client. The deploy that reset the object may have happened long before; the stub only notices on its next call.

## Fix

- Take a fresh stub per call: `env.NS.get(env.NS.idFromName(key))` right before each RPC, never once per tick.
- Bound each RPC to seconds, not minutes. Split a long pass into chunks (a handful of addresses per hop) and loop in the Worker with a fresh stub per chunk.
- Keep retry ladders inside the RPC client, so a single hop never spans the window where a reset can land.
- Log one summary line per pass and one line per refused item with a safe reason; a silent pass made this look like a rate limit for hours.

## Key Takeaway

A stub is a connection, not a handle. Reacquire it per call and keep every hop short.
