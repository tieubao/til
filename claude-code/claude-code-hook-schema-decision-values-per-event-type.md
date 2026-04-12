---
title: "Claude Code hook schema decision values per event type"
date: 2026-03-28
captured: 2026-03-28T05:38:13.039Z
tags: ["claude-code", "hooks", "debugging"]
source: "Claude Code session - github-mcp-worker project, fixing Stop hook validation error"
aliases: []
status: refined
---
## Context

While building a knowledge capture pipeline for the `github-mcp-worker` project, a Stop hook was configured in Claude Code's `settings.json` to remind Claude to check for learning moments at the end of each response. The hook started throwing `JSON validation failed: Hook JSON output validation failed` errors on every prompt submission, surfacing a noisy error banner that obscured the actual conversation.

## The Problem

The Stop hook was outputting `"decision": "allow"` in its JSON response:

```json
{"decision": "allow", "reason": "LEARNING CAPTURE CHECK: ..."}
```

Claude Code rejected this with `Invalid input`. The error message showed the expected schema, but the valid values weren't immediately obvious because the schema lists multiple hook event types with different field names and values.

## What I Found

Claude Code hooks have **different valid `decision` values depending on the hook event type**. The field names and accepted values are not interchangeable:

| Hook Event | Decision Field | Valid Values |
|---|---|---|
| **Stop** | `decision` | `"approve"` or `"block"` |
| **PreToolUse** | `permissionDecision` | `"allow"`, `"deny"`, or `"ask"` |
| **PostToolUse** | n/a | No decision field; uses `additionalContext` |
| **UserPromptSubmit** | n/a | No decision field; uses `additionalContext` |

The confusion comes from the fact that `"allow"` is a valid value in the system, just not for Stop hooks. It belongs to `PreToolUse`'s `permissionDecision` field. The fix was a one-character change:

```json
// Before (wrong -- PreToolUse value in a Stop hook)
{"decision": "allow", "reason": "..."}

// After (correct -- Stop hook value)
{"decision": "approve", "reason": "..."}
```

The `reason` field in a Stop hook surfaces as context in the conversation, making it useful for injecting behavioral reminders (like "check for learning moments"). But the `decision` field must still use the correct enum or the hook errors out visibly every time.

## How to spot this

Any time a Claude Code hook throws `Hook JSON output validation failed` with `Invalid input`, check whether the `decision` or `permissionDecision` field value matches the hook event type. The most common mistake is mixing up `"allow"` (PreToolUse) with `"approve"` (Stop).

## Related

- [[claude-code-hook-lifecycle-and-event-system]] - the full event system and exit code contract these decision values plug into
- [[commands-vs-hooks-vs-skills-decision-framework]] - choosing when a hook is the right mechanism
- [[why-knowledge-notes-need-context-not-just-facts]] - this debugging discovery was one of the notes that prompted the "context is mandatory" rule