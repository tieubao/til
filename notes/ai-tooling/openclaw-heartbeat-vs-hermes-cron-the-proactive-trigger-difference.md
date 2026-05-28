---
title: "OpenClaw heartbeat vs Hermes cron: the proactive trigger difference"
date: 2026-05-28
captured: 2026-05-28T09:24:42.583Z
tags: ["ai", "openclaw", "hermes"]
source: "Claude.ai chat"
---
Both OpenClaw and Hermes Agent can act without being explicitly prompted, but they do it through fundamentally different mechanisms. OpenClaw uses a judgment-based heartbeat loop; Hermes uses a cron scheduler. Conflating the two leads to wrong expectations about how autonomous each one actually is.

## How each one triggers itself

OpenClaw runs a heartbeat: the gateway daemon wakes on a configurable interval (every 30 minutes by default, hourly with Anthropic OAuth), reads a checklist from `HEARTBEAT.md`, and decides for itself whether any item warrants action. It either messages the user or replies `HEARTBEAT_OK` (silently dropped). It is open-ended: "survey the situation, figure out if I should do something." External events (webhooks, cron, teammate messages) can also fire the same loop. This judgment-based polling is what produced the well-known emergent behaviors, e.g. an agent autonomously drafting and sending a legal rebuttal to an insurance denial without being asked.

Hermes runs a built-in cron scheduler with delivery to any connected platform, for daily reports, nightly backups, weekly audits, and morning briefings, all running unattended. It is deterministic: it executes predefined jobs at predefined times. The agent is not roaming a checklist deciding what is worth doing; it runs the automation that was set up in advance. Closer to traditional cron-with-an-LLM-attached than to a self-directed survey loop.

## The difference at a glance

| Dimension | OpenClaw heartbeat | Hermes cron |
|-----------|--------------------|-----------------|
| Trigger type | Polling loop on an interval | Scheduled jobs at set times |
| Decision model | Reads a checklist, uses judgment to decide what to act on | Executes the job already defined |
| Open-endedness | Open-ended, emergent, can surprise you | Bounded, does only what is scheduled |
| Predictability | Lower, autonomy is emergent | Higher, deterministic |
| Failure mode | Acts on something you did not intend (liability risk) | Misconfigured schedule, but no surprise actions |
| Mental model | "Wake up and decide what needs attention" | "Run this task at this time" |

![OpenClaw heartbeat vs Hermes cron](https://assets.han-ws.workers.dev/i/2026/05/openclaw-heartbeat-vs-hermes-cron.svg)

## A separate autonomy axis

Where Hermes goes further than OpenClaw is not scheduling but parallelism: it can spawn isolated sub-agents for parallel workstreams, each with its own conversation and terminal, collapsing multi-step pipelines into zero-context-cost turns via RPC. That is delegation, not proactive triggering, and shouldn't be confused with heartbeat-style autonomy.

## Verdict

If the appealing property of OpenClaw's heartbeat is "it surveys my situation and decides what needs attention," Hermes's cron does not natively replicate that. You would have to encode the judgment into a scheduled job's prompt (e.g. a job that runs "review my inbox and act on anything urgent"). OpenClaw ships the judgment loop as a default operating mode; Hermes makes you construct it as a scheduled prompt. Choose the heartbeat model when you want emergent, self-directed action and can tolerate surprises; choose cron when you want predictable, bounded automation.

## Caveat on confidence

The absence of an OpenClaw-style judgment-polling loop in Hermes is inferred from the Hermes marketing site (which documents the cron scheduler and says nothing about a heartbeat), not confirmed against the Hermes GitHub README. Treat the "no heartbeat in Hermes" claim as documented-not-verified. To confirm, check github.com/NousResearch/hermes-agent directly.