---
title: "OpenClaw virtual company pattern"
date: 2026-04-18
captured: 2026-04-18T16:25:01.815Z
tags: ["openclaw", "multi-agent", "architecture", "agents"]
source: "Claude.ai chat"
---
The "virtual company with roles" pattern (CEO, CTO, PM, Engineer, QA agents) is an **OpenClaw** idiom, not a Hermes one. It's not an official framework feature called "virtual company." It's a convention people built on top of OpenClaw's agent-and-subagent primitives.

## Three layers of the setup

![OpenClaw virtual company pattern](https://assets.han-ws.workers.dev/i/2026/04/openclaw-virtual-company-pattern.svg)

**Layer 1: Persistent agents.** Long-lived "people." Each has its own directory under `~/.openclaw/agents/<agentId>/` with six markdown files that shape its identity:

| File | Purpose |
|------|---------|
| SOUL.md | Personality and behavioral philosophy. Tone, style, boundaries, rules. 200-500 words max |
| IDENTITY.md | Factual identity card. Name, role, creator, version, specialization. 5-15 lines |
| AGENTS.md | Tool instructions. Which tools the agent can use, how, what to never touch |
| USER.md | What the agent knows about the owner. Preferences, project context |
| MEMORY.md | Persistent notes the agent writes over time. Changes most frequently |
| TOOLS.md | Capability definitions |

These six files get concatenated into the system prompt at boot. That's the entire "personality injection" mechanism. The agent is an LLM with a long system prompt saying "you are Forge the Engineering Assistant, here are your rules and memories, now respond."

**Layer 2: The orchestrator.** One agent designated "main." Owner talks to it through Telegram, Discord, Slack. It has its own SOUL.md that tells it how to delegate and when. When it decides a task needs specialist help, it spawns a sub-agent or routes to a persistent specialist.

**Layer 3: Sub-agents.** Background workers spawned for a single task. Run in their own session with their own toolset, do the job, post the result back to the parent, auto-archive. Short-lived contractors vs the persistent agents as full-time staff.

## What "CEO, CTO, marketing lead" actually means

Worst-kept secret: the "CEO" is a markdown file.

```markdown
# SOUL.md - Ethan, Chief Strategist

## Vibe
A seasoned strategist who thinks in quarters, not days. Comfortable with
ambiguity, uncomfortable with unsupported claims.

## Personality rules
- Ask three clarifying questions before scoping anything
- Always produce a written brief, never verbal only
- Delegate implementation, never do it yourself
- Escalate to the owner when cost exceeds $X or risk exceeds Y

## Tools allowed
- web_search, reddit, notion_read, gmail_read
- delegate_task (to spawn Engineer, QA, Research sub-agents)

## Tools denied
- terminal, filesystem_write, gmail_send (must escalate first)
```

That's it. The "company" is as sophisticated as the person writing the SOUL files. Well-written SOUL for a CTO role produces an agent that asks architectural questions. Lazy SOUL produces an agent that says "sure, boss" to everything.

## Delegation flow

User messages main agent: "I want to ship a landing page for the skills library."

1. Main agent (CEO SOUL) parses the ask. Decides this is product first, then engineering, then QA.
2. Calls `delegate_task` to PM sub-agent with scoped brief: "Define success criteria and user flows for a landing page that markets X to Y. Return a spec."
3. PM sub-agent runs in its own fresh conversation, no knowledge of prior context except what was passed in. Produces spec. Returns summary.
4. Main agent reads summary, hands to Engineer sub-agent: "Implement spec at path, using stack."
5. Engineer runs, terminals, git, builds. Returns what it did.
6. Main agent dispatches QA sub-agent to verify.
7. Main agent synthesizes all sub-agent summaries and reports back.

**Critical detail from the docs**: sub-agents know nothing about the parent's conversation. They start fresh. The parent has to pass every relevant detail explicitly in the goal and context fields. "Fix the bug we were discussing" fails because the sub-agent has no clue what bug. Feature (context isolation prevents cross-contamination) and constant source of failures (people forget to pass context).

## What people run this way

Honest list based on documented write-ups:

- **Multi-role coding teams**: strategist + engineer + QA tester + reviewer mirroring a human team. Works for well-scoped, repeatable tasks.
- **Research intelligence teams**: coordinator + parallel researchers + synthesizer. Given a topic, produce a report in 15 minutes. Probably the most reliably useful pattern because tasks decompose cleanly.
- **Personal assistants with household members**: each family member binds their own persistent agent on their own messaging account. Shared gateway, isolated memories.
- **Legal or compliance bots**: 177 production SOUL templates at `awesome-openclaw-agents`, including practice-specific legal personas with built-in "never give legal advice" guardrails.

Works best when sub-tasks have crisp inputs/outputs and can run in parallel. Works worst when tasks are interdependent, require long shared context, or need judgment that doesn't compose.

## Six failure modes in practice

This is where most write-ups go quiet and where the real learning lives.

1. **Context starvation in sub-agents.** People forget to pass enough context. Sub-agent has a fresh system prompt and the goal, nothing else. Either hallucinates the missing context or asks clarifying questions that go nowhere because it has no user to ask. Output is confident and wrong.

2. **Cost blowouts.** Every sub-agent is its own LLM session with its own context. Five-role teams can burn 5-10x the tokens of a single agent for the same task because they re-explain context to each other. With Claude Opus at $5/$25 per MTok, moderately complex tasks hitting three sub-agents cost real money. April 4 subscription change made this much worse for OpenClaw users.

3. **Runaway spawning.** Hermes docs explicitly block recursive delegation (sub-agents can't spawn their own sub-agents) because early multi-agent systems would fork-bomb themselves. OpenClaw allows it with rate limits. People still hit the limits.

4. **Persona drift.** SOUL.md says "you are opinionated and push back on vague specs." Model, especially a smaller one, drifts toward agreeable helpful-chatbot mode over a long session. By hour three the "CEO" is a yes-machine.

5. **Coordination overhead exceeding the work.** For any task short enough that a single agent could just do it, the multi-agent version is slower, more expensive, more error-prone. People keep building elaborate companies for tasks that would take a single agent 30 seconds.

6. **The "looks impressive on Twitter" failure.** Half the viral multi-agent demos are theater. Someone builds a six-role "startup" that writes one landing page and tweets it, implying this scales to running a business. It doesn't. Gap between toy company and actually useful multi-agent workflow is two orders of magnitude of engineering.

## How Hermes does the same idea differently

From the Hermes docs:

| Aspect | OpenClaw | Hermes |
|--------|----------|--------|
| Persistent personas | Yes, via SOUL.md + six-file workspace per agent | No. Single-identity agent, sub-agents scoped to tasks |
| Sub-agent spawning | Yes, via delegation | Yes, via `delegate_task` with isolated sub-agents |
| Sub-agent context | Fresh, goal + context passed explicitly | Same: fresh, zero knowledge of parent conversation |
| Persona files for sub-agents | SOUL.md + IDENTITY.md + AGENTS.md + USER.md injected | AGENTS.md + TOOLS.md only, NOT SOUL.md |
| Memory across sub-agents | Isolated per-persona workspaces | Single shared memory and skills library |
| Recursive delegation | Allowed with rate limits | Hard-blocked |
| True multi-agent architecture | Shipped | Tracked as future umbrella issue #344, not shipped |

Translation: Hermes bet on "one smart agent with a learning loop." OpenClaw bet on "many role-specialized agents talking to each other." Different bets on what makes agents more useful.

## The honest take

This pattern is interesting intellectually and mostly wrong operationally for most solo setups.

Three reasons:

1. **If you already have the human version of this** (contractor model, task board, spec-driven workflow), bolting on a virtual AI version adds coordination overhead without replacing anything.
2. **The SOUL.md + persona convention is portable.** That's the part worth stealing. Version-controlled markdown files defining agent identity (persona, identity, tool allow/deny, memory) is clean and maps directly onto a skills repo. If you want to give Claude Code different modes (strict reviewer, permissive prototyping, book-editing), the OpenClaw convention is a good template.
3. **The "watch agents chat to each other" appeal is mostly theater.** Seeing PM, Engineer, QA bots message each other in Slack feels like watching an org run. It's dopamine, not output. The real question: does multi-agent produce better final results than a single well-prompted agent for your tasks? Honest answer: for well-decomposable parallel tasks (research, benchmarking multiple options), yes. For everything else, no.

To experiment hands-on: small OpenClaw instance with three SOUL files for a concrete task, measure whether it beats a single Claude Code session with a thorough prompt. That's the experiment that tells you whether the pattern is worth anything, not how the agents "feel" chatting.