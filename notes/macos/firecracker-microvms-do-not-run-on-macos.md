---
title: "Firecracker microVMs do not run on macOS; reach for Apple Containers instead"
date: 2026-05-04
captured: 2026-05-04T15:30:00+07:00
tags: ["firecracker", "microvms", "macos", "sandboxing", "virtualization"]
source: "agentkernel + Hermes brainstorm 2026-05-04"
aliases: []
status: refined
---

**Firecracker (`firecracker-microvm/firecracker`) requires a Linux host with KVM enabled. There is no native macOS port.** The README's "Tested platforms" table lists only AWS EC2 instances (Intel + AMD x86_64, plus Graviton ARM64). No Darwin, no Apple Silicon. The build instructions assume "Unix/Linux system that has Docker running" - read: Linux only.

This catches people who hear "lightweight microVMs, sub-second boot, ~5 MB overhead" from the AWS Lambda / Fly.io world and assume Firecracker is the answer for sandboxing on a developer laptop or Mac mini. It is not.

To use Firecracker on a Mac you would need:

```
macOS (Apple Silicon)
  └── Lima/Tart Linux VM via Apple Virtualization framework
       └── Firecracker microVMs (one per workload)
```

That is two layers of virtualization. The outer Linux VM is the heavy thing - the whole pitch of Firecracker (lightweight, fast boot, low memory tax) is wasted because you are paying the Linux-guest cost up front before any Firecracker microVM exists. By the time you have a working setup, you have rebuilt a small Vultr server inside your Mac.

If you specifically want microVM-grade isolation on macOS, the right tool is **Apple Containers (`apple/container`)**: macOS 26+ on Apple Silicon required, written in Swift, uses the Apple Virtualization framework natively, runs each container as its own Linux VM. OCI-compatible images (Docker images load). It is what `agentkernel` uses underneath on macOS, and it is what AWS Lambda / Fly.io would use if they ran on a Mac.

Quick decision rule:

| Goal | Right tool |
|---|---|
| Microvm sandboxing on Linux | Firecracker (or a wrapper like agentkernel that picks Firecracker on Linux) |
| Microvm sandboxing on macOS Apple Silicon, macOS 26+ | Apple Containers (or a wrapper that picks it on macOS) |
| Microvm sandboxing on older macOS / Intel Mac | Docker / Podman / colima (namespace isolation, not VM) |
| "I just want to run untrusted code in a clean Linux env on my Mac" | colima or Apple Containers |

The bigger lesson: when a tool is famous for running in a specific cloud (Firecracker = AWS Lambda / Fargate), check the host-platform list before architecting around it. Cloud-native tools often share an ABI with their cloud's host kernel; bringing them to a different host is layered virtualization and erodes the benefit you came for.

## Related

- [[apple-containers-overview-the-macos-native-microvm-runtime]] - what to use instead on macOS
- [[agentkernel-broken-flags-on-apple-containers]] - agentkernel's Apple Containers backend has its own gotchas; Firecracker is not a workaround
- [[macos-multi-user-cost-myth-gui-vs-service-users]] - if you reach for Firecracker because "macOS multi-user is expensive", check that assumption first
