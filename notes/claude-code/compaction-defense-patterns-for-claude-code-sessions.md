---
title: "Compaction defense patterns for Claude Code sessions"
date: 2026-03-29
captured: 2026-03-29T08:04:05.867Z
tags: ["claude-code", "compaction", "context-management", "hooks"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
aliases: []
status: refined
---
## When to use this

You're in a long Claude Code session (2+ hours). Context fills up. Auto-compaction fires. Claude suddenly doesn't remember the specific error message you debugged, the architectural decisions you made, or the exact function signatures you discussed. The summary captures the gist but the precision is gone.

## The two-layer defense

### Layer 1: Pre-compaction backup (PreCompact hook)

Fires right before compaction. Saves a structured markdown snapshot to `.claude/backups/`.

What to capture (not a raw dump -- organized for recovery):
- Git state: branch, last 5 commits, uncommitted changes
- Active spec: task breakdown section from `.planning/SPEC.md`
- Recently modified files (last 10 source files by modification time)
- Context essentials: key rules that must survive compaction

What NOT to capture: full conversation transcript (too large), file contents (already in git), tool results (noisy).

### Layer 2: Post-compaction re-injection (PostToolUse hook, matcher: compact)

After compaction fires, re-inject 10-50 lines of critical rules as `additionalContext`. This is NOT the full CLAUDE.md.

The mental model: CLAUDE.md is the employee handbook (comprehensive, loaded at session start, gets summarized during compaction). Context essentials is the "before you ship" checklist (surgical, re-injected after compaction to restore precision).

What to re-inject:
- Project identity (from CLAUDE.md ## Project)
- Current branch and spec location
- Latest backup file path (so Claude can read it)
- Hard rules: don't push to main, read spec before implementing, write tests, no phantom features

### Decision tree

```
Session running long?
├── Under 50k tokens: no action needed
├── Approaching compaction:
│   ├── Can you /clear and start fresh? → Do that (best option)
│   └── Can't /clear (mid-task)? → Pre-compact backup saves your state
└── Compaction already fired:
    ├── Post-compact hook re-injects critical rules
    └── Claude reads latest backup file for recovery
```

## Alternative: Notepad pattern (from oh-my-claudecode)

OMC uses `.omc/notepad.md` which persists key facts across compaction in a single file. Simpler than two hooks. Tradeoff: less structured recovery (it's a scratchpad, not an organized snapshot) but easier to implement and maintain.

## When none of this helps

Pre-compact backup is insurance, not prevention. The best strategy remains:
- Keep sessions short. One task per conversation.
- Prefer `/clear` over `/compact`. Starting fresh costs 20k tokens (nothing) vs. quality loss from compacted context.
- Write decisions to files (not just conversation). Claude can always re-read a file. It can't recover compacted conversation history.

## Related

- [[claude-code-hook-lifecycle-and-event-system]] - the PreCompact and PostToolUse events this pattern depends on
- [[dwarves-kit-v1-2-claudekit-patterns-adopted]] - the session-state-save hook that extends this pattern to Stop events
- [[claudekit-deep-dive-session-recovery-red-team-and-gaps]] - ClaudeKit's session-state.cjs that inspired the Stop-hook extension
- [[multi-agent-coding-brain-rot-scan-design-externalized-state-clean-handoffs]] - externalized state as a general pattern for maintaining coherence