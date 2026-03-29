---
title: "Claude Code hook lifecycle and event system"
date: 2026-03-29
captured: 2026-03-29T07:46:58.981Z
tags: ["claude-code", "hooks", "lifecycle", "agent-engineering"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
---
Claude Code exposes 21 lifecycle hook events that fire at specific points during a session. Hooks are the enforcement layer: unlike CLAUDE.md rules (followed ~70% of the time), a hook with exit code 2 is followed 100% of the time.

## The mental model

Think of hooks as interceptors at every decision point in the agent loop. The agent thinks, picks a tool, runs it, gets results, decides to stop or continue. Hooks can intercept before tool execution (gate it), after tool execution (validate it), and at the stop decision (force continuation).

## Events by where they fire

**Session boundaries** -- setup, teardown, context management:

| Event | When | Key behavior |
|-------|------|-------------|
| Setup | `--init` or `--maintenance` CLI flags | One-time project initialization |
| SessionStart | Startup, resume, clear, compact | stdout becomes Claude's context (critical for injection) |
| SessionEnd | Exit, sigint, error | Cleanup, logging |
| PreCompact | Before conversation compaction | Last chance to save state before summarization |

**User input** -- before Claude processes a prompt:

| Event | When | Key behavior |
|-------|------|-------------|
| UserPromptSubmit | User hits enter | Can inject context (stdout) or block the prompt |

**Tool execution** -- the core agent loop:

| Event | When | Key behavior |
|-------|------|-------------|
| PreToolUse | After Claude picks a tool, before running it | Can allow, deny, or modify tool input. The only hook that blocks tool execution. |
| PermissionRequest | When permission dialog would show | Can auto-approve or deny on behalf of user |
| PostToolUse | After tool succeeds | Validate output, run formatters, log results |
| PostToolUseFailure | After tool fails | Error logging, retry logic |

**Agent control** -- subagents and completion:

| Event | When | Key behavior |
|-------|------|-------------|
| SubagentStart / SubagentStop | Subagent lifecycle | Hooks fire recursively for subagent tool calls too |
| Stop | Agent finishes response | Exit 2 forces continuation. Must guard against infinite loops via `stop_hook_active`. |
| Notification | Async alerts | Non-blocking. Good for desktop notifications. |

## The exit code contract

This is the single most important thing to understand:

| Exit code | Meaning | What happens |
|-----------|---------|-------------|
| 0 | Success | Proceed. stdout visible in transcript. For SessionStart/UserPromptSubmit, stdout is injected as context. |
| 1 | Non-blocking error | stderr shown to user. Action STILL proceeds. Warning only. |
| 2 | Block | Action cancelled. stderr routed to the model. This is the only real enforcement. |

Every security hook MUST use exit 2, not exit 1. Exit 1 logs a warning. Exit 2 actually stops the action.

## 4 handler types

| Type | What it is | When to use | Latency |
|------|-----------|-------------|---------|
| command | Shell script, reads JSON from stdin | Fast checks, pattern matching, file operations | <100ms |
| http | POST to an endpoint | Team-wide enforcement via shared server | Network-dependent |
| prompt | Single-turn LLM evaluation | Semantic judgment (is this rationalization?) | 2-5s, costs tokens |
| agent | Spawns subagent with tool access | Deep verification requiring codebase analysis | 10-30s |

Start with command hooks. Graduate to prompt hooks only when grep patterns aren't accurate enough.

## JSON output contracts

Each event type has its own output format. The three you'll use most:

**PreToolUse** (allow/deny/modify):
```json
{"hookSpecificOutput": {"hookEventName": "PreToolUse", "permissionDecision": "allow|deny|ask"}}
```

**Stop** (force continuation):
```json
{"decision": "block", "reason": "Finish what you started."}
```

**SessionStart** (inject context):
```json
{"additionalContext": "Branch: main. Spec found. 3 uncommitted files."}
```

## The performance constraint

Hooks run synchronously. Total hook execution time adds to every matched tool call. If a PostToolUse hook adds 500ms+ to every file edit, sessions feel sluggish. Profile with `time` before deploying. Ten fast hooks outperform two slow ones.