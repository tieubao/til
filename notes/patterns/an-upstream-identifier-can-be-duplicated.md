---
title: "an upstream identifier can be duplicated"
date: 2026-09-17
captured: 2026-09-17T00:00:00Z
tags: [data, ingestion, patterns, debugging]
source: "Claude Code session"
aliases: ["official id is not unique", "dedup collapsed distinct records", "choosing a primary key for scraped data"]
status: refined
---

An identifier printed by the upstream source looks like the obvious primary key. It is numbered, it is sequential, it is what humans use to refer to the item. None of that makes it unique. Sources duplicate their own numbering: a record gets entered twice, a correction reuses a number, a human typed it.

Keying on it means dedup silently collapses two distinct records into one. Nothing errors. The collection is simply short, and by the time anyone notices, every later record is offset from its real position.

Two rules that cost nothing up front:

1. **Check uniqueness in the source before keying on it.** One count of distinct values against total values on the raw pull. If it does not hold there, it will never hold downstream.
2. **Prefer a key the traversal itself guarantees.** If records are discovered by walking an ordered listing, position in that walk is unique by construction. The upstream number becomes an attribute, useful for display and for flagging anomalies, never the identity.

The upstream number is still worth keeping. Once position is the key, a mismatch between position and printed number is a cheap signal that the source duplicated or skipped something, which is exactly the fact the old design was hiding.

## Key takeaway

Officialness is not a uniqueness guarantee. Verify it in the source, and prefer a key your own traversal produces.

## Related

- [[synthetic-ledger-identities-and-receipt-lookups]] - the opposite failure, a synthetic key where a real one was expected
- [[an-incremental-job-must-record-confirmed-absent]] - same ingestion-job family: a stored result that trusts the wrong signal, collapsing a distinct outcome into one that hides it
- [[scope-boundary-bugs]] - the general form: the bug is in the set the code consults, not the answer it computes; keying on a non-unique upstream identifier is one instance
