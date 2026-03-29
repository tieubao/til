---
title: "Commands vs hooks vs skills decision framework"
date: 2026-03-29
captured: 2026-03-29T07:19:48.536Z
tags: ["claude-code", "hooks", "commands", "skills", "architecture"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
---
## The mapping problem

Claude Code hooks provide enforcement. Commands provide workflow. Skills provide autonomy. Most kits use one mechanism. The best kits use all three for different purposes.

## Decision framework

| Question | Mechanism | Why |
|----------|-----------|-----|
| Should this happen every time, no exceptions? | Hook | 100% enforcement via exit code 2 |
| Does this need human judgment on when to run? | Command | User invokes at the right moment |
| Should Claude decide when to apply this? | Skill | Claude reads SKILL.md and activates contextually |

## Concrete examples from dwarves-kit

| Feature | Mechanism | Rationale |
|---------|-----------|-----------|
| Block rm -rf | Hook (PreToolUse) | Never optional, must be deterministic |
| Generate spec from intent | Command (/spec) | User decides when to plan |
| Fetch API docs before coding | Skill (get-api-docs) | Claude should check docs autonomously |
| Challenge an idea | Command (/think) | User decides when to question assumptions |
| Auto-format on file write | Hook (PostToolUse) | Should happen every time, no exceptions |
| Check context readiness | Hook (SessionStart) | Should inject context automatically every session |
| Warn on spec drift | Hook (PreToolUse) | Soft enforcement (allow + context warning, not block) |
| Anti-rationalization | Hook (Stop) | Must catch cop-outs deterministically |
| Code review | Command (/review) | User decides when to review |

## The gradient

Hooks are the strictest (deterministic, blocks actions). Commands are medium (user-triggered, Claude follows the prompt). Skills are the loosest (Claude decides if and when to apply). Place each feature at the right level of strictness. Don't over-hook (creates friction) or under-hook (creates risk).

## The trap to avoid

Putting safety rules in CLAUDE.md instead of hooks. "Don't push to main" in CLAUDE.md is followed ~70% of the time. The same rule as a PreToolUse hook with exit code 2 is followed 100%. If a rule matters enough to write down, ask: "can I live with 30% non-compliance?" If no, it needs a hook.