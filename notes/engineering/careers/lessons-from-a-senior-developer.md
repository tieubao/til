---
title: "things I learnt from a senior software engineer"
date: 2019-08-26
captured: 2019-08-26T14:41:17Z
tags: [software-engineering, career, best-practices, debugging]
source: "GitHub issue tieubao/til#447 + https://neilkakkar.com/things-I-learnt-from-a-senior-dev.html"
aliases: []
status: refined
---

## Context

Neil Kakkar documents practical lessons learned from working alongside a senior software engineer, covering everything from naming conventions to deployment strategies.

**Source:** [Things I Learnt from a Senior Software Engineer](https://neilkakkar.com/things-I-learnt-from-a-senior-dev.html)

## Writing code

**Names encode meaning.** A `GodComponent` name signals "dumping ground"; `LayoutComponent` clarifies actual responsibility. Good names highlight when refactoring is needed.

**Preserve context for future developers.** Documentation and comments preserve knowledge when original authors leave. One client lost data when a "useless" API endpoint was removed - it was actually used once yearly by journalists.

**Comments explain why**, not what. Strong comments cover why decisions were made, what non-obvious behavior occurs, and when specific conditions matter.

**Atomic commits.** Each commit should represent a meaningful, rollbackable unit of work.

**Deleting code carefully.** Safe to delete code you know won't execute. Risky to delete code you don't understand (Chesterton's Fence principle).

## Testing

- Test **behavior**, not implementation
- When bugs appear, add regression tests documenting "another thing that can go wrong"
- Environment mismatches between local, dev, staging, and production create hidden bugs

## Deployment and risk

**Deploy incrementally** rather than bundling all changes into one big release.

**Rollback first.** When failures occur: immediately rollback to last working state, then diagnose root cause, then fix and redeploy. This minimizes client impact.

**Treat servers as cattle, not pets.** Know exactly what runs on each machine. Recreate from scratch when needed.

## Debugging methodology

**Breadth-first before depth-first.** Check environmental factors before analyzing code: Is the machine running? Is correct code deployed? Is configuration enabled? Is routing correct? Does schema version match service version? Only then investigate code logic.

## Monitoring

Three components: **logging** (captures system state, iterated as bugs reveal missing info), **metrics** (quantifies behavior like query times and call counts), and **alarms** (alerts when thresholds breach).

"I can sleep well at night, knowing if something goes wrong, I'll be woken up."

## Related

- [[so-you-want-to-be-senior]] - complementary perspective on senior engineering
- [[lessons-learned-in-software-dev]] - similar collection of engineering lessons
- [[code-review-basics]] - related practices on code quality
