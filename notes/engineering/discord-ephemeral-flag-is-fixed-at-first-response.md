---
title: "Discord fixes a reply's ephemerality at the first interaction response"
date: 2026-09-05
captured: 2026-09-05T07:30:00Z
tags: [discord, interactions, bots, debugging]
source: "Claude Code session"
aliases: [flags 64 ignored on edit, deferred ack public follow-up, ephemeral interaction response]
status: refined
---

**Whether a Discord interaction reply is ephemeral is decided by the first response to that interaction, and only there.** A `flags: 64` (EPHEMERAL) sent on a later `PATCH /webhooks/{app}/{token}/messages/@original` is silently discarded. So a bot that answers a button or slash command with a public deferred acknowledgement (`DEFERRED_CHANNEL_MESSAGE_WITH_SOURCE` without the flag) has already committed every follow-up edit to the channel, however many times the edit asks for ephemeral.

The trap is that the edit call succeeds. There is no error, the message updates, the flag is just not applied, so the failure only shows up as "everyone can see this" in production.

## What to test

Assert on the acknowledgement, not on the edit. In a test for an ephemeral flow, the interaction's first response body must carry `flags: 64` (or `DEFERRED_CHANNEL_MESSAGE_WITH_SOURCE` with `data.flags: 64`). A test that checks the later edit payload for the flag passes while the real reply is public.

## Rule of thumb

Decide visibility before you ack. If a handler cannot know yet whether the result should be private, ack ephemeral and post a separate public message later when needed; the reverse is impossible.
