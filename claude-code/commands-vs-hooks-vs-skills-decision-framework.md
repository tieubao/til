---
title: "Commands vs hooks vs skills decision framework"
date: 2026-03-29
captured: 2026-03-29T08:03:40.625Z
tags: ["claude-code", "hooks", "commands", "skills", "architecture"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
---
Claude Code has three extension mechanisms. The choice between them determines whether a behavior is enforced, suggested, or autonomous. Getting this wrong is the #1 kit design mistake.

## The decision question

Ask: "What happens if this doesn't run?"

| Answer | Mechanism | Why |
|--------|-----------|-----|
| Something irreversible happens (data loss, security breach, pushed to main) | **Hook** (exit 2) | Must be deterministic. 100% enforcement. |
| The output is worse but nothing breaks | **Skill** (Claude-triggered) | Claude applies it when relevant. No human action needed. |
| Nothing happens until the human decides it's time | **Command** (human-triggered) | User invokes at the right moment in their workflow. |

## The strictness gradient

![Strictness gradient from Hooks (deterministic, 100% compliance) through Commands (user-triggered) to Skills (Claude-triggered)](https://assets.han-ws.workers.dev/i/2026/03/strictness-gradient.png)

## Concrete mappings from dwarves-kit

| Feature | Mechanism | Why this level of strictness |
|---------|-----------|---------------------------|
| Block rm -rf | Hook (PreToolUse, exit 2) | Irreversible. Must block every time. |
| Block push to main | Hook (PreToolUse, exit 2) | Irreversible. Convention compliance isn't optional. |
| Catch premature "done" | Hook (Stop, exit 2) | Claude rationalizing costs hours of wasted review cycles. |
| Auto-format on write | Hook (PostToolUse, exit 0) | Should happen every time, but non-blocking (formatter failure shouldn't stop work). |
| Warn on unplanned files | Hook (PreToolUse, allow + context) | Soft enforcement. Warn but don't block. |
| Inject project context | Hook (SessionStart) | Must happen automatically at session start. |
| Fetch API docs before coding | Skill (get-api-docs) | Claude should check docs autonomously when it detects API work. |
| Generate spec from intent | Command (/spec) | User decides when to plan. |
| Paranoid code review | Command (/review) | User decides when to review. |

## The trap

Putting safety rules in CLAUDE.md instead of hooks. "Don't push to main" in CLAUDE.md is followed ~70% of the time. As a PreToolUse hook with exit 2, it's followed 100%. The 30% gap is where production incidents live.

The reverse trap is also real: over-hooking. If you hook everything, every tool call is gated and sessions feel sluggish. Hook only what's irreversible or has high cost-of-failure. Everything else is commands (human judgment) or skills (Claude judgment).

## When this framework breaks down

When you need conditional strictness. Example: "block push to main on production repos, allow it on personal projects." The hook doesn't know which type of repo it's in. Solutions: path-scoped rules (.claude/rules/ with YAML frontmatter), environment variables checked inside the hook, or project-level settings.json overriding user-level settings.json.