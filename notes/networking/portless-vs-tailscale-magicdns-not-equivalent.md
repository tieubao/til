---
title: "portless vs Tailscale MagicDNS not equivalent"
date: 2026-04-30
captured: 2026-04-30T16:39:33.201Z
tags: ["portless", "tailscale", "devtools", "reverse-proxy"]
source: "Claude.ai chat"
---
## Question

Is `vercel-labs/portless` equivalent to Tailscale MagicDNS?

## Answer

No. They solve different problems at different layers and the only thing they share is "replace numeric stuff with names" - but they replace different numeric stuff.

- **portless** is a **local reverse proxy** that gives dev servers pretty hostnames (`https://myapp.localhost`) instead of `localhost:3000`. Single machine, single developer.
- **Tailscale MagicDNS** is a **DNS layer over a mesh VPN** that lets you reach any machine on your tailnet by name (`han-laptop`, `homelab-vps`) from anywhere in the world.

## How portless actually works

![portless architecture](https://assets.han-ws.workers.dev/i/2026/04/portless-architecture.svg)

Key technical details:

- HTTP reverse proxy listening on one port (1355 by default, or 443 with `--https`).
- Exploits the fact that `*.localhost` is a reserved TLD that auto-resolves to 127.0.0.1 in most browsers (RFC 6761), so no DNS modification, no `/etc/hosts` editing, no resolver tricks.
- Auto-discovers monorepo packages from `pnpm-workspace.yaml` or the `workspaces` field in `package.json`.
- Generates a local CA, gets it trusted by the OS, issues per-hostname certs so HTTPS works without browser warnings.
- Auto-injects `PORT=<random>` into your dev script so frameworks don't collide.

The use case is pure DX for local development on one machine: replace `localhost:3000`, `localhost:3001`, `localhost:8080` with `myapp.localhost`, `api.localhost`, `docs.localhost`, all on the same port, all over HTTPS, no port collisions when you spin up a second project.

## What Tailscale MagicDNS does

MagicDNS runs on top of Tailscale's WireGuard mesh VPN and gives every device on the tailnet a stable name. Type `homelab` into a terminal, the Tailscale resolver intercepts the lookup, returns the homelab's tailnet IP (in the `100.x.y.z` range), and the WireGuard tunnel carries traffic to that machine.

Three things MagicDNS does that portless doesn't:

1. Builds an authenticated overlay network across the public internet (WireGuard).
2. Resolves names to private tailnet IPs (DNS).
3. Routes traffic encrypted, peer-to-peer, between devices.

## Side-by-side

| Axis | portless | Tailscale MagicDNS |
|---|---|---|
| Layer | L7 reverse proxy | L3 VPN + DNS |
| Scope | One machine, one dev | All devices on the tailnet, anywhere |
| What it routes | HTTP(S) requests to dev servers | Any IP traffic between machines |
| Identity / auth | None (it's local) | Tailscale identity (SSO, ACLs, tags) |
| Naming source | `.localhost` TLD + Host header | DNS over the tailnet |
| Persistence | Runs while developing | Always-on background daemon |
| Why use it | Pretty URLs and HTTPS for dev servers | Reach private services from anywhere |

## When to reach for which

- Dev server has ugly port numbers and `EADDRINUSE` keeps happening -> portless.
- Need to SSH or hit a service on a homelab/VPS from a laptop while traveling -> Tailscale (MagicDNS comes for free).
- Want a teammate or a phone to hit a local dev server -> Tailscale, or specifically Tailscale Funnel for public access.

## They actually compose well

Recent portless releases added explicit Tailscale integration: a `--tailscale` flag and a `--funnel` flag.

> "New --tailscale flag shares any portless app over your Tailscale network with zero framework config. Each app is root-mounted on its own Tailscale HTTPS port (443, 8443, 8444, ...) so no basePath configuration is needed."

> "New --funnel flag exposes apps to the public internet through Tailscale Funnel."

portless handles local proxying and HTTPS, Tailscale handles transport across machines. Stacked, not competing.

![portless plus tailscale layered](https://assets.han-ws.workers.dev/i/2026/04/portless-plus-tailscale.svg)

## Why the "are they equivalent" intuition is wrong

The trap is that both replace numeric stuff with names. But:

- portless replaces **port numbers** (`:3000`, `:8080`) with **hostnames** on a single machine.
- MagicDNS replaces **IP addresses** (`100.64.x.y`) with **hostnames** across a fleet.

If portless is "nginx + .localhost trick + auto cert generation", then MagicDNS is "WireGuard + a private resolver". Different parts of the stack.

## Key Takeaway

portless and Tailscale MagicDNS aren't competitors - they're different layers of the same stack. portless is L7 application routing for one machine; MagicDNS is L3 network addressing across many. The naming overlap is superficial; the layer separation is what matters when picking one.

## Related

- [[portless-competitive-landscape-no-exact-1-to-1-competitor]] - full landscape map across reverse proxies, tunnels, and VPN-adjacent tools
- [[when-to-add-tailscale-to-a-personal-dev-surface]] - when MagicDNS-style cross-machine access is the actual problem you have
- [[tailscale-vpn-on-demand-feature-overview-and-rule-semantics]] - the iOS/macOS-only auto-connect logic that makes Tailscale ergonomic enough to coexist with portless
- [[wireguard-static-ip-exchange-whitelist]] - what Tailscale's underlying transport (WireGuard) looks like without the proprietary control plane