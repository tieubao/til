---
title: "dwarves-kit v1.2 five open decisions"
date: 2026-03-29
captured: 2026-03-29T16:06:09.626Z
tags: ["dwarves-kit", "decisions", "gsd"]
source: "Migrated from ai-tooling/ (March 2026)"
---
# dwarves-kit v1.2: Five open decisions resolved

## 1. Execution engine: STAY NATIVE
Use Claude Code's native Task tool. Don't adopt GSD v2's Pi SDK. Verification pipeline addresses quality through post-task verification. Revisit if crash recovery becomes a real pain after 3+ /execute runs.

## 2. Bootstrap command: COMPLETE
/start detects 8 project states. context-readiness.sh v2 injects suggestions at SessionStart.

## 3. Graph context: ADOPT codebase-memory-mcp
Single static binary, 66 languages, 120x token reduction. Install on any project with 50+ source files.

## 4. Agent layer: COMPLETE for v1.2
8 agents built. Future: parallel reviewers and research agents (Phase 3).

## 5. GSD v1 vs v2
v1 = markdown prompts, asks LLM to dispatch. v2 = TypeScript CLI on Pi SDK, programmatic control. Kit stays on v1 architecture because verification pipeline solves quality differently.