---
title: "wrangler --env deploys a second worker when no env block exists"
date: 2026-09-17
captured: 2026-09-17T00:00:00Z
tags: [cloudflare, workers, wrangler, deployment]
source: "Claude Code session"
aliases: ["wrangler deploy --env no env block", "stray worker named name-env", "wrangler environment not defined"]
status: refined
---

`wrangler deploy --env <name>` against a config that declares no `env.<name>` block does not error. Wrangler treats the flag as a request for a named environment and synthesises one, deploying a brand new Worker called `<name>-<env>` with the whole config attached: cron triggers, queue producers, bindings, routes. The real Worker is left untouched, so every symptom points away from the deploy.

The failure is loud in the wrong direction. The new script starts firing its own crons on schedule against production bindings, while the Worker that was meant to be updated still runs the old code.

Two habits cover it:

1. Dry run first and read the resolved name back against the `name` field in the config.

   ```
   wrangler deploy --env staging --dry-run
   ```

   If the printed script name is not the one in `wrangler.jsonc`, stop.

2. Treat a stray `<name>-<env>` script as an incident, not litter. It needs `wrangler delete` before its first cron tick, otherwise it is a second live writer against the same downstream resources.

## Key takeaway

A flag that names a configuration section which does not exist should be an error. In Wrangler it is a creation instruction, so the guard has to be the operator reading the resolved name, not the tool refusing.

## Related

- [[durable-object-stubs-age-across-a-long-cron-tick]] - another Workers runtime behaviour that only shows up on the scheduled path
