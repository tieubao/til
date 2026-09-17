---
title: "an incremental job must record confirmed absent"
date: 2026-09-17
captured: 2026-09-17T00:00:00Z
tags: [patterns, jobs, data, ingestion]
source: "Claude Code session"
aliases: ["job rescans the same items forever", "negative result caching", "absent versus failed"]
status: refined
---

An incremental job usually stores only what it found. Anything with no stored result is treated as not yet done, so the next run scans it again, finds nothing again, stores nothing again. The work never converges, and the cost per run stops falling even though the backlog is finished.

The missing piece is that a fetch which returned nothing is a result. It proves the item is absent, which is not the same fact as the fetch having failed:

| Outcome | What it proves | Next run should |
|---|---|---|
| Found | the value | skip |
| Confirmed absent | the source has nothing here | skip, until the source could plausibly change |
| Failed | nothing at all | retry |

Collapsing the middle row into the bottom one is what creates the permanent rescan. Keeping them apart needs the job to record absence with a timestamp, and to distinguish a clean empty response from a timeout, a 5xx, or a parse error. Only the clean empty response earns the absent marker.

Give the absent marker a re-check interval rather than making it permanent. Sources do gain data later, so absence is true as of a time, not forever.

## Key takeaway

Store negative results, and only the ones a successful request produced. An absent marker with no distinction from failure just hides the retries.

## Related

- [[an-upstream-identifier-can-be-duplicated]] - same ingestion-job family: a stored identity that trusts the wrong signal, keying on the source's number instead of on the traversal
- [[scope-boundary-bugs]] - the general form: the bug is in the set the code consults, not the answer it computes; collapsing "absent" into "failed" is one instance
