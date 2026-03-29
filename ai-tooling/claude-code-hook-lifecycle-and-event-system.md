---
title: "Claude Code hook lifecycle and event system"
date: 2026-03-29
captured: 2026-03-29T07:18:53.706Z
tags: ["claude-code", "hooks", "lifecycle", "agent-engineering"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
---
## Overview

Claude Code has 21 lifecycle hook events across 4 handler types. Hooks are deterministic automation points -- unlike CLAUDE.md rules (followed ~70% of the time), hooks with exit code 2 are enforced 100% of the time.

## Hook events by category

**Session layer:**
- `Setup` -- fires on `--init` or `--maintenance` CLI flags
- `SessionStart` -- startup, resume, clear, compact. stdout becomes Claude's context.
- `SessionEnd` -- exit, sigint, error

**Prompt layer:**
- `UserPromptSubmit` -- before Claude processes user input. stdout injected as context.

**Tool layer:**
- `PreToolUse` -- before tool runs. Can allow, deny, or ask via `hookSpecificOutput.permissionDecision`. Matchers: Bash, Edit, Write, Read, Glob, Grep, Agent, WebFetch, WebSearch, MCP tools.
- `PermissionRequest` -- when permission dialog would show. Can auto-approve or deny.
- `PostToolUse` -- after successful tool execution. Matcher on tool name.
- `PostToolUseFailure` -- after tool error.

**Agent layer:**
- `SubagentStart`, `SubagentStop` -- subagent lifecycle
- `Stop` -- when agent finishes response. Exit 2 forces continuation.

**Maintenance:**
- `PreCompact` -- before conversation compaction
- `Notification` -- async, non-blocking system notifications

**Newer events:**
- `Elicitation`, `ElicitationResult` (v2.1.76+) -- intercept MCP server elicitation
- `TeammateIdle`, `TaskCompleted`, `ConfigChange`

## 4 handler types

| Type | Use case | Latency |
|------|----------|---------|
| command | Shell scripts, fastest, most debuggable | <100ms typical |
| http | POST to external endpoints | Network-dependent |
| prompt | Single-turn LLM evaluation (e.g., Haiku) | 2-5s |
| agent | Spawns subagent with tool access | 10-30s |

## Exit code semantics

- Exit 0: success, proceed. stdout visible in transcript mode. For SessionStart and UserPromptSubmit, stdout is injected as Claude's context.
- Exit 1: non-blocking error. stderr shown to user. Action still proceeds. Warning only.
- Exit 2: BLOCKS the action. stderr routed to the model. Only real enforcement mechanism.

## JSON output contract

PreToolUse uses `hookSpecificOutput`:
```json
{"hookSpecificOutput": {"hookEventName": "PreToolUse", "permissionDecision": "allow|deny|ask", "permissionDecisionReason": "string"}}
```

Stop/SubagentStop uses top-level `decision`:
```json
{"decision": "block", "reason": "Finish what you started."}
```

SessionStart/UserPromptSubmit uses `additionalContext`:
```json
{"additionalContext": "Branch: main. Spec found. 3 uncommitted files."}
```

## Key design insight

The critical distinction: PreToolUse is the only hook that can block tool execution. Use it for safety gates and mandatory review enforcement. PostToolUse hooks validate after the fact -- they cannot undo actions. Stop hooks can force continuation but cannot undo completed work. Structure accordingly: critical checks in PreToolUse, cleanup in PostToolUse, completion validation in Stop.

## Performance budget

Each hook runs synchronously. Total hook execution time adds to every matched tool call. Threshold: if a hook adds more than 500ms per matched action, sessions feel sluggish. Profile with `time` before deploying. The `stop_hook_active` guard is essential for Stop hooks to prevent infinite loops.

## Config locations (priority order)

1. `~/.claude/settings.json` -- user-level, highest priority
2. `.claude/settings.json` -- project-level, committed to git
3. `.claude/settings.local.json` -- local project, git-ignored
4. Skill/agent YAML frontmatter -- scoped to component lifecycle