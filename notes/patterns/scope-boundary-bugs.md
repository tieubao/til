---
title: Scope-boundary bugs, when the gate guards the wrong set
date: 2026-07-20
captured: 2026-07-20T12:00:00Z
tags: [patterns, anti-pattern, debugging, correctness, caching, deduplication]
source: Personal debugging notes, four independent defects found in one week and paid down together
aliases: [wrong-set bugs, dedup scope gap, global key for scoped resource]
status: refined
---

# Scope-boundary bugs, when the gate guards the wrong set

## Context

Four defects showed up in one week across unrelated projects. Each was filed separately
and fixed separately. Reviewing them together, they turned out to be one bug wearing four
costumes, and recognizing the family is far more useful than any individual fix. A fifth
instance was shipping the same day, in code written by someone who had the measurement for
the first instance sitting unread in his queue.

## The pattern

Any gate, dedup, cache, or ledger answers a question about a SET:

- "have I seen this before?" (dedup)
- "is this authorized?" (permission record)
- "does this conform?" (conformance gate)
- "what did this cost?" (metrics ledger)

The bug is never in the answer. It is in the set. The code consults a set that does not
match the set the invariant is actually about.

```
                the set the code actually consults
                ┌───────────────────────┐
   the set the  │                       │
   invariant    │   ┌───────────┐       │   too NARROW -> false "new" / false "clean"
   is about ────┼──►│  overlap  │       │   too WIDE   -> false "known" / false "allowed"
                │   └───────────┘       │   empty      -> unevaluable
                └───────────────────────┘
```

## Why these survive review

Every direction of the error is disguised as a good outcome:

- A **too-narrow** scope reports MORE work. Looks productive.
- A **too-wide** scope reports LESS friction. Looks efficient.
- An **empty** scope reports nothing at all. Looks free.

And the tests pass, because the tests were written against the same wrong set as the code.
There is no failing assertion to notice.

## Four shapes

**1. Dedup anchored to one surface out of many.** A proposal generator deduped candidates
against its own board only, while equivalent work was tracked on other boards and in
planning files it could not see. Measured precision: 23%. The instinct is to blame the
model for hallucinating. The measurement said otherwise: 41% of the batch was real work,
already tracked, just tracked somewhere invisible to the deduper. A visibility boundary,
not a reasoning failure.

**2. A global key for a scoped resource.** An override log keyed entries on a branch name
alone, with no project in the key, and checked it with a substring match. An override
recorded in one project satisfied the gate for an identically-named branch in a completely
different project, which then shipped with no verification of its own.

Two halves to the fix, and only the first is obvious. Obvious: put the isolation boundary
in the key. Non-obvious: anchor the LOOKUP to fields, not substrings, or a free-text field
containing the delimiter can shift the columns and forge a match. Widening the key without
anchoring the lookup leaves the same hole in a new shape.

**3. A gate with no ledger at all.** A quality gate ran, produced verdicts, and recorded
nothing. When the time came to evaluate whether it was worth its cost, the answer was
unobtainable: not "0% cost" but "0% measurable". The primitives to record it all existed;
nothing called them, because recording one round required three calls kept in sync and the
procedure never made them.

The fix that matters here is not "add logging". It is making the measurement field
MANDATORY and failing closed: a round that reports no cost is rejected outright and writes
nothing. Accepting a costless round would reproduce the original failure in a worse shape,
a ledger that looks complete and averages wrong, which ends the investigation instead of
merely delaying it.

**4. A fast path that consulted the wrong set entirely.** A shared long-lived browser
automation session had an already-connected fast path, `if (this.isConnected()) return;`,
which never compared the caller's explicit target against the one already attached. A
script asking for a scoped throwaway browser silently drove the live logged-in one. A
deliberately dead port also "succeeded".

This one earns an extra rule from what happened after the first fix. The first patch closed
the reported case. A review round found the same defect shape on three adjacent paths (a
sibling parameter not compared, the documented targeting option unchecked entirely, a
concurrent call riding the first one's target, and the auto-reconnect falling back to blind
detection). A later round found two more. Six doors into one room, three patches.

## The check

Before shipping anything that answers "have I seen this / is this allowed / does this
conform / what did this cost", write down two sets and compare them:

1. **The set the invariant is about.** Not what is convenient to reach: what would make the
   answer WRONG if excluded.
2. **The set the code actually consults.** Read the loop bound, the lookup key, the glob,
   the emit site.

If they differ, the direction of the difference tells you which lie the code will tell.

Four follow-on rules the instances earned:

- Put the isolation boundary IN the key, and anchor the lookup to fields, not substrings.
- A gate must loop the whole set it names, and run before the first mutation, not
  interleaved with it.
- Make the measurement field mandatory and fail closed.
- Fix every path that asks the question, not just the one that reported.

## The tell

In every instance, the wrong scope was the one that was easy to reach from where the code
sat. The deduper saw its own board because that was the object in hand. The fast path
checked "am I connected" because that was the flag already there.

Scope follows the author's cursor unless someone writes the set down first.

## Related

- [[redundant-api-pre-checks-in-wrapper-functions]] - another anti-pattern where the check
  and the thing being checked drift apart
