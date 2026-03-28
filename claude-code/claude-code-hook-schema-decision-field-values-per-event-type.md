---
title: "Claude Code Hook Schema - Decision Field Values Per Event Type"
date: 2026-03-28
captured: 2026-03-28T05:14:48.508Z
tags: ["claude-code", "hooks", "debugging", "til"]
source: "Claude Code session - fixing Stop hook validation error in github-mcp-worker project"
---
## The Gotcha

Claude Code hooks have different valid `decision` values depending on the hook event type. Using the wrong value causes a JSON validation error that surfaces to the user.

## Valid Values by Hook Type

| Hook Event | Field | Valid Values |
|---|---|---|
| **Stop** | `decision` | `"approve"` or `"block"` |
| **PreToolUse** | `permissionDecision` | `"allow"`, `"deny"`, or `"ask"` |
| **PostToolUse** | n/a | No decision field, uses `additionalContext` |
| **UserPromptSubmit** | n/a | No decision field, uses `additionalContext` |

## How to Spot

If you see `Hook JSON output validation failed` with `Invalid input`, check whether your `decision` field value matches the hook event type. Common mistake: using `"allow"` (a PreToolUse value) in a Stop hook instead of `"approve"`.

## Example Fix

```json
// WRONG - Stop hook with PreToolUse value
{"decision": "allow", "reason": "..."}

// RIGHT - Stop hook with correct value
{"decision": "approve", "reason": "..."}
```

## Key Insight

The `reason` field in a Stop hook surfaces as context in the conversation, making it useful for injecting reminders (like "check for learning moments"). But the `decision` field must still be valid or the hook errors out visibly.