---
title: "An Apple Containers virtiofs mount pins one host file descriptor per inode the guest has touched"
date: 2026-09-05
captured: 2026-09-05T08:30:00Z
tags: [macos, apple-containers, virtiofs, kernel-panic, ci]
source: "Claude Code session"
aliases: [too many open files in system, kern.maxfiles exhausted, initproc exited kernel panic, ENFILE launchd]
status: refined
---

**A host directory bind-mounted into an Apple Containers VM costs the host one open file descriptor for every inode the guest has looked up, and the guest's dentry cache keeps those descriptors pinned after the work that touched them is done.** Mount a package-manager cache with a hundred thousand files, run one install inside the VM, and the host is now holding a hundred thousand FDs it will not release until the VM stops.

The failure this produces is not per-process. `kern.maxfilesperproc` never trips, because the descriptors belong to the VM process, and that process is allowed to hold them. What fills is the system-wide table, `kern.maxfiles`. When it is full, every process on the host gets `ENFILE` ("Too many open files in system") on its next `open()`, including `launchd`. launchd cannot open a file, exits, and the kernel panics on `initproc exited`. The whole machine reboots because a CI cache was mounted from the wrong side.

## How it looked from the outside

Every daemon on the box logged the same error within a minute of each other, which is the tell for a full system table rather than a leaking process:

```
fork/exec /usr/sbin/netstat: too many open files in system
OSError: [Errno 23] Too many open files in system
```

Then the panic report: `initproc exited -- exit reason namespace 2 subcode 0xa`.

## How to check

```
sysctl kern.num_files kern.maxfiles kern.maxfilesperproc
for p in $(pgrep -f com.apple.Virtualization.VirtualMachine); do
  echo "$p $(lsof -n -p "$p" 2>/dev/null | wc -l)"
done
```

`kern.num_files` climbing toward `kern.maxfiles` with the count concentrated in one or two VM processes is this bug. A freshly rebooted host with two runner VMs and a shared cache mount was back at 180,000 of 491,520 within twenty minutes.

## What to do instead

- Keep large file trees on the VM's own disk (a state volume the VM owns), never bind-mounted from the host. Mount host directories only for small, read-mostly trees.
- Raise `kern.maxfiles` as a buffer, not as a fix; a bigger table only delays the same panic.
- Alert on `kern.num_files / kern.maxfiles` (warn at 60 percent, critical at 80) rather than on any per-process count, because the per-process limits never fire here.
