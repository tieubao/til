---
title: "Ralph Loop pattern explained: persistent goals via file-based state"
date: 2026-05-09
captured: 2026-05-09T17:41:57.002Z
tags: ["agents", "hermes", "architecture", "ai-tooling"]
source: "Claude.ai chat"
---
# Ralph Loop pattern explained: persistent goals via file-based state

## What is Ralph loop

Full name: **Ralph Wiggum Loop**, named by Geoffrey Huntley in February 2024 after the dim-but-persistent Simpsons character who keeps stumbling forward regardless of failure. The naming is deliberate: the technique embraces being "deterministically bad in an undeterministic world."

The original form is one line of bash:

```bash
while :; do cat PROMPT.md | <ANY_CODING_AGENT> ; done
```

That's it. An infinite shell loop that re-feeds the same prompt file to a coding agent. The loop runs until external verification (test pass, build success, health check) confirms the task is done, not when the LLM says it's done.

## Why it's different from ReAct

The single most important property of Ralph loop: **it does not keep state in the context window**. State lives on disk in files and git history.

This solves what Huntley calls the "malloc/free problem" of LLM conversations:
- In traditional programming you `malloc()` memory and `free()` it when done.
- In an LLM context window, reading files, tool outputs, and conversation history all act like `malloc()`. There is no `free()`. You cannot selectively release context.
- As the conversation grows, context rot and compaction events corrupt the agent's grasp of the original spec.

Ralph's response: throw the context away every iteration. Each loop spawns a fresh agent with empty context. The agent reads the spec from `PROMPT.md`, examines the codebase on disk, takes one step, commits to git, and dies. Next iteration is a new agent reading the updated codebase. State survives across iterations entirely outside the LLM.

## ReAct vs Ralph comparison

![Ralph loop vs ReAct loop](https://assets.han-ws.workers.dev/i/2026/05/ralph-loop-vs-react-loop.svg)

| Aspect | ReAct loop | Ralph loop |
|---|---|---|
| State location | Context window | Files + git |
| Each turn | Append to history | Fresh context |
| Stop condition | LLM self-judges | External verifier |
| Failure mode | Context rot, drift, compaction | Token cost |
| Best for | Conversation, short debugging | Long-running tasks, AFK coding |
| Determinism | Undeterministic | "Deterministically bad" (predictable) |

## Application to Hermes Agent /goal feature

Hermes Agent's release feature `/goal — persistent cross-turn goals (Ralph loop)` is this pattern applied to a Telegram-based agent system.

**Without Ralph (ReAct-style):** A goal lives in the conversation context. Hits compaction → agent drifts. Session ends → goal lost. VPS restart → goal lost.

**With Ralph (`/goal` command):** Goal is written to persistent storage (file, DB, or Notion). Every subsequent turn — regardless of context resets, restarts, or time gaps — the agent:
1. Reads goal from storage.
2. Reads current progress state (what's done, what remains).
3. Executes one step.
4. Writes progress back to storage.
5. Replies via Telegram.

The goal survives anything that destroys context.

### Mapping to the Hermes 3-tier architecture

**Ops-agent** — `/goal "Audit Q1 contractor invoices, flag anomalies"`
- Iteration 1 (fresh context): read Notion ContractorPayables, list January
- Iteration 2 (fresh context): load goal + progress, list February
- Iteration N: generate report
- External verifier: Notion task marked done, or all months iterated

**Briefing-agent** — `/goal "Track Sarene Residence launch news daily 8am"`
- Cron triggers Ralph loop each morning
- Fresh context per run, agent reads goal, searches news, appends to briefing log
- External verifier: log file has entry dated today

**Trading-agent** — `/goal "Monitor BTC < 90k entry signal"`
- Loop runs at fixed interval, fresh context per tick
- Agent reads goal, fetches price, checks condition, writes status
- External verifier: price condition matched OR goal expired

## Catches

**Token cost.** Huntley reports shipping a $50K client MVP for $297 in API spend, but that's Claude Code with optimized prompts on greenfield code. Hermes Agent already has ~13.9K tokens of fixed overhead per call. A naive Ralph loop multiplies this by every iteration. Budget accordingly.

**External verifier is the heart of the pattern.** If the LLM decides when the goal is complete, the system collapses back to ReAct semantics regardless of how the loop is structured. Each tier needs a concrete completion signal:
- Ops-agent: Notion record state change
- Briefing-agent: log entry presence + timestamp
- Trading-agent: price/market condition match

Without an external verifier, Ralph loop runs forever and burns tokens.

**The "deterministically bad" framing.** Ralph isn't trying to be elegant per iteration. Each pass might do something stupid or redundant. The bet is that progress survives in files and git, so eventual consistency wins even when individual iterations are weak. This is a different mental model from traditional agent design and worth internalizing before adopting.

## References

- Geoffrey Huntley, "everything is a ralph loop" — https://ghuntley.com/loop/
- Original Ralph Wiggum Loop technique
- Vercel Labs reference implementation: https://github.com/vercel-labs/ralph-loop-agent