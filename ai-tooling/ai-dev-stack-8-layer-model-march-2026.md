---
title: "AI dev stack 8-layer model (March 2026)"
date: 2026-03-26
captured: 2026-03-26T23:09:30.592Z
tags: ["sdd", "claude-code", "dev-stack", "ai-coding"]
source: "Claude iOS session - SDD research sessions 2 and 3"
---
## The 8-layer model

After two research sessions surveying SDD frameworks, context tools, and workflow orchestration, the AI-assisted development stack resolves into 8 layers:

| Layer | Name | What lives here |
|-------|------|----------------|
| L5 | Orchestration / workspace | Multi-session management, parallel task coordination. Nimbalyst, Intent (Augment). Needed when 3+ agents run simultaneously. |
| L4 | Methodology / workflow | Two species: **spec-driven** (GSD, Spec Kit, OpenSpec, BMAD) and **role-driven** (gstack, ClaudeKit). They combine: use SDD for spec, gstack roles for review/QA. |
| L3.5a | Context: codebase intelligence | AST-level structural understanding of YOUR code. codebase-memory-mcp (tree-sitter + SQLite graph). 40-95% token savings. |
| L3.5b | Context: external docs | API/library docs for external services. Context Hub (Andrew Ng), Context7 (Upstash). Also: CLAUDE.md, skill files, MCP servers, Trail of Bits hooks. |
| L3 | Coding agent | Claude Code (primary). Alternatives: OpenCode, Codex, Gemini CLI. |
| L2.5 | Agent workspace / session manager | Sits between agent and IDE. tmux (manual), Nimbalyst (visual kanban), Claude Code Agent Teams (experimental). |
| L2 | IDE / editor | VS Code, Cursor, Windsurf, Kiro. |
| L1 | Terminal | tmux, Ghostty (recommended for long sessions), Kaku, Warp. |

**Separate axis:** AutoResearch (Karpathy loop) -- not a layer, an optimization pattern for anything with a measurable metric.

## Key insight: L3.5 is two problems

External API docs (Context Hub/Context7) solve "the agent doesn't know Stripe's latest API." Codebase intelligence (codebase-memory-mcp) solves "the agent doesn't know YOUR code's structure." Different data sources, different tools, different failure modes.

## Key insight: L4 has two species

Spec-driven tools produce artifacts (spec doc, architecture, task list). Role-driven tools produce judgment (CEO pushback, eng review, QA testing). They're complementary, not competing: use SDD for planning, gstack for review.

#ai-tooling #sdd #claude-code #dev-stack