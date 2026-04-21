---
title: Static outbound IP solutions for crypto trading bots, ten options compared
type: article
created: 2026-04-19
tags: [crypto-exchange, static-ip, trading-infra, vps, vpn, cloudflare, fly-io, decision-framework]
---

# Static outbound IP solutions for crypto trading bots, ten options compared

Companion to [[wireguard-static-ip-exchange-whitelist]]. The WireGuard-on-VPS note tells you what to build; this note catalogs every other option you might consider and explains why each one wins or loses for this specific problem.

## The question behind the question

"Where do I get a static IP for my trading bot?" sounds like one question. It's actually three:

1. Where does the compute live that runs the bot?
2. Where does the outbound traffic exit to the internet?
3. Is the exit IP stable and owned by you alone?

Most commercial static-IP products bundle answers to (2) and (3) together, and implicitly force an answer to (1). The confusion is thinking "static IP" is one thing. It's not. It's a property of whatever infrastructure handles your packets at egress.

## The ten options, categorized

**Category A - VPS with full control (you rent the compute, you rent the IP with it)**
1. Vultr / Linode / Oracle / DigitalOcean + WireGuard tunnel
2. Hetzner Cloud ARM

**Category B - VPN services sold to consumers**
3. NordVPN, Surfshark, CyberGhost with dedicated-IP add-on
4. Crypto-targeted VPNs (ipsabet et al)

**Category C - Enterprise network products**
5. Cloudflare Zero Trust + dedicated egress IPs
6. Cloudflare Aegis for Workers

**Category D - Static proxy services**
7. QuotaGuard Shield, Fixie
8. IPRoyal, Bright Data, Smartproxy (residential static)

**Category E - Platform-as-a-Service workarounds**
9. fly.io + static egress IP add-on
10. Business-tier ISP static IP

## Category A: VPS with full control

### Option 1: Vultr / Linode / Oracle / DigitalOcean + WireGuard

**What it is.** Rent the cheapest Linux VM in the region closest to your exchange. Install WireGuard. Route exchange-bound traffic through it. The VM's public IPv4 becomes your whitelisted IP.

**Cost.** $2.50-12/mo across Vultr / Linode / DigitalOcean. Oracle Free Tier is $0 if signup works.

**Pros.** Cheapest. Full root control. Clean cloud-provider ASN (exchanges don't flag it as VPN). Trivially extensible (can grow into a Hermes host or signal server later). ~$5/mo total including the IP.

**Cons.** Requires 30-60 min of Linux setup. Client side not persistent across laptop reboots by design.

**When it wins.** Almost always, for solo traders. Only loses on pure convenience if you're deeply allergic to any Linux configuration.

### Option 2: Hetzner Cloud ARM

**What it is.** Same as option 1, but on Hetzner's ARM instances starting at €3.79/mo.

**Cons.** EU-only ARM offering; their Singapore region has x86 only, and pricier. Not suitable for Tokyo-latency use cases.

**When it wins.** If your exchange's matching engine is in Frankfurt (Kraken EU, Bitstamp) and you want ARM pricing. Otherwise skip.

## Category B: Consumer VPN with dedicated IP

### Option 3: NordVPN / Surfshark / CyberGhost dedicated IP

**What it is.** Major consumer VPNs sell a dedicated-IP add-on on top of their regular subscription. ~$3-5/mo extra on a 2-year plan.

**Pros.** Zero Linux setup. Install the VPN client, select your dedicated IP. Works on any OS.

**Cons.**
- Total cost $7-11/mo before add-on, so 2x the VPS price
- Region availability limited: NordVPN dedicated IPs are in 24-30 countries but NOT Tokyo. Surfshark and CyberGhost have Singapore but not Tokyo
- VPN-provider ASNs are sometimes pre-flagged by exchanges for extra fraud scrutiny
- Dedicated IP = single IP on a known VPN range; exchanges occasionally blanket-ban the range if abuse is detected

**When it wins.** Latency-indifferent setup where a $5/mo premium is worth zero Linux exposure, AND your exchange doesn't block VPN ranges.

### Option 4: Crypto-targeted VPN services (ipsabet et al)

**What it is.** Small providers specifically marketing dedicated IPs to crypto traders for exchange whitelisting.

**Pros.** Feature parity with the major VPN services but more targeted positioning.

**Cons.**
- Typically $10-15/mo for dedicated IP, so 2-3x VPS price
- Small providers often have no public company registration / founding year / physical address
- Single-vertical focus means if the provider gets banned by one exchange, their IPs across customers are collateral damage
- Crypto-specific means ToS often crypto-friendly, but exit-scam risk is real

**When it wins.** Essentially never vs option 1 or 3. The crypto-specific marketing is not enough upside to offset the opacity penalty.

## Category C: Enterprise network products

### Option 5: Cloudflare Zero Trust + dedicated egress IPs

**What it is.** Cloudflare's enterprise SWG (Secure Web Gateway) with a dedicated-egress-IP add-on. Your traffic goes through Cloudflare's edge and exits via an IP reserved for your account.

**Cost.** Enterprise Contract plan only. Cloudflare does not publish the dedicated-egress-IP price. Based on typical CF enterprise quotes, expect $500-2000/mo minimum plus a contract floor of $5,000-15,000/year.

**Pros.** If you're already a CF Zero Trust Enterprise customer, adding dedicated egress is genuinely clean.

**Cons.** Wrong scale for retail. No self-serve tier. And you still need compute somewhere to run the bot; Cloudflare only solves the egress IP.

**When it wins.** Never at solo-trader scale. Only makes sense as part of an existing enterprise CF deployment.

### Option 6: Cloudflare Aegis for Workers

**What it is.** Dedicated IPs for outbound traffic from Cloudflare Workers.

**The technical dealbreaker.** Cloudflare's documentation states explicitly: "For `connect()` requests - which create outbound TCP connections from Workers - Dedicated CDN Egress IPs are not used." WebSocket streams from Workers therefore do NOT egress from the dedicated IP. Half your Binance traffic (the market data websocket) would still go through a shared IP pool the exchange doesn't whitelist.

**When it wins.** Never for a trading bot that needs WebSocket streams. Could work for a pure-REST poller with no live data feed.

## Category D: Static proxy services

### Option 7: QuotaGuard Shield, Fixie

**What it is.** Platform-integrated static-IP proxies originally designed for Heroku. You keep your bot on any PaaS / cloud; traffic routes through the proxy which gives a stable exit IP.

**Cost.** ~$19-49/mo.

**Pros.** Works architecturally: bot stays wherever, proxy provides stable IP, TLS passthrough preserves end-to-end encryption.

**Cons.** 4-10x VPS price. You're paying for middleware you don't need if you can just run the bot on the same box.

**When it wins.** If you're stuck with a bot deployed on Heroku / Render / Railway / another PaaS that you genuinely can't migrate off.

### Option 8: IPRoyal, Bright Data, Smartproxy (residential static)

**What it is.** Proxy services offering static IPs sourced from residential networks.

**Cost.** $10-40/mo per dedicated IP.

**Cons.**
- "Residential" often means IPs sourced via murky consent-ware on actual home computers; whether those consents are informed is a real question
- Exchanges increasingly detect and block residential-proxy IPs as a fraud signal
- No control over the IP's reputation history

**When it wins.** Web scraping, not exchange whitelisting. Skip for this use case.

## Category E: PaaS + business ISP

### Option 9: fly.io + static egress IP add-on

**What it is.** Deploy your bot as a fly.io Machine, pay for the static egress IP add-on (~$3.60/mo) to get a stable outbound IP.

**Full cost.** Machine in Tokyo ~$12-15/mo + static egress $3.60/mo + volume + bandwidth = ~$17-20/mo all-in.

**Architectural issues.**
- Fly.io Machines are containers designed for web apps, not long-running stateful daemons
- 30-second CPU limits on some Worker configurations (unless you specifically configure always-on)
- `connect()` vs `fetch()` semantics affect WebSocket handling
- Persistent state requires separately priced volumes

**When it wins.** Never for a self-hosted trading bot. Works fine for webhook handlers or event-driven flows, but a trading bot with live market data wants the straightforward VPS model.

### Option 10: Business-tier ISP static IP

**What it is.** Upgrade your residential ISP plan to a "business" tier that includes a static IP.

**Cost.** $30-100/mo extra, sometimes requires a business registration.

**Pros.** Your home-based IP becomes truly static.

**Cons.**
- Availability varies by region; many consumer ISPs don't offer this at all
- Still a residential ISP range, which exchanges may flag differently from cloud ASNs
- Doesn't help if you ever want to trade while traveling

**When it wins.** Very specific: when you run a home-based crypto operation at a business scale that justifies the upgrade, and your local ISP actually offers it. Rare.

## Decision matrix

Simplified down to the decisions that matter:

| Question | If yes → | If no → |
|---|---|---|
| Willing to run Linux commands? | Option 1 (VPS + WireGuard) | Option 3 (VPN dedicated IP) |
| Exchange matcher in Tokyo, and latency matters? | Option 1 with Tokyo VPS | Option 1 with any region |
| Already on an enterprise CF contract? | Option 5 could work | Skip enterprise options |
| Bot must stay on Heroku/PaaS? | Option 7 (QuotaGuard) | Option 1 |
| Truly $0 budget and patient? | Option 1 with Oracle Free Tier | Option 1 with Vultr $5/mo |

## Category-level scoring

| Category | Typical cost | Control | Reputation | Winner for solo trader? |
|---|---|---|---|---|
| A. VPS | $0-12/mo | Full root | Clean cloud ASN | **Yes** |
| B. Consumer VPN | $7-15/mo | None | VPN ASN (mixed) | Sometimes |
| C. Enterprise | $500+/mo | Partial | Clean | No |
| D. Static proxy | $19-40/mo | None | Varies | Only if locked to PaaS |
| E. PaaS / ISP | $17-100/mo | Varies | Varies | No |

## The meta-takeaway

Every "static IP service" in categories B-E is **re-bundling category A with extra steps and markup**. The IP comes from a machine in a datacenter. The question is just who owns the machine and how much abstraction you want between you and the packet forwarding.

For solo crypto trading, the right abstraction level is "you rent the machine directly." It's the cheapest, most flexible, most extensible, and lowest-trust-required layer. Every rung of the commercial ladder above it is paying someone to manage a machine you could manage yourself in 30 minutes.

## Related

- [[finance-tooling/wireguard-static-ip-exchange-whitelist]] - the how-to for category A (the winning category)
- [[finance-tooling/oss-trading-stack-survey-april-2026]] - where the bot that uses this IP gets built
