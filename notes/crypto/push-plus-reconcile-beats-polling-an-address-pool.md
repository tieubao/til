---
title: "push plus reconcile beats polling an address pool"
date: 2026-09-17
captured: 2026-09-17T00:00:00Z
tags: [crypto, architecture, deposits]
source: "Claude Code session"
aliases: ["deposit detection polling cost", "address activity webhook", "how to watch many deposit addresses"]
status: refined
---

Deposit detection over a pool of permanent per-user addresses, polled on a timer, costs O(addresses x ticks). The pool only grows, the tick rate is fixed by how fast a deposit must be noticed, and almost every call returns nothing. The bill scales with how many users have ever signed up, not with how many of them are depositing.

The alternative is two mechanisms with different jobs:

```
chain --> address-activity webhook --> credit immediately        (cost: O(real deposits))
      \
       -> slow reconcile sweep      --> catch what the push lost  (cost: O(addresses) but rare)
```

Push handles the normal case at a cost proportional to actual deposits. The reconcile sweep, running on a much slower cadence, is what stops a missed or dropped push from becoming a lost deposit. Neither half is sufficient alone: a pure push path has no recovery from a webhook outage, and a pure poll path is the cost problem.

The reconcile also needs to be idempotent against the push, since both can credit the same transaction. Key the credit by transaction hash so a double detection collapses.

## Key takeaway

Polling scales with the watch list, push scales with the event stream. Use push for cost and a slow poll for durability; the poll is not a fallback you enable during an incident, it runs always.

## Related

- [[workers-logs-push-vs-poll-for-error-alerting]] - the same push versus poll tradeoff in an alerting pipeline
- [[synthetic-ledger-identities-and-receipt-lookups]] - what happens when a deposit ledger is keyed by something that is not a real transaction hash
