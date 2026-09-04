---
title: "RPC provider API keys leak through error messages, not config"
date: 2026-09-04
captured: 2026-09-04T11:30:00+07:00
tags: ["security", "logging", "cloudflare-workers", "viem", "secrets"]
source: "Claude Code session, two production incidents 16 days apart"
aliases: ["viem HttpRequestError key leak", "API key in request URL"]
status: refined
---

**An RPC provider that authenticates by putting the API key in the request URL turns every error message into a credential leak, and the leak path is the error object, not the config.** Alchemy-style endpoints look like `https://<chain>.g.alchemy.com/v2/<key>`. viem's `HttpRequestError` message includes a `URL: ...` line with that full endpoint. Any `console.error(err.message)`, any `throw err` that the platform records as an uncaught exception, and any logging middleware that serialises the error writes the key into the log store. Cloudflare Workers Logs keeps it for the retention window, and any census tool that prints raw messages echoes it into the operator's terminal on the next triage.

## Why rotation alone does not close it

Rotating the key answers the exposure, not the class. The next 403 or timeout logs the new key the same way. Two incidents 16 days apart on the same worker, both found by a human reading a log census, before a structural fix landed.

## The fix that holds

1. One shared redaction helper: mask `/v2/<token>` path segments on provider hosts and provider-prefixed tokens (`alch_...`) in any string.
2. Apply it at **every log site and every rethrow site** downstream of the RPC client. The rethrow matters as much as the log: an uncaught exception's message is what the platform stores.
3. Leave the endpoint out of the client's own logs entirely (log method and status, never the URL).
4. Mask census output before reading it: pipe log queries through the same patterns so a leaked key never lands in a transcript.

## How to spot it

Grep the worker for `err.message`, `String(e)`, and bare `throw e` below any `createPublicClient` / `http(` transport or any REST call whose base URL carries a credential. Each one is a leak site until it goes through the helper.
