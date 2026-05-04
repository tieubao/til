---
title: "Tailscale plus NordVPN plus iCloud Private Relay coexistence on iOS and macOS"
date: 2026-04-29
captured: 2026-04-29T02:50:27.972Z
tags: ["tailscale", "nordvpn", "privacy", "homelab"]
source: "Claude.ai chat"
---
## Context

Three-device personal setup needs to coexist with two opt-in privacy/anonymity tools that fight for the OS's single VPN slot:

- **iPhone** - primary mobile, sometimes uses NordVPN, rarely uses iCloud Private Relay
- **MacBook Air** - remote work terminal across various networks (home, cafes, travel)
- **Mac mini** - homelab server at home, always on

The constraint: iOS allows only one active VPN at a time. NordVPN and Tailscale will fight. iCloud Private Relay also conflicts with anything that intercepts Safari traffic.

## Topology

![Three-device Tailscale topology](https://assets.han-ws.workers.dev/i/2026/04/tailscale-han-three-device-topology.svg)

## Decision: per-device configuration

### Mac mini (the anchor)

| Setting | Value | Rationale |
|---------|-------|-----------|
| Mode | Always-on, kernel networking | Server, no battery concerns, must be reachable 24/7 |
| Exit node | **Advertise** | Lets Air and iPhone full-tunnel through home IP |
| Subnet router | Advertise home LAN (`192.168.1.0/24`) | Reach printers, NAS, IoT remotely |
| MagicDNS hostname | `homelab.tail-xxxx.ts.net` | Clean SSH target |
| Tailscale SSH | Enabled | Skip key management |
| Run as | System service (`tailscaled` with `--operator=$USER`) | Survives logout/reboot |

### MacBook Air

| Setting | Value | Rationale |
|---------|-------|-----------|
| Mode | Always-on | Tailnet must always be reachable for work |
| Build | **Standalone .pkg from tailscale.com**, NOT Mac App Store | MAS variant has known bug silently disabling iCloud Private Relay even when not connected |
| Exit node | Use Mac mini when on untrusted Wi-Fi | Doubles as Nord-replacement on cafe/airport networks |
| Userspace networking | Off by default; enable when running Nord simultaneously | Avoids kernel-level firewall conflicts |
| MagicDNS | On | `ssh homelab` works |

### iPhone

| Setting | Value | Rationale |
|---------|-------|-----------|
| Mode | VPN On Demand | Best balance of battery, local mDNS, remote access |
| Wi-Fi rule | **Except On** → home_wifi | Tunnel off at home (AirPlay/HomeKit work), auto-on elsewhere |
| Cellular rule | Always | Tailnet always available on cell |
| Exit node | Off by default | Battery; enable manually only on hostile Wi-Fi |
| MagicDNS | On | Bitwarden/Vaultwarden, dashboards resolve cleanly |

## Conflict matrix

| Tool combo | Outcome | Recovery |
|------------|---------|----------|
| Tailscale + NordVPN on iOS | Tailscale silently disabled (single VPN slot) | Reopen Tailscale app, toggle on |
| Tailscale + NordVPN on macOS (kernel mode) | Nord's aggressive firewall blackholes tailnet packets | Quit Nord, OR run Tailscale in userspace mode (SOCKS5 proxy) |
| Tailscale (no exit node) + Private Relay | **Coexists fine** | Tailscale routes only 100.64/10, Safari traffic untouched |
| Tailscale exit node + Private Relay | Conflict - exit node makes Tailscale full-tunnel | Disable exit node when using Private Relay |
| Tailscale MAS build + Private Relay | Silent break even when "not connected" | Use standalone .pkg build |
| Tailscale + Mullvad (as exit node) | Single tunnel, no conflict, Private Relay still works | N/A - recommended replacement |

## Alternatives considered

### Mullvad as Tailscale exit node ($5/mo for 5 devices)

**Pros:** Single tunnel slot used. No NordVPN coexistence problems. Works alongside Private Relay (no exit node conflict because the exit node IS the privacy layer). Native integration via Tailscale admin console. No second app to install.

**Cons:** Fewer countries than Nord (~40 vs ~60+). No Nord-specific features (Meshnet, Threat Protection, dedicated IP, double-VPN). Requires paid Tailscale plan.

### Status quo (keep Nord, manage conflicts manually)

**Pros:** Keep existing Nord subscription. No subscription changes.

**Cons:** Constant friction. Tailscale silently disabled when Nord starts. iOS users must remember to re-enable. macOS needs userspace mode workaround. Confusing failure modes.

### Hybrid: Mullvad-as-exit-node + Nord kept for edge cases

**Pros:** Best of both. Mullvad covers 95% of "I want privacy on this network." Nord stays for specific country exits or features Mullvad lacks.

**Cons:** Two subscriptions. Still need to manage Nord conflicts when actually using it.

## Day-in-the-life scenarios

- **At home, working on Air**: Tailscale on, no exit node, no Nord. SSH to mini works as `ssh han@homelab`. Internet direct via home Wi-Fi. Private Relay (if on) works for Safari.
- **At a cafe, working on Air**: Tailscale always-on, exit node = Mac mini (or Mullvad). Traffic tunnels through home or Mullvad. SSH/homelab still work.
- **iPhone away from home, accessing homelab**: Out of home Wi-Fi → On Demand auto-connects → Bitwarden/dashboards/SSH all work. Zero manual action.
- **iPhone with geo-bypass need**: With Mullvad - tap "exit node → Mullvad US" in Tailscale app. With Nord - toggle Nord, lose Tailscale, re-enable Tailscale after.
- **Rare Private Relay session on Air**: Disable exit node. Base Tailscale tunnel coexists fine.
- **Family AirPlays at home**: iPhone Tailscale OFF (home_wifi in Except On). mDNS/AirPlay works.

## Consequences

**Gains:**
- Mobile homelab access auto-engages (no manual toggling)
- Cafe Wi-Fi protection without launching Nord
- AirPlay/HomeKit work at home (Tailscale off via Except On)
- Predictable conflict handling, clear recovery paths
- Lower iPhone battery drain
- Private Relay coexists when not using exit node

**Losses:**
- Small purity loss: at-home homelab access uses LAN IPs (192.168.x.x via subnet router) instead of tailnet IPs - functionally identical for 99% of cases
- If keeping Nord: still need to manage occasional conflicts manually
- If switching to Mullvad: monthly add-on cost, fewer countries

## Recommended first action

Install standalone .pkg build of Tailscale on Mac mini and Air (not App Store). Set mini as exit node + subnet router. Configure VPN On Demand on iPhone with "Except On: home_wifi" + Cellular "Always". Run for one week. Then decide on the Mullvad question.

## How we'll know this was wrong

- iPhone tunnel keeps reconnecting/disconnecting at home (rule misconfigured - debug with the OS Wi-Fi history)
- AirPlay still broken at home (Tailscale not actually disconnecting on home_wifi join - verify SSID match exactly)
- Cafe browsing slow despite exit node (Mac mini upload bandwidth from home is the bottleneck - switch to Mullvad)
- Need Nord weekly (Mullvad-as-exit-node is the right move; cancel Nord)

## Related

- [[tailscale-vpn-on-demand-feature-overview-and-rule-semantics]] - the rule semantics this design relies on (Except On home_wifi, Cellular Always)
- [[when-to-add-tailscale-to-a-personal-dev-surface]] - the smaller "should I bother with Tailscale at all" decision; this note assumes you already said yes
- [[portless-vs-tailscale-magicdns-not-equivalent]] - Tailscale and portless cover different layers; this note covers the Tailscale layer's privacy-tool conflicts
- [[wireguard-static-ip-exchange-whitelist]] - how the underlying WireGuard transport works without a proprietary control plane (Headscale path implied)