---
title: "Memory systems as agent harness plugins"
date: 2026-04-07
captured: 2026-04-07T17:53:59.810Z
tags: ["ai", "memory", "agents", "orchestration", "openclaw"]
source: "Claude.ai chat"
---
## Overview

Memory systems and agent orchestration harnesses are converging. The integration pattern has standardized around lifecycle hooks: memory plugins hook into the agent loop at two points (before-turn for recall, after-turn for capture). This is the same pattern as deterministic enforcement hooks in spec-driven development, applied to memory.

![Agent harness memory hooks](https://assets.han-ws.workers.dev/i/2026/04/agent-harness-memory-hooks.svg)

## The lifecycle hook pattern

Agent harnesses (OpenClaw, LangGraph, CrewAI, SwarmClaw) all expose lifecycle hooks that fire at specific points in the agent loop. Memory systems plug into exactly two:

**Before-turn (recall):** Before the agent sees the user's message, the memory plugin searches the store for relevant memories and injects them into the context. The agent reasons with memories already present. It never needs to call `memory_search` itself.

**After-turn (capture):** After the agent responds, the plugin extracts durable facts from the exchange and pushes them to the memory store. This runs asynchronously so it doesn't block the response.

The key architectural insight: memory control moves out of the agent loop and into the system layer. The agent doesn't decide what to remember. The harness enforces it.

## Concrete integrations

### OpenClaw + Mem0

The most widely deployed integration. Registration: `api.on("agent.turn", handler)`.

```json
{
  "openclaw-mem0": {
    "enabled": true,
    "config": {
      "autoCapture": true,
      "autoRecall": true,
      "userId": "alice"
    }
  }
}
```

Auto-recall silently injects memories before every response. Auto-capture extracts facts after every exchange. The agent never calls memory tools. All five explicit memory tools (search, store, forget, get, list) are also available for agent-initiated operations.

Multi-agent isolation: the plugin derives agent-specific user IDs from session keys using pattern `agent:<agentId>:<uuid>`.

Known issues in self-hosted mode: auto-recall silently discards memories (wrong property name in hook return), embeddings ignore custom baseURL (always hits api.openai.com).

### OpenClaw + MemOS

Same lifecycle pattern but with richer configuration. Supports configurable recall filters, knowledgebase IDs, multi-agent mode, relativity thresholds, and async mode. Launched as an official MemOS Cloud plugin in March 2026.

### OpenClaw + Memori

Launched March 13, 2026. Key differentiator: strips OpenClaw metadata, timestamps, and thinking blocks before storing to prevent context feedback loops where agent reasoning pollutes the memory store.

Provides structured SQL-native records instead of raw markdown files. Includes observability (what was stored, what was recalled, how memory is performing).

### LangGraph (different approach)

LangGraph doesn't use the plugin pattern. Memory is baked into the state graph. A centralized state system persists throughout the workflow, acting as shared memory accessible to all nodes. Each node reads and updates specific parts of the global state.

You can still bolt on Mem0 or LangMem as external memory backends, but the native approach is state-graph-native rather than plugin-based.

### SwarmClaw (multi-agent)

Includes durable memory, reflection memory, human-context learning, document recall, and project-aware context. The multi-agent case is harder: multiple agents sharing a harness need memory isolation via namespaced user IDs.

### Claude Code / Cursor / Codex (via MCP)

Memory plugins also expose MCP interfaces so they can be used with coding agents:

```bash
claude mcp add --transport http memori https://api.memorilabs.ai/mcp/ \
  --header "X-Memori-API-Key: ${MEMORI_API_KEY}" \
  --header "X-Memori-Entity-Id: your_username" \
  --header "X-Memori-Process-Id: claude-code"
```

## The "same memory, any agent" vision

The emerging goal is a unified memory layer that works across all harnesses. Today each agent platform maintains isolated memory. An agent's knowledge dies with its session or stays locked inside one platform.

The vision: adapters for every harness (OpenClaw, LangGraph, CrewAI, Claude Code) backed by the same memory store. Memory follows the user, not the tool.

Current blockers:
- No standard memory wire protocol (each plugin is custom)
- Conflict resolution when the same fact is written by different agents
- Access control and provenance tracking across agents
- Memory lifecycle management (nothing decays today)

## Connection to spec-driven development

This is the same architecture as dwarves-kit's hook system:
- Hooks = deterministic enforcement at lifecycle boundaries
- Memory recall hook = "before" hook that injects context
- Memory capture hook = "after" hook that extracts and persists
- The agent doesn't manage memory; the system layer does

The pattern: separate the concerns. The agent focuses on reasoning. The harness handles state management. Memory is just another hook consumer.

## The race

OpenClaw is the dominant harness (9k to 157k GitHub stars in 60 days as of early 2026). Mem0, MemOS, and Memori all launched OpenClaw plugins within weeks of each other. The competition isn't about algorithms. It's about who ships the most reliable, lowest-friction plugin for the harnesses developers are already using.