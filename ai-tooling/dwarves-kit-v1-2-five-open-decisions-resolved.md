---
title: "dwarves-kit v1.2 five open decisions resolved"
date: 2026-03-29
captured: 2026-03-29T15:32:54.589Z
tags: ["dwarves-kit", "sdd", "gsd", "codebase-memory-mcp", "decisions"]
source: "Claude.ai session on SDD open decisions (March 2026)"
---
# dwarves-kit v1.2: Five open decisions resolved

## 1. Execution engine: STAY NATIVE

Verdict: Use Claude Code's native Task tool for subagent dispatch. Don't adopt GSD v2's Pi SDK runtime.

Why: The verification pipeline (task-verifier + fix-agent) addresses quality through post-task verification rather than programmatic context control. Avoids Node.js/TypeScript runtime dependency. Aligns with "bash over binaries" principle.

Revisit trigger: If crash recovery becomes a real pain after 3+ real `/execute` runs (sessions dying mid-task), evaluate GSD v2 as execution engine while keeping dwarves-kit's spec format and verification pipeline.

Steal from GSD v1: "aggressive atomicity" principle -- each task should fit in ~50% of a context window. Add this as a check in /spec-validate's Scope Critic.

## 2. Bootstrap command: COMPLETE

`/start` detects 8 project states and suggests the right command. `context-readiness.sh` v2 injects the same suggestion at SessionStart automatically. Both built and shipped in v1.2.

CCGS's "Collaborative Design Principle" (Question > Options > Decision > Draft > Approval) is a different pattern -- it's about how agents present choices before writing files. Already partially implemented in /think and /spec. Gap: worker subagents in /execute don't follow this pattern. Park until real usage shows workers making unwanted decisions.

## 3. Graph-based codebase context: ADOPT codebase-memory-mcp

Verdict: Install codebase-memory-mcp on any Dwarves project with 50+ source files.

Why: Single static binary (C + tree-sitter), no Docker, no Node.js, no API keys. 66 languages -- handles Go + TypeScript + SQL together. 120x token reduction for structural queries (3,400 vs 412,000 tokens). One-command install auto-configures Claude Code MCP entry. context-readiness.sh already warns about missing codebase index.

Skip vexp for now. Session memory is nice-to-have; orientation cost is the core problem.

## 4. Agent layer: COMPLETE for v1.2

Current agents: task-verifier (read-only verification), fix-agent (targeted fixes).
Not needed yet: docs-writer (/docs handles it in main session), security-auditor (/review covers it).

Future candidates (Phase 3):
- 3 parallel reviewer agents for /review-team (security, architecture, test-coverage)
- Research agent for brownfield /spec (GSD v1's 4 parallel researchers pattern)

## 5. GSD v1 vs v2: Pi SDK explained

GSD v1 = ~50 markdown files as slash commands. Works through prompt engineering. Asks Claude to dispatch subagents.
GSD v2 = standalone TypeScript CLI on Pi SDK. Programmatic control over agent sessions.

Key v2 capabilities v1 lacks:
- Programmatic context clear between tasks (not asking Claude, actually doing it)
- Crash recovery (state persisted to disk, resume from last task)
- Sliding-window stuck detection (TypeScript pattern matching, not prompt-based)
- Direct token/cost tracking per turn
- Auto-advance loop (TypeScript: complete > commit > next > repeat)

dwarves-kit stays on v1 architecture because verification pipeline solves quality differently: detect issues post-task vs. prevent degradation via context control. Simpler, no runtime dependency.