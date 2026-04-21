---
title: "deepagents vs OpenClaw vs Hermes: category positioning"
date: 2026-04-19
captured: 2026-04-19T06:47:59.034Z
tags: ["ai", "agents", "langchain", "evaluation"]
source: "Claude.ai chat"
---
Evaluating `langchain-ai/deepagents` against Hermes Agent and OpenClaw, and deciding whether it belongs in the stack.

## Context

The question that triggered this: "help me understand deepagents and how it fits into my current stack (Hermes, OpenClaw)."

The framing of the question contained a category error worth unpacking. `deepagents`, OpenClaw, and Hermes Agent are not peers. They occupy different layers and compete on different axes. Treating them as alternatives to each other leads to bad adoption decisions.

## The three tools in one line each

- **`deepagents`** (LangChain, MIT, 9.3k stars): a Python *library* that gives you a pre-wired LangGraph agent with planning, virtual filesystem, shell, and subagents baked in. No UI, no daemon, no persistence beyond LangGraph checkpointers. `pip install deepagents; create_deep_agent()`.
- **OpenClaw** (MIT, 163k stars): a *persistent daemon* wired to messaging platforms (Telegram, Slack, Discord, WhatsApp, etc.). Config-first via `SOUL.md`. Runs 24/7, remembers across sessions, takes actions on external services.
- **Hermes Agent** (Nous Research): a persistent daemon plus skill runtime. Skills live in `~/.hermes/skills/` as `SKILL.md` files, compatible with the `agentskills.io` open standard. Self-improving loop: after a complex task succeeds, the agent proactively creates a skill.

## The category insight: they are NOT peers

`deepagents` is a *library layer* thing. You import it into your own Python service. It has no messaging surface, no daemon, no 24/7 loop.

OpenClaw and Hermes are *runtime platform layer* things. They ship as products you run, not code you import. You could, in theory, use something like `deepagents` as the agent logic *inside* a runtime like OpenClaw. They don't compete; they stack.

![Stack positioning of deepagents, OpenClaw, and Hermes Agent](https://assets.han-ws.workers.dev/i/2026/04/deepagents-vs-openclaw-vs-hermes-stack-positioning.svg)

The confusion comes from surface-level similarity: all three use tool-calling LLMs, all three do multi-turn agentic work, all three let you plug in custom capabilities. But the shape of the deliverable is different. `deepagents` gives you a function you call from Python. OpenClaw and Hermes give you a process that runs and listens.

## Use cases mapped to winners

Each of the five situations below implies a different tool. If a project doesn't match any of them, none of these three is the right answer.

![Use case to tool mapping](https://assets.han-ws.workers.dev/i/2026/04/deepagents-vs-openclaw-vs-hermes-use-case-mapping.svg)

## Verdict matrix

| Tool | What it is | Adopt? |
|------|-----------|--------|
| `deepagents` | Python agent harness, LangGraph-based | **Read the source, steal the pattern.** The "planning tool + virtual FS + subagents" model is worth internalizing. Don't adopt the library unless building a Python backend agent. |
| Hermes Agent | Persistent daemon + skill runtime + `agentskills.io` standard | **Keep watching.** Already aligned with the skills format in `dwarvesf/claude-skills`. This is the one to watch for skill interchange format emergence. |
| OpenClaw | Persistent daemon + messaging gateways + `SOUL.md` config | **Ignore for now.** Cloudflare Workers AI direction is better for the Agentic Inbox use case. Don't port to `SOUL.md`; it's OpenClaw-internal. |
| Claude Code + `dwarves-kit` | Current solo-dev SDLC | **Keep building.** Nothing else fits this slot. |

## What this is NOT

- **Not a Claude Code replacement.** Claude Code is a CLI with IDE/git integration and human-in-the-loop by design. `deepagents` is a library for headless autonomous agents. Different category.
- **Not an OpenClaw/Hermes replacement.** No messaging gateways, no daemon, no persistent personal-agent pattern. If that's what you want, OpenClaw or Hermes.
- **Not a skill interchange format player.** `deepagents` has no skill concept, no SKILL.md, no marketplace. Tools are plain Python functions passed to `create_deep_agent(tools=[...])`. If you're watching the interchange format debate, Hermes is the one to watch; `deepagents` doesn't enter that conversation.

## LangChain baggage caveat

`deepagents` inherits the full LangChain dependency tree. The Deep Agents FAQ is deliberately careful to say "provider agnostic" and "built on LangGraph" because LangGraph is the grown-up, production-ready part of the LangChain ecosystem. But if you adopt `deepagents`, you're pulling in `langchain-core` regardless. For teams that have been burned by LangChain's over-abstraction and churn, that's a real cost to weigh against the harness convenience.

## Key takeaway

Category position determines the decision. Ask which layer you need, not which framework is hot. Library-layer problems (backend agent inside a service) get `deepagents` or raw LangGraph. Runtime-layer problems (always-on messaging agent) get OpenClaw or Hermes. SDLC-layer problems (human-in-the-loop coding) get Claude Code. If three "competing" tools solve three different problems, they aren't actually competing.

The most valuable move from `deepagents` isn't adoption, it's studying the source. The "planning tool + virtual filesystem + subagents with isolated context + trust-the-LLM" pattern is the open-source lecture notes for why Claude Code works. Steal the pattern; don't necessarily adopt the library.

## Related

- [[hermes-agent-comprehensive-briefing-april-2026]] - the runtime-platform alternative this note differentiates against
- [[hermes-vs-openclaw-competitive-scene-april-2026]] - the head-to-head this note expands by adding the library/runtime axis
- [[openclaw-virtual-company-pattern]] - OpenClaw's persona/SOUL idiom that deepagents does not implement (different category)
- [[ai-tooling-stack-synthesis-april-2026]] - the broader synthesis where this category-positioning insight fits as a refinement of cluster 4
- [[ai-dev-stack-8-layer-model-with-tool-evaluations-march-2026]] - deepagents lives at L3 (library) vs OpenClaw/Hermes at L3-L5 (runtime + orchestration)
- [[tool-evaluation-5-question-rubric]] - Q1 ("which layer?") is exactly the question this note answers for deepagents