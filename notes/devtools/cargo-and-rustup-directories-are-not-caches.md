---
title: "cargo and rustup directories are not caches"
date: 2026-09-17
captured: 2026-09-17T00:00:00Z
tags: [rust, devtools, disk, cleanup]
source: "Claude Code session"
aliases: ["is it safe to delete ~/.cargo", "rustup folder huge disk", "disk cleanup deleted my rust toolchain"]
status: refined
---

A disk cleanup that ranks directories by size and judges each one by its reputation will delete working installs. Two of the usual top hits are not caches at all:

| Directory | What is actually in it |
|---|---|
| `~/.cargo` | `registry/` and `git/` are caches, but `bin/` holds every binary installed with `cargo install`. Deleting the directory uninstalls those tools. |
| `~/.rustup` | The toolchains themselves. The `rustc` and `cargo` on PATH are shims that resolve into here. Deleting it leaves the shims pointing at nothing. |

Both are large, both sit in a dotfolder in `$HOME`, and both read as build artefacts to a size-ranked sweep. Neither regenerates on its own: recovery is a `rustup` reinstall plus remembering every `cargo install` ever run.

The general check, before trashing anything whose name suggests a cache:

```bash
ls <dir>/bin 2>/dev/null          # does it ship executables?
command -v <some-tool>            # does a tool on PATH resolve inside it?
```

If either answers yes, it is an installation with a cache inside it, not a cache. Clean the cache subdirectory (`cargo cache -a`, or `~/.cargo/registry` by hand) and leave the parent alone.

## Key takeaway

Size plus a familiar-looking name is not evidence that a directory is disposable. Look for a `bin/` and for PATH resolution before deleting.

## Related

- [[xdg-base-directory-specification]] - the spec that defines which dotfolder subtree is actually meant to be a cache
- [[optimize-mac-storage-turns-disk-pressure-into-an-icloud-loop]] - the other half of a disk cleanup pass: free-space pressure eviction versus a size-ranked sweep that deletes installs
