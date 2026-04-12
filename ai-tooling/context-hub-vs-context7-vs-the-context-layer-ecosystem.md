---
title: "Context Hub vs Context7 vs the context layer ecosystem"
date: 2026-03-26
captured: 2026-03-26T23:10:54.098Z
tags: ["ai-tooling", "context-layer", "mcp", "documentation"]
source: "Claude iOS session - SDD research sessions 2 and 3"
aliases: []
status: refined
---
## Context Hub (Andrew Ng, DeepLearning.AI)

CLI tool (`chub`) that gives coding agents curated, versioned API documentation. The agent's training data goes stale; Context Hub provides ground truth at inference time.

Key features: `chub fetch` pulls API docs, `chub annotate` saves workarounds to local registry (persists between sessions), `chub feedback` lets agents vote on doc quality (crowdsourced improvement loop).

Andrej Karpathy is listed as a contributor. 10k+ stars in first week. MIT licensed.

## Context7 (Upstash)

MCP server or CLI for version-specific library documentation. Broader coverage than Context Hub (9k+ packages). Two tools: `resolve-library-id` and `query-docs`. Free tier limited to 1k requests/month (was 6k, reduced Jan 2026).

## How they differ

Context Hub focuses on **external API specs** (Stripe, AWS, etc.) with an annotation/feedback loop. Context7 focuses on **library docs** (React, Next.js, Prisma) with version-specific filtering. They're complementary, not competing.

## llms.txt standard

The foundation underneath both. Proposed by Jeremy Howard (Answer.AI, Sep 2024). A markdown file at `/llms.txt` that provides LLM-friendly content for a website. Think robots.txt but for AI. Context Hub and Context7 are demand-side tools that consume this standard.

## Other tools in this category

- Docfork: 9k+ libraries, volume play
- Ref Tools: session-aware filtering, ~5k token cap per query
- GitMCP: zero config, just a URL
- Deepcon: semantic search over package docs
- DeepWiki: architecture understanding of internal code (different angle)

#ai-tooling #context-layer #mcp #llms-txt

## Related

- [[code-graph-context-tools-for-token-reduction]] - the "other half" of the context layer: codebase intelligence vs external docs
- [[ai-dev-stack-8-layer-model-march-2026]] - these tools live at L3.5b in the 8-layer stack
- [[compaction-defense-patterns-for-claude-code-sessions]] - context injection strategies relate to how memory/docs enter the agent's context window