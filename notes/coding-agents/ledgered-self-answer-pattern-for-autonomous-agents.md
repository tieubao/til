---
title: "The ledgered self-answer pattern: let the agent decide, never invisibly"
date: 2026-07-31
captured: 2026-07-31T23:55:00+07:00
tags: ["coding-agents", "autonomy", "workflow", "decision-records"]
source: "Claude Code session, orchestrator-loop build + prior-art survey"
aliases: ["decision ledger for agents", "self-answer mode"]
status: refined
---

**An autonomous coding agent does not need permission to make planning decisions. It needs an audit trail.** Every tool in the field picks one of two bad extremes, and the useful pattern sits in the unclaimed middle.

## The two camps, and why both lose

Camp one: **stop and ask.** spec-kit inserts `[NEEDS CLARIFICATION]` markers a human must resolve. Kiro and Devin pause on low confidence. A recent decision-rights paper goes further and forbids an agent from finalizing its own decision at all. This preserves human control and kills unattended operation: an overnight run that stops on its first ambiguous question at 1 a.m. delivers nothing by morning.

Camp two: **decide and move on.** Most autonomous loops (the Ralph family, task-queue runners, issue-to-PR resolvers) self-plan with no clarifying step and at best leave thin, unstructured logs after the fact. Assumption tags in generated specs and auto-generated decision logs exist, but none carry the fields an overrider needs: what was the question, what was chosen, why, and what would reverse it. This preserves throughput and hides every judgment call inside a transcript nobody rereads.

A 2026 paper ("Ask or Assume? Uncertainty-Aware Clarification-Seeking in Coding Agents") measures the ask-vs-assume tradeoff on SWE-bench Verified and names the missing half explicitly: nobody has built the structured logging side of assume.

## The pattern

Three parts, each load-bearing:

1. **An explicit opt-in gate per work item.** The operator marks a task as eligible for self-answered planning (a tag on the backlog row). Untagged work keeps the human interview. The agent can never grant itself the mode; the human decides where autonomy applies, item by item.
2. **One structured ledger row per self-answered question.** At the planning step the agent answers its own interview with its best-recommended answer, and writes each one as a record: the question, the chosen answer, the why. The record is override-ready, a reviewer can disagree with a specific decision and reverse it, not archaeology through a transcript.
3. **A scheduled audit moment that drains the ledger.** The rows feed an existing review ritual (a weekly debt paydown works). Decisions get seen by a human on a cadence, not never and not synchronously.

The rule this enforces is worth stating in one sentence, because the sentence is the design: **the system does not enforce "never decide", it enforces "never decide invisibly."**

## Why this beats both camps

- Unattended runs complete: no 1 a.m. stall on a question the agent could answer sensibly.
- Wrong calls become ledgered decisions with a reversal path, not silent drift.
- The human's attention lands where it pays: reviewing a page of decisions weekly instead of answering interrupts nightly.
- The opt-in tag keeps the blast radius chosen: high-stakes work simply never gets the tag.

## Failure modes to design against

The gate must be operator-writable only: if the agent (or any bot-writable store) can add the tag, the gate is decoration. The ledger must have a reader before it has a writer: rows nobody drains are camp two with extra steps. And the audit moment needs a real cadence owner, or the ledger becomes an append-only conscience.

## Related

- [[opt-in-beats-all-in-for-coding-agent-sandboxing]]: the same shape (explicit narrow grant beats blanket policy) applied to sandboxing.
