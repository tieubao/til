---
title: "OAuth refresh tokens die when a grant is copied into several stores"
date: 2026-09-10
captured: 2026-09-10T15:30:00+07:00
tags: ["oauth", "security", "hermes", "debugging", "credentials"]
source: "Claude Code session, a nine-day staggered credential die-off"
aliases: ["refresh token rotation replay", "one grant one store"]
status: refined
---

**A rotating refresh token is a single-holder credential, not a shared secret.** Copying an OAuth store into more than one place creates a race the provider is designed to lose on purpose.

## Question

Why did a fleet of agent profiles sharing one xAI OAuth login lose the credential one profile at a time over nine days, and why did each re-login only buy a few more days?

## Investigation

Every profile's auth store showed the same shape: `tokens = {}`, `last_auth_error.relogin_required = true`, `invalid_grant` from the token endpoint. The timestamps were the clue. They were not simultaneous: one profile died on day 1, another on day 3, three on day 5, two on day 7, the last on day 9. A revoked account or a lapsed subscription kills everything at once. A staggered die-off means each store failed on its own schedule, the moment its access token expired and it tried to refresh.

The deploy script explained the rest: it copied the root `auth.json` into every profile directory on first deploy (`[ -f profile/auth.json ] || cp root/auth.json profile/`). Ten files, one grant.

## Root cause

OAuth providers that implement refresh-token rotation (xAI does) issue a new refresh token on every refresh and invalidate the previous one. Presenting an already-rotated refresh token is treated as replay, a token-theft signal, and the provider revokes the whole grant family.

With one grant copied into ten stores, the first store to refresh wins a fresh token; every other copy now holds a rotated-away token. Each of them dies the next time its access token expires. Eventually one of the dead copies replays, and the family is revoked, taking the winner down too. Re-authenticating and copying again restarts the same clock.

## Fix

Hold exactly one copy of the grant. In hermes-agent the engine already supports this: a profile with no own `providers.<oauth-provider>` block reads the root store's grant and writes each rotated token back through to root. The fix was to delete the `cp`, strip the per-profile copies on every deploy, and re-authenticate once at the root.

Order matters on re-auth: remove the dead pool entries first, then add the new grant. The engine stamps a sticky `last_auth_error` on the provider whenever any pool entry fails to refresh and never clears it on success, so a dead entry still in the pool after the new grant lands re-stamps the error seconds later.

## Key takeaway

A rotating refresh token is a single-holder credential, not a shared secret. Any process that copies an OAuth store (a deploy script, a `--clone-from` profile create, a backup restored beside the original) creates a race the provider is designed to lose on purpose. Monitor the auth store's own state (`relogin_required`, newest pool entry vs the error timestamp), not a log string that only appears when something happens to use the credential: a quiet night made a log-based probe declare the dead fleet recovered.

## Related

- [[rpc-provider-api-keys-leak-through-error-messages]] - same lesson from the other direction: monitor the credential's own state, not a log line that only fires on use
- [[secret-resolution-for-pi-agent-providers-via-1password-op-read]] - same domain, agent-fleet provider credentials, but for a static API key rather than a rotating grant
- [[age-and-1password-complementary-encryption-tiers]] - the general key-separation principle this is a special case of: a credential and its copy must not share a failure domain, here the failure domain is "who last refreshed"
