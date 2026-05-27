---
title: "Managing Claude Code's agent view (background sessions)"
date: 2026-05-24
captured: 2026-05-24T04:59:18.861Z
tags: ["claude-code", "workflow", "agents"]
source: "Claude Code session"
---
Agent view (`claude agents`) is the TUI that lists every background session, running and completed. As you dispatch more work, the list grows with finished jobs. The instinct is to treat that pile as mess to clean. It mostly is not: completed sessions auto-purge after ~30 days, and the TUI folds old ones into a `... N more` row to stay scannable. The real work is triage, not deletion.

(Behavior below verified against Claude Code v2.1.x. Surfaces: terminal TUI is primary; the desktop app, web at claude.ai/code, and the VS Code extension expose the same session list.)

## The mental model

A session moves through four stages, persists on disk the whole time, and auto-expires. The one thing that is not reversible is deleting a session that still holds uncommitted changes in its worktree.

![Agent view session lifecycle: dispatch to running to in-list to exit, with on-disk persistence, the worktree-delete gotcha, and 30-day retention](https://assets.han-ws.workers.dev/i/2026/05/agent-view-session-lifecycle.svg)

On disk:

- `~/.claude/jobs/<id>/state.json` and `timeline.jsonl` (the transcript) for every session
- `<repo>/.claude/worktrees/<id>/` only if the session edited files
- `~/.claude/daemon/roster.json` tracks currently-running sessions

## Day-to-day routine

| Step | Action | Control |
|---|---|---|
| Open | List everything | `claude agents` |
| Group | Sort by state so "Needs input" floats up, done sinks | `Ctrl+S` toggles state vs directory |
| Check done | Peek at latest output without opening the full transcript | `Space` |
| Reply fast | Type into a peeked session and send, no attach needed | type + `Enter` |
| Attach | Only when you need full history | `Enter` or `→` |
| Pin | Keep long-runners (servers, monitors) alive through idle timeout | `Ctrl+T` |
| Name | So you can find them later | `Ctrl+R`, or `claude --bg --name "x"` at dispatch |
| Delete | Stop, then press again within 2s to remove | `Ctrl+X` `Ctrl+X` |

## Where each control lives

The same session list is driven from three surfaces. Knowing which lives where saves a lot of context-switching.

![Agent view control map: shell commands, TUI keyboard shortcuts, and in-session slash commands side by side](https://assets.han-ws.workers.dev/i/2026/05/agent-view-control-map.svg)

Shell equivalents are handy for scripting: `claude agents --json`, `claude logs <id>`, `claude stop <id>`, `claude rm <id>`.

## The worktree gotcha

Every background session that edits files first moves into an isolated git worktree under `.claude/worktrees/<id>/`, so parallel sessions do not collide.

Deleting that session with `Ctrl+X` twice **removes the worktree, including uncommitted changes**. Push or merge before you delete. `claude rm <id>` is the safer path: it preserves a worktree that has uncommitted changes and prints the path. A worktree you created yourself with `git worktree add` is left alone either way.

Orphaned worktrees are the one thing that genuinely lingers. Sweep them:

```bash
git worktree list
git worktree remove <path>
```

## Retention and cleanup

Transcripts auto-delete after `cleanupPeriodDays` (default 30) at startup. `claude --resume <id>` still works until that purge, even after you remove the session from the list. Deleting from the TUI is cosmetic; the setting is what controls disk turnover. To purge weekly instead:

```json
// ~/.claude/settings.json
{ "cleanupPeriodDays": 7 }
```

## Bottom line

Do not micromanage the list. The design assumes many sessions. Spend effort on two habits: name jobs at dispatch so the list is greppable, and push work out of worktrees before deleting. Everything else auto-handles. The only time to actively clean is when `.claude/worktrees/` grows large, since orphaned worktrees are what actually accumulate.

## Related

- [[claude-code-surfaces-cli-vs-web-vs-desktop-and-resource-usage]] - the surfaces this session list shows up on
- [[claude-code-hook-lifecycle-and-event-system]] - how hooks fire across the session stages this note walks through
- [[compaction-defense-patterns-for-claude-code-sessions]] - companion long-session-management technique
- [[commands-vs-hooks-vs-skills-decision-framework]] - choosing the right primitive when configuring a background-session workflow
- [[claude-integration-with-jupyter-notebooks]] - Jupyter-side workflow that pairs with managing Claude Code background sessions