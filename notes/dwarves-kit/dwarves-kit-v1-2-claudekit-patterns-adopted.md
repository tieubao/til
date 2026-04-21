---
title: "dwarves-kit v1.2 ClaudeKit patterns adopted"
date: 2026-03-29
captured: 2026-03-29T16:16:51.283Z
tags: ["dwarves-kit", "claudekit", "session-state", "ship-pipeline"]
source: "Claude.ai session adopting ClaudeKit patterns (March 2026)"
aliases: []
status: refined
---
# dwarves-kit v1.2: ClaudeKit patterns adopted

Two patterns extracted from ClaudeKit deep-dive and implemented.

## 1. Session state save hook

New hook: `hooks/session-state-save.sh`
Events: Stop + SubagentStop (wired in settings.json)
Writes to: `.claude/session-state/last-state.md`

State includes: branch, uncommitted count, spec status, task progress, last 5 commits, modified files, resume instructions. Archives rotate (keeps 10). Fail-open.

Gap it fills: pre-compact-backup only fires on compaction. This fires on every Stop and SubagentStop, catching session crashes and normal endings.

Source: ClaudeKit session-state.cjs (adapted to bash, no Node.js).

## 2. Ship pipeline upgrade

Updated: `commands/ship.md`
New steps: review gate (checks REVIEW.md verdict), version bump (auto-detect project type), changelog generation (Keep a Changelog format).

Full pipeline: review gate > test > uncommitted check > version bump > changelog > conventional commit > docs update > PR with linked issues.

Source: ClaudeKit /ck:ship pipeline. Adapted: review gate checks existing REVIEW.md instead of running inline review.

## File count

50 total (was 49). 1 new hook file, 1 modified command, 1 modified settings.json, 1 modified CLAUDE.md.

## Related

- [[claudekit-deep-dive-session-recovery-red-team-and-gaps]] - the deep-dive session that identified these patterns for extraction
- [[claudekit-evaluation-and-unique-features]] - the initial evaluation that scored ClaudeKit as BOOKMARK
- [[compaction-defense-patterns-for-claude-code-sessions]] - the pre-compact defense that the session-state-save hook extends
- [[building-dwarves-kit-from-extracted-patterns]] - the overall extraction methodology this follows