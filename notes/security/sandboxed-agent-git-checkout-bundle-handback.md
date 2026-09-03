---
title: "A git checkout mounted into an agent sandbox makes .git attacker-controlled; hand commits back as a bundle"
date: 2026-09-03
captured: 2026-09-03T14:10:00+07:00
tags: ["security", "sandboxing", "git", "ai-agents", "threat-model"]
source: "Claude Code session, designing a VM boundary for a headless coding agent, 2026-09-03"
aliases: ["bundle hand-back", "untrusted .git", "sandboxed agent git checkout"]
status: refined
---

**If a coding agent runs inside a sandbox with a git checkout bind-mounted read-write, the checkout's `.git` directory belongs to the agent, and any git command the host later runs against that directory executes agent-chosen configuration on the host.** The sandbox contains the agent's process; it does not contain the host's own `git diff`, `git log`, `git push` or `gh pr create` when those run on the mounted directory afterwards. The fix is structural: host git never opens the sandbox's repository. The sandbox hands its commits back as a bundle file, and the host fetches that file into a clone the agent never touched.

## Why a mounted checkout is a host-side code-execution path

Git reads configuration and hook-like settings from the repository it operates on. A session that can write anywhere under the checkout can write `.git/config` and `.gitattributes`, so every one of these fires on the host with the host user's privileges the next time the host runs git there:

| Agent writes | Host command that executes it |
|---|---|
| `[core] sshCommand = /bin/sh -c '<payload>'` | `git push` over an ssh remote |
| `[diff] external = <payload>` or `[diff "x"] textconv = <payload>` plus a `.gitattributes` line | `git diff` in the post-session scan |
| `[filter "x"] clean = <payload>` plus `.gitattributes` | any checkout or diff that touches a matching path |
| `[credential] helper = !<payload>` | `git push` over an https remote |
| `[remote "origin"] url = <attacker repo>` | `gh pr create` without an explicit `-R`, which resolves the target from the checkout's config and opens the PR, carrying the host's token, against a repository the agent chose |
| `.git` replaced by a one-line gitfile `gitdir: ../../<real repo>/.git` | every `git -C <dir>` on the host now operates on a different repository than the one the guard fingerprinted |

Overriding one key on the command line (`-c core.hooksPath=...`) closes one row. A denylist over git's configuration space always has a next row.

## The hand-back shape

```
sandbox (agent uid)                         host (trusted)
──────────────────                          ──────────────
/work  <- clone with NO remote              host-repo <- second clone, never mounted
agent edits, commits on branch B
epilogue after the agent exits:
  git bundle create /out/B.bundle base..B
                                            git -C host-repo \
                                              -c transfer.fsckObjects=true \
                                              -c fetch.fsckObjects=true \
                                              fetch /out/B.bundle refs/heads/B:refs/heads/B
                                            git merge-base --is-ancestor <base> B   (else reject)
                                            scans, push by explicit URL, gh pr create -R owner/repo
                                            rm -rf /work   (no git call, symlinks not followed)
```

Rules that make it hold:

1. **Two clones.** The sandbox gets a self-contained clone with its remote removed, so nothing inside can push even if a push permission ever reappears. The host keeps its own clone, made from the same base commit, that the sandbox never sees.
2. **A bundle is data, not a repository.** The host parses it with `transfer.fsckObjects` on, checks the tip descends from the recorded base commit, and only then treats the branch as real. A missing or malformed bundle is a failure reason, never a reason to run git inside the mount.
3. **The epilogue runs inside the sandbox.** `git bundle create` runs as the agent uid after the agent's session ends. Whatever the agent did to `/work/.git` affects that one call, inside the boundary.
4. **PR target from a value read before the session.** Parse `owner/repo` from the origin URL at clone time and pass it with `-R`; never let `gh` resolve it from any checkout's config.
5. **Diffs with `--no-ext-diff --no-textconv`** on the host clone, as belt: the host clone's config is trusted, but the committed `.gitattributes` still names drivers, and the flags make sure none resolve.
6. **Delete the mounted directory without git.** `rm -rf` follows no symlink and reads no gitfile.

## Why a worktree does not help

A `git worktree` keeps its gitdir under the parent repository's `.git/worktrees/<name>`, outside the mounted directory. That is why the pre-sandbox design was safe from this class: the agent could not reach `.git` at all. Moving the agent into a sandbox forces a self-contained clone (the worktree's gitdir is not inside the mount), which is exactly what puts `.git` in the agent's hands. The bundle hand-back restores the property the worktree had, with the sandbox added.

## The scan gaps that come with the same move

Once commits arrive by bundle, the host's secret scan must cover every channel the push carries, not just added diff lines: commit messages, author and committer identities (`%an %ae %cn %ce`), changed path names, and binary blobs (refuse them rather than scan them as text). If the sandbox holds one credential (an API token for the model), scan for its exact value and its obvious encodings too, and state plainly that this raises cost rather than containing the token; the containment fix is a proxy that keeps the token out of the sandbox altogether.

## How to spot this class

Any design that says "the sandbox writes here, then the host runs `<tool>` on it" has this shape when `<tool>` reads configuration from the directory it operates on. Git is the obvious case; package managers reading a lockfile plus a config, test runners reading `conftest.py` or `package.json` scripts, and editors reading project settings are the same class. The question to ask of every host-side step is: which files under the agent-writable path does this command read as instructions, not data?
