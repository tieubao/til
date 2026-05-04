---
title: "Tailscale VPN On Demand feature overview and rule semantics"
date: 2026-04-29
captured: 2026-04-29T02:49:36.564Z
tags: ["tailscale", "ios", "macos", "vpn"]
source: "Claude.ai chat"
---
VPN On Demand is an iOS/macOS-only Tailscale feature that auto-connects or disconnects the tailnet tunnel based on network conditions. It solves the two real-world frictions of always-on Tailscale: interference with local mDNS services (AirPlay, Chromecast, HomeKit, printers) and unnecessary battery drain on cellular.

## Availability

- iOS: Tailscale 1.48+
- macOS: Tailscale 1.60+
- **Not available on Android, Windows, Linux** - uses Apple's NetworkExtension framework.

## How it works

Whenever the OS detects a network change (Wi-Fi join/leave, cellular handoff, Ethernet plug), it evaluates the configured rule for that interface and either connects, disconnects, or holds Tailscale.

![VPN On Demand decision flow](https://assets.han-ws.workers.dev/i/2026/04/tailscale-vpn-on-demand-decision-flow.svg)

## Rule types per interface

Each interface (Wi-Fi, Cellular, Ethernet) supports five rules:

| Rule | Behavior |
|------|----------|
| **Always** | Connect on any active connection |
| **Only On** | Connect only on listed networks (allowlist) - Wi-Fi only, by SSID |
| **Except On** | Connect on any network except listed (blocklist) - Wi-Fi only |
| **Never** | Always disconnect on this interface |
| **Do Nothing** | No auto behavior - manual control, but enables MagicDNS triggers |

## MagicDNS auto-trigger

When the rule is set to **Do Nothing**, Tailscale auto-connects the moment any app on the device tries to resolve a hostname ending in `*.ts.net`. Any other rule disables this MagicDNS-based triggering.

This is the "lazy connect" pattern: useful when you want the VPN up only when something on the device actively reaches for a tailnet hostname (e.g. Bitwarden trying to sync with Vaultwarden).

## Use cases

1. **Homelab access without breaking local casting.** Set Wi-Fi to **Except On** with home SSID. Tailscale stays off at home (mDNS, AirPlay, Chromecast work normally) and auto-connects everywhere else.
2. **Lazy-connect for password managers.** Set Wi-Fi to "Do Nothing" + MagicDNS. App tries to reach `vaultwarden.xxx.ts.net` → tunnel comes up.
3. **Cellular-only tunnel.** Wi-Fi **Only On** with trusted SSIDs allowlisted, Cellular **Always**. Tunnel runs on cell data and untrusted Wi-Fi but skips trusted networks.
4. **BYOD compliance posture.** On managed iOS fleets, push rules so the tunnel auto-engages outside the office.
5. **Battery-conscious always-on.** Both interfaces "Do Nothing", relying purely on MagicDNS triggers.

## Trade-offs

- **The disconnect side is rough.** Once MagicDNS fires the tunnel up, there's no condition to take it back down. It stays connected until the network changes.
- **Single VPN slot on iOS/macOS.** Connecting any other VPN with On Demand enabled silently disables it for Tailscale. You must reopen the Tailscale app and reconnect manually.
- **A bad rule lockout.** Setting an interface to "Never" causes the OS to immediately disconnect any manual reconnect. Recovery requires editing rules or disabling On Demand.
- **No exit-node logic.** Users currently work around this with iOS Shortcuts watching the SSID.

## Key takeaway

VPN On Demand is the right default for **mobile homelab/personal infrastructure access**. The "Except On home_wifi" Wi-Fi rule + Cellular "Always" combination eliminates the constant "is Tailscale on?" cognitive overhead while preserving local network functionality at home.

## Related

- [[tailscale-plus-nordvpn-plus-icloud-private-relay-coexistence-on-ios-and-macos]] - the per-device design that uses these rule semantics
- [[when-to-add-tailscale-to-a-personal-dev-surface]] - the prior decision; once you adopt Tailscale, On Demand is how you live with it on iOS without battery pain
- [[portless-vs-tailscale-magicdns-not-equivalent]] - MagicDNS triggers ("Do Nothing" + auto-resolve) are part of the On Demand surface
- [[wireguard-static-ip-exchange-whitelist]] - what Tailscale's WireGuard transport looks like outside the Apple-only NetworkExtension features