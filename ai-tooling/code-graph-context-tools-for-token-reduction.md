---
title: "Code graph context tools for token reduction"
date: 2026-03-26
captured: 2026-03-26T23:10:36.301Z
tags: ["ai-tooling", "token-optimization", "mcp", "code-graph"]
source: "Claude iOS session - SDD research session 3"
aliases: []
status: refined
---
## The problem

Without structural context, AI coding agents grep 40 files to answer a question that needs 5. Up to 80% of token budget spent on orientation, not problem-solving.

## Three tools solving this

| Tool | Mechanism | Token savings | Key feature |
|------|-----------|--------------|-------------|
| codebase-memory-mcp (CartoGopher) | tree-sitter AST parsing into SQLite graph. 14 MCP tools. | 40-95% | Incremental updates on file change. Multi-language. |
| vexp (Nicola Alessi) | AST dependency graph + passive session memory. Local-first. | 65-70% | Session memory persists discoveries between sessions. |
| Bito AI Architect | Knowledge graph of repo structure, data flows, module relationships. | Not published | Commercial. Replaces the agent, not complementary. |

## How the graph works

1. One-time indexing pass using tree-sitter AST parsing
2. Extracts every function, class, method, import, call relationship into SQLite
3. Exposed to agent via MCP tools (query by function name, trace callers, find dependencies)
4. Incremental updates: when you edit a file, only affected nodes re-index
5. Agent queries graph instead of grepping files. 200 tokens instead of 12,000.

## Evaluation verdict

- codebase-memory-mcp: **ADOPT** for projects 100+ files. Easy MCP install, low risk.
- vexp: **BOOKMARK**. Try codebase-memory-mcp first. Come back for session memory if needed.
- Bito: **SKIP**. Commercial, replaces Claude Code. Use open-source MCP tools instead.

## This is different from Context Hub / Context7

External docs tools (Context Hub, Context7) solve "the agent doesn't know the Stripe API." Codebase intelligence solves "the agent doesn't know YOUR code." Different problem, different layer.

#ai-tooling #token-optimization #mcp #codebase-graph

## Related

- [[context-hub-vs-context7-vs-the-context-layer-ecosystem]] - the "other half" of the context layer: external docs vs codebase intelligence
- [[ai-dev-stack-8-layer-model-march-2026]] - these tools live at L3.5a in the 8-layer stack
- [[mcp-tool-schema-caching-in-claude-ai-connectors]] - MCP schema patterns relevant to how these tools expose their graph via MCP