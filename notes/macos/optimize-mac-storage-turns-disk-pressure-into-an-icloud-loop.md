---
title: "Optimize Mac Storage turns disk pressure into an iCloud loop"
date: 2026-09-17
captured: 2026-09-17T00:00:00Z
tags: [macos, icloud, storage, debugging]
source: "Claude Code session"
aliases: ["icloud keeps syncing the same files", "mac downloads files over and over", "evicted icloud files backup loop"]
status: refined
---

macOS "Optimize Mac Storage" evicts iCloud Drive files when the disk gets tight, leaving dataless stubs behind. Any periodic job that walks those paths and forces the bytes back, a backup, a two way mirror, an indexer, pulls them down. Then the disk is tight again, macOS evicts them again, and the next run redownloads the same bytes.

The symptom presents as a sync problem: constant iCloud activity, steady network use, a spinning progress indicator that never finishes. It is a disk problem wearing a sync problem's clothes, so time goes into sync settings and accounts while the actual cause sits in the free-space number.

Order of checks when iCloud will not settle:

1. Free space on the volume. If it is near full, stop here; this is the cause.
2. Whether a scheduled job walks the iCloud tree and materialises files (`brctl download`, restic on `~/Documents`, a mirror tool).
3. Only then, sync settings.

The fix is either half of the loop: free enough disk that eviction stops, or pin the folder with Finder's "Keep Downloaded" so macOS stops evicting that tree regardless of pressure. Pointing a backup at an evicted tree is its own trap, since `du` counts allocated blocks and reports an evicted tree as nearly empty.

## Key takeaway

An automated reader and an automatic evictor pointed at the same tree form a loop. Look at free space before you look at sync.
