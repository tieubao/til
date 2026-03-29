---
title: "Compaction defense patterns for Claude Code sessions"
date: 2026-03-29
captured: 2026-03-29T07:20:39.501Z
tags: ["claude-code", "compaction", "context-management", "hooks"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
---
Compaction is the biggest quality killer in long Claude Code sessions. When context fills up, Claude auto-compacts (summarizes) and loses precision. CLAUDE.md rules get diluted. Specific decisions get flattened. The solution is a two-layer defense.

## Layer 1: Pre-compaction backup (PreCompact hook)

Save a structured snapshot before compaction fires. Not a raw dump -- organized markdown with:

- Git state (branch, last 5 commits, uncommitted changes)
- Active spec (task breakdown section from .planning/SPEC.md)
- Recently modified files (last 10 source files by modification time)
- Context essentials (key rules that must survive compaction)

Saved to `.claude/backups/{N}-backup-{timestamp}.md`. Numbered for history.

**Source**: claudefa.st context-recovery-hook pattern. Their implementation uses token-based triggers via StatusLine (first backup at 50k tokens, then every 10k). The simpler approach: just backup on PreCompact event.

## Layer 2: Post-compaction re-injection (PostToolUse hook, matcher: compact)

After compaction fires, re-inject 10-50 lines of critical rules as `additionalContext`. This is NOT the full CLAUDE.md (which gets summarized during compaction anyway). It's the surgical "before you ship" checklist:

- Project identity (from CLAUDE.md ## Project section)
- Current branch and spec location
- Latest backup file path
- Hard rules: don't push to main, read spec before implementing, write tests, no phantom features

**Source**: Nick Porter's PostToolUse(compact) pattern. Key insight: CLAUDE.md = employee handbook (comprehensive, loaded at start). Context essentials = "before you ship" checklist (surgical, re-injected after compaction).

## Alternative: Notepad pattern (oh-my-claudecode)

OMC uses `.omc/notepad.md` which persists across compaction. Simpler than two hooks. Worth evaluating as a replacement if the two-hook approach is too heavy.

## The honest assessment

Pre-compact backup is insurance, not prevention. The best strategy remains: keep sessions short, one task per conversation, prefer /clear over /compact. The backup catches the cases where sessions run long despite best intentions (contractor coding for 4 hours on a complex feature).