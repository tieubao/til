---
title: "Threat-model split: cross-tenant isolation vs per-agent damage containment"
date: 2026-05-04
captured: 2026-05-04T16:15:00+07:00
tags: ["security", "threat-modeling", "agents", "sandboxing", "architecture"]
source: "agentkernel + Hermes brainstorm 2026-05-04"
aliases: []
status: refined
---

**Two threats around AI-agent sandboxing look similar and need different solutions.** People (including me, recently) conflate them. The conflation produces architecture choices that solve a problem you don't actually have while leaving the actual problem unaddressed.

The threats:

| Threat | What it really is | Boundary lives between |
|---|---|---|
| **Cross-tenant isolation** | I run multiple agents, each owned by me, each scoped to different data. I do not want agent A to read agent B's data when prompt-injected. | Agent A and agent B (multiple agents I trust individually) |
| **Per-agent damage containment** | I run one agent (a coding assistant). I do not trust its tool calls. I want a hard wall so its rm -rf or accidental git push cannot hit the host. | One agent and the host (one agent I do not trust to act safely) |

Both feel like "isolate the agent." Both reach for the word "sandbox." The mental shortcut is to assume one tool solves both. It does not.

## Concrete examples

**A friend recommended `agentkernel`** (microVM-per-task wrapper around Apple Containers / Firecracker) framed as "prevent the coding agent from destroying my computer." That's threat 2 for one agent — Claude Code or similar. Solving it well: ephemeral microVM per task, default-isolated filesystem, default-isolated network. The agent gets its job done in a short-lived sandbox; the blast radius if it goes wrong is the sandbox's bind-mounts.

**My problem the same week** was three Hermes Agent daemons on a Mac mini (one for ops, one for family, one for IP work), each scoped to different data. That's threat 1 — multiple agents, each individually trusted, but the boundary is between them, not around any one of them. Solving it well: separate macOS users (UID + POSIX), or separate VMs / containers if the trust model goes beyond POSIX.

If I'd taken the friend's recommendation literally and put each Hermes inside an agentkernel sandbox, I'd have failed:

- agentkernel sandboxes are short-lived single-task containers; Hermes is a long-running daemon with persistent Telegram listeners + Notion tokens + sessions.db
- The threat I'm defending against (prompt injection in agent A reads agent B's data) doesn't exist within a single sandbox; it's an inter-tenant property
- Even if I forced it to work (long-lived sandboxes, persistent state inside each), I'd be paying for a boundary I don't need (between the agent and the host) while not building the boundary I do need (between agent A and agent B)

## The diagnostic question

When someone proposes a security tool for "your agent problem," ask one sentence:

> "Is the boundary between agent and host (one agent, host doesn't trust it), or between agent and other-agent (many agents, none trusts the others)?"

If you can't articulate this in one sentence, you might be conflating the two threats. If the proposer can't either, the tool recommendation is operating on vibes.

The same tool can sometimes serve both threats with different configurations. Apple Containers per tenant can give you cross-tenant isolation (each tenant in its own VM) AND per-agent damage containment (each agent's actions confined to its container's mounts) simultaneously. But that's a deliberate choice to over-pay for isolation, not a free win — multi-user POSIX would solve the cross-tenant threat at ~3-5x lower cost if your tenants are mutually trusted.

## Layering

The two threat solutions are usually **complementary**, not substitutes:

```
┌─────────────────────────────────────────────────┐
│ Cross-tenant boundary (multi-user OR containers)│  ← solves threat 1
├─────────────────────────────────────────────────┤
│   Tenant A's agent     │  Tenant B's agent      │
│   ┌──────────────┐     │  ┌──────────────┐      │
│   │ damage-      │     │  │ damage-      │      │ ← each can have
│   │ containment  │     │  │ containment  │      │   its own threat-2
│   │ sandbox      │     │  │ sandbox      │      │   solution if needed
│   └──────────────┘     │  └──────────────┘      │
└─────────────────────────────────────────────────┘
                  │
                  ▼
              Host system
```

Tenant A could itself run a coding-agent inside an agentkernel sandbox; tenant B could just run its workflow directly. The cross-tenant boundary (between A and B) is one design; the per-agent containment (around each agent's actions inside its tenant) is another.

## Where I went wrong before catching myself

In a planning session 2026-05-04, I almost wrote a SPEC where agentkernel was the cross-tenant isolation mechanism for the three-Hermes-on-Mac-mini case. The friend's framing ("agentkernel for security") was sticky enough that I almost adopted it as the cross-tenant solution. The save was articulating, in writing: "agentkernel solves damage containment for one coding agent. It does not solve cross-tenant isolation between three persistent daemons." Once I wrote that sentence, the right architecture became obvious (multi-user POSIX for the cross-tenant problem; agentkernel reserved as a per-agent containment tool for daily Claude Code).

The sticky bit is that "isolate" sounds like one concept. "Isolate from whom" is the question that splits it.

## Useful framings to keep on hand

- **"Isolate from whom?"** Forces the threat-model split.
- **"What's the trust model between the agents?"** If they're all you, multi-user is enough. If one is a less-trusted operator, escalate to VMs.
- **"Is this a long-lived daemon or a short-lived task?"** Damage-containment tools are usually optimized for short tasks; cross-tenant tools for long-lived daemons.
- **"What's the worst thing the boundary should prevent?"** If it's "agent A reads B's filesystem," cross-tenant. If it's "agent runs `rm -rf`," damage-containment.

## The bigger lesson

In design discussions about AI-agent security, the word "sandbox" is almost always doing more work than it should. Forcing the threat-model split out into the open early prevents you from architecting around the wrong threat. Two questions, one decision tree, less wasted work later.

## Related

- [[agentkernel-broken-flags-on-apple-containers]] — concrete example of agentkernel as a damage-containment tool (and its current limitations)
- [[apple-containers-overview-the-macos-native-microvm-runtime]] — one tool that can serve either threat depending on how you configure it
- [[macos-multi-user-cost-myth-gui-vs-service-users]] — when cross-tenant isolation is the right threat, multi-user POSIX is usually enough
- [[opt-in-beats-all-in-for-coding-agent-sandboxing]] — practical example of damage-containment design (variant 2 = opt-in sandbox)
