---
title: "Apple Containers (apple/container): the macOS-native microVM runtime"
date: 2026-05-04
captured: 2026-05-04T15:55:00+07:00
tags: ["apple-containers", "macos", "vm", "sandboxing", "virtualization"]
source: "agentkernel + Hermes brainstorm 2026-05-04"
aliases: []
status: refined
---

**`apple/container` is Apple's open-source container platform for macOS Apple Silicon, built in Swift, runs each container as a lightweight Linux VM via the Apple Virtualization framework + the `apple/containerization` Swift package.** OCI-compatible (Docker images load). It is what AWS Lambda would look like if AWS Lambda ran on a Mac. It is also what `agentkernel` and similar wrapper tools use underneath when they detect macOS as the host.

Status as of 2026-05: pre-1.0, "currently under active development." Worth understanding because agentkernel-on-macOS uses it, and because for any planned multi-tenant sandboxing pattern on Apple Silicon, this is the framework you build against if you want hardware-VM isolation per tenant.

## What it is, exactly

| Facet | Detail |
|---|---|
| Vendor | Apple, open source at `github.com/apple/container` |
| Language | Swift, optimized for Apple Silicon |
| Underlying runtime | `apple/containerization` Swift package + Apple Virtualization framework (the same framework Lima / Tart / Apple's Virtualization.framework apps use) |
| Per-container model | Each container = its own lightweight Linux VM with its own kernel |
| Image format | OCI-compatible. Pulls from any container registry. Docker images work. |
| Platform requirement | **macOS 26+ AND Apple Silicon required**. Explicitly not supported on Intel Macs or older macOS. |
| Maturity | Pre-1.0. Apple says breaking changes possible until v1.0. Maintainers won't fix issues on older macOS. |

If you've used Docker Desktop or OrbStack, the conceptual model is similar (run Linux containers on a Mac), but the runtime is materially different: Docker / OrbStack run a single Linux VM and orbit containers as namespaces inside that VM (one shared kernel). Apple Containers gives each container its own VM with its own kernel.

## Native CLI primitives that work

From `container run --help`:

```
PROCESS:
  -e, --env <env>          Set environment variables (key=value)
  --env-file <env-file>    Read env from file
  -i, --interactive        Keep stdin open
  -t, --tty                Open a TTY
  -u, --user <user>        Set user inside container

RESOURCE:
  -c, --cpus <cpus>        Allocate CPUs
  -m, --memory <memory>    Allocate memory (1MiByte granularity)

MANAGEMENT:
  -d, --detach             Run detached
  --dns <ip>               DNS nameserver
  --entrypoint <cmd>       Override entrypoint
  -k, --kernel <path>      Custom kernel
  --mount <mount>          Add a mount: type=<>,source=<>,target=<>,readonly
  --name <name>            Container name
  --network <network>      Attach to a network
  -p, --publish <spec>     Publish port: [host-ip:]host-port:container-port[/proto]
  --rm, --remove           Remove container after exit
  --ssh                    Forward SSH agent socket into container
  --tmpfs <tmpfs>          Add a tmpfs mount
  -v, --volume <volume>    Bind mount a volume
```

Bind mounts work. Volume mounts work. Env injection works. Network attachment to a named network works. SSH agent forwarding is a thoughtful detail. The framework primitives are intact and roughly Docker-compatible; if you've shelled containers before, the muscle memory transfers.

## Documented gaps

| Gap | Detail |
|---|---|
| No `--restart` policy | Apple Containers does not auto-restart on failure. For daemon-style use, wrap `container run` in a launchd LaunchDaemon with `KeepAlive=true` to substitute. |
| No documented `--network none` | The `--network <name>` flag attaches to a named network. There's no documented "fully disable egress" mode. To kill egress, build a custom network with no upstream via `container network create`, or use macOS PF firewall rules to block the container subnet. |
| No documented `host.docker.internal` equivalent | Reaching host services from inside a container (e.g., Ollama on `127.0.0.1:11434`) is not documented. The default network at `192.168.64.0/24` lets containers reach the internet but host-loopback access patterns are unclear. |
| Persistence across reboots | Whether `--detach` containers survive host reboots is not documented. Assume ephemeral and use a LaunchDaemon to re-create at boot if you need a daemon-style service. |
| `container system start/stop` semantics | These manage the platform's own LaunchDaemons (`com.apple.container.*`), not individual containers. Don't confuse "start the container system" with "start my container". |

## When to reach for it

Apple Containers is the right tool when:

- You're on macOS 26+ Apple Silicon AND
- You need hardware-VM isolation per workload (not just namespace isolation), AND
- You want to run untrusted Linux workloads side-by-side with native macOS work, OR
- You're building tooling that should portably target both Apple Silicon and Linux microVMs (Apple Containers on Mac, Firecracker on Linux). Wrappers like `agentkernel` already do this auto-detection.

It is NOT the right tool when:

- You're on pre-macOS-26 or Intel Mac. Use colima (production-shaped, runs Lima underneath) or OrbStack (developer-shaped, GUI) or Docker Desktop instead. All three give you Docker-compatible Linux containers via a shared Linux VM.
- You need production-critical stability today. Pre-1.0 status means breaking changes are possible. For production work where uptime matters, colima is more battle-tested.
- Your tenants are trusted-by-the-same-operator (e.g., several daemons all owned by you). POSIX multi-user gives you "tenants can't read each other's filesystems" at a fraction of the resource cost without spinning up N Linux kernels. See [[macos-multi-user-cost-myth-gui-vs-service-users]] for why.

## Coexistence with colima / OrbStack / Docker

| Concern | Reality |
|---|---|
| Conflict with colima | None observed. Different runtimes, different state directories (`~/.colima/` vs Apple's). |
| Brewfile drift | Adding `apple/container` is a new install; track in `Brewfile`. |
| TCC entitlements | `container system start` registers `com.apple.container.*` LaunchDaemons. May need additional TCC grants similar to OrbStack's known issue with `~/Library/Group Containers/`. Untested at the time of writing; verify in your environment if you bring it up alongside colima. |
| Image cache | Separate from Docker / colima caches. Same image pulled twice if you use both. |

## Practical use today

The most common practical surface is via wrappers, not direct CLI:

- **`agentkernel`** uses Apple Containers as its macOS backend. Caveat: agentkernel v0.16.0 and v0.18.1 silently no-op on `--no-network`, `--dir`, `--secret-file` per [[agentkernel-broken-flags-on-apple-containers]]. The framework primitives are fine; the wrapper has bugs.
- **Direct `container run`** for ad-hoc "run this image with these mounts" work. Manual but reliable.
- **Future**: as more tooling targets Apple Containers, the wrapper ecosystem will mature. Today (May 2026) the surface is sparse but functional.

## The bigger lesson

Apple Containers is the closest thing macOS has to "Linux-microVM isolation as a first-class primitive." Until Apple ships v1.0, treat it as a useful but pre-stable tool: build with awareness of the gaps, don't put production workloads on it without redundancy, and keep an eye on the release notes (every 3-5 weeks based on current cadence).

For most macOS sandboxing use cases (untrusted code, multi-tenant daemons, per-task isolation), check first whether you actually need VM-level isolation. POSIX multi-user is enough for many cases at far lower cost. Reach for Apple Containers when the tenants don't trust each other at the kernel level - that's the threshold where hardware-VM separation starts paying for itself.

## Related

- [[agentkernel-broken-flags-on-apple-containers]] - concrete wrapper-bug case for the macOS Apple Containers backend
- [[firecracker-microvms-do-not-run-on-macos]] - Apple Containers is what Firecracker would be if Firecracker ran on a Mac
- [[macos-multi-user-cost-myth-gui-vs-service-users]] - multi-user vs Apple Containers cost comparison; multi-user is cheaper when tenants are mutually trusted
- [[threat-model-split-cross-tenant-isolation-vs-per-agent-damage-containment]] - Apple Containers serves both threat models depending on configuration
