---
title: "git reset --keep drops a duplicate commit when --hard is forbidden"
date: 2026-09-05
captured: 2026-09-05T09:40:00Z
tags: [git, devtools, workflow]
source: "Claude Code session"
aliases: [git reset --keep, duplicate commit after merge, reset hard blocked by hook, local branch ahead with identical commit]
status: refined
---

**When a local branch carries a commit whose content is already on the remote (the same diff applied twice, for example once from each of two worktrees), `git reset --keep origin/main` discards the redundant commit without touching uncommitted work, and it passes a safety hook that blocks `git reset --hard`.**

## The situation

After a merge, `git log origin/main..HEAD` still showed one local commit. `git show <local>` and `git show <origin>` were byte-identical: same message, same diff. Pushing would have created a pointless merge of a duplicate; `git reset --hard origin/main` was blocked by a repo hook that refuses `--hard` whenever untracked or modified files exist.

## Why `--keep` is the right verb

| Mode | Index | Working tree | Local modifications |
|---|---|---|---|
| `--soft` | untouched | untouched | kept, still staged |
| `--mixed` (default) | reset | untouched | kept, unstaged |
| `--keep` | reset | reset for files the commits changed | kept; aborts if a modified file would be overwritten |
| `--hard` | reset | reset | destroyed |

`--keep` moves the branch pointer to the target, updates only the paths that differ between the commits, and refuses to proceed if any of those paths has local changes. That is the guarantee a "no `--hard`" hook exists to enforce, so the hook can allow it.

```bash
git fetch origin
git diff origin/main HEAD --stat        # expect: nothing, the content is already upstream
git reset --keep origin/main
git log --oneline -1                     # now equal to origin/main
```

If `--keep` aborts with "Entry ... not uptodate", a modified file overlaps the commits being dropped; stash or commit it first rather than reaching for `--hard`.
