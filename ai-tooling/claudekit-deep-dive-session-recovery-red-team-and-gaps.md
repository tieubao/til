---
title: "ClaudeKit deep-dive session recovery red-team and gaps"
date: 2026-03-29
captured: 2026-03-29T16:10:22.860Z
tags: ["claudekit", "session-management", "red-team", "landscape"]
source: "Claude.ai session on ClaudeKit deep-dive (March 2026)"
---
# ClaudeKit deep-dive: session recovery, red-team, and landscape gaps (March 2026)

## Session state management

ClaudeKit beta has `session-state.cjs` -- a hook that auto-persists and restores session state. Saves on Stop + SubagentStop events (not just PreCompact). State includes: active plan, todo items, subagent outputs, branch/commit status. Archives after 10 sessions.

Our gap: dwarves-kit only saves state on PreCompact. If a session crashes without compaction, state is lost. Adding a Stop-hook state save would catch both cases.

Also notable: `/resume` by session name (not just UUID), `/export` for manual context handoff to a fresh session, and CCS multi-provider model switching (start with Claude Opus for planning, switch to GLM for implementation at 81% lower cost).

## Red team review

ClaudeKit's `/ck:plan red-team` spawns 4 adversarial reviewers (security, failure mode, assumption destroyer, scope critic). This is exactly our `/spec-validate`. Feature parity. Nothing to extract.

## Ship pipeline

ClaudeKit's `/ck:ship` does: merge > test > adversarial review > version bump > changelog > push > PR with linked issues. Our `/ship` does: test > commit > PR. The gap is: no version bumping, no changelog generation, no enforced review-before-ship gate.

## Other features evaluated

- `/ck:deploy` (15+ platforms): out of scope per philosophy
- `/ck:llms` (generate llms.txt): interesting, bookmark
- `/ck:security-scan` (OWASP): we have security-auditor agent, comparable
- CCS (multi-provider switching): product feature, not workflow pattern, skip
- 66 stable + 71 beta skills: most are domain-specific (Shopify, Stripe, SEO), not comparable to a workflow kit

## Patterns to extract

1. **Stop-hook state save**: save session state on Stop + SubagentStop, not just PreCompact
2. **Ship pipeline upgrade**: add review gate + version bump + changelog to /ship

## Updated landscape position

ClaudeKit remains BOOKMARK (10/15). The Vietnamese docs and breadth make it relevant for Dwarves team adoption later. The session state pattern is the most valuable extraction target. Their red-team review is identical to ours. Their ship pipeline is slightly more complete.