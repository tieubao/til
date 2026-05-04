---
title: "macOS multi-user cost myth: it's the GUI session that's heavy, not the user"
date: 2026-05-04
captured: 2026-05-04T16:05:00+07:00
tags: ["macos", "launchd", "multi-user", "architecture", "performance"]
source: "agentkernel + Hermes brainstorm 2026-05-04, empirical measurement 2026-05-04"
aliases: []
status: refined
---

**macOS is a multi-user UNIX with one shared kernel. A user is a UID + home dir + permissions context, not a separate OS instance.** The folk-belief that "running 3 macOS users at once costs 3x the OS overhead" is wrong, and it leads people to architect around containers / VMs when plain multi-user POSIX would have been ~3-5x cheaper.

What people actually remember when they say "multi-user is heavy" is the GUI session — WindowServer, Finder, Dock, login items, autostart apps. Those are heavy (~500 MB-1 GB per active GUI session). They are not the cost of having multiple users; they are the cost of having multiple users **logged into the desktop concurrently**.

For the common case of "I want to run several daemons under different UIDs via LaunchDaemon `UserName=foo`, no one ever logs into the desktop as foo" — the cost per user is essentially zero beyond the daemon process itself.

## The empirical evidence

Run these on any healthy macOS install:

```bash
# How many service users coexist on this box?
$ dscl . -list /Users | grep -c '^_'
161

# What does that 161 cost?
$ ps -axo user,rss | awk '$1 ~ /^_/ { sum += $2 } END { printf "%.0f MB\n", sum/1024 }'
935 MB
```

161 system service users (`_mdnsresponder`, `_locationd`, `_windowserver`, `_coreaudiod`, `_atsserver`, `_periodic`, `_softwareupdate`, ...) coexist on a single laptop, sharing one macOS kernel. Total RAM consumed by ALL of them combined: ~935 MB. That's the cost of every system service running on the box, not per-user OS overhead.

Drilling into one user shows what GUI session costs versus what user-existence costs:

```bash
$ launchctl print user/501  | head -2
user/501 = { ... active count = 84, service count = 82 ... }

$ launchctl print gui/501   | head -2
gui/501 = { ... active count = 459, service count = 458 ... }
```

For UID 501 (the interactive user on this laptop), the non-GUI launchd domain manages 82 services. The GUI session manages 458. The 376-service delta is what GUI session adds on top: WindowServer, Dock, Finder, login items, autostart apps. That delta is the "multi-user is heavy" memory people have. **It does not apply to service-only users.**

## Why this matters for multi-tenant daemon designs

Suppose you want to run three Hermes Agent daemons on a single Mac mini, each scoped to different data, and you want them isolated from each other so a prompt-injected agent A cannot read agent B's secrets. Two cheap options:

| Option | Mechanism | RAM cost |
|---|---|---|
| 3 macOS service users | UID + chmod 700 home dir + LaunchDaemon `UserName=hermes-<role>` | ~1-1.5 GB total (3x ~300-500 MB Hermes process; per-user OS overhead ≈ 0) |
| 3 Apple Containers | `container run` per role, each container = own Linux VM | ~3-5 GB total (3x Hermes process + 3x ~512MB-1GB Linux guest kernel + base image) |

The 3-5x RAM advantage for multi-user is real, AND it's because we're literally NOT running 3 macOS instances. Apple Containers literally runs 3 Linux kernels (one per container) via the Apple Virtualization framework. macOS multi-user runs ONE macOS kernel. The container path is paying for a stronger isolation guarantee (hardware-VM boundary vs POSIX); the multi-user path is paying for what you actually need if the tenants are operator-trusted.

If your tenants are mutually trusted (you operate all of them), POSIX multi-user is enough. If a tenant ever becomes operator-untrusted (e.g., you onboard an external collaborator with their own Hermes daemon), you flip to Apple Containers for the kernel-level barrier.

## The "GUI fast user switching" trap

People who've used macOS's **Login Window > Switch User** with two or three GUI sessions running concurrently remember it being heavy. It is. Each active GUI session adds ~500 MB-1 GB. After three concurrent sessions you've lost 2-3 GB to GUI subsystems alone.

That experience generalizes to "multi-user macOS is expensive" in folk memory. The accurate version is "multi-user GUI is expensive; multi-user services is free." If you can express your multi-tenant problem with LaunchDaemons under different UserNames (no GUI auto-login, no `~/Library/LaunchAgents/`), you sidestep the GUI cost entirely.

## How to spot this in a design discussion

Whenever someone says "multi-user is expensive on macOS," ask one question: **are we talking about GUI-session-per-user or service-only-user?**

If the proposed design uses LaunchDaemons (system-context with `UserName=foo`) and never has the user GUI-log-in, it's the cheap variant. If the design contemplates multiple desktop sessions running simultaneously (rare in server-style use), it's the heavy variant.

When a teammate proposes containers / VMs for "multi-tenant on macOS," check first whether plain LaunchDaemon-as-user would suffice. The cost difference is large enough to matter (~3-5x RAM for ~3 tenants) and the operational complexity difference is bigger (no image rebuild loop, no per-container 1Password injection, no LaunchDaemon-wrapping-`container run` plumbing).

## The bigger lesson

UNIX multi-user is decades old and still works. macOS preserved that machinery; it just buried it under "user" in System Settings, which most people associate with "another desktop login." If your problem fits the daemon-per-UID shape, reach for it before reaching for containers. Containers are the right answer when the trust model goes beyond POSIX (hostile tenants, kernel-level threats, image-as-deployment-unit), not when "isolation between mutually trusted daemons" is what you actually need.

The cheapest isolation you don't need is the most overengineered. POSIX multi-user is what we already have. Use it where it fits.

## Related

- [[apple-containers-overview-the-macos-native-microvm-runtime]] — when you DO need hardware-VM isolation, Apple Containers is the macOS-native answer
- [[firecracker-microvms-do-not-run-on-macos]] — the Linux-equivalent isolation primitive doesn't apply on Mac
- [[threat-model-split-cross-tenant-isolation-vs-per-agent-damage-containment]] — when multi-user POSIX is enough vs when you need stronger
- [[opt-in-beats-all-in-for-coding-agent-sandboxing]] — adjacent design pattern: pick the right boundary for the actual threat
