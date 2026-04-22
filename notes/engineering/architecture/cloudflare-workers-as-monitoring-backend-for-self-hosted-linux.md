---
title: Cloudflare Workers as a monitoring backend for self-hosted Linux
type: article
date: 2026-04-22
tags: [monitoring, cloudflare, workers, self-hosted, infrastructure]
status: refined
---

# Cloudflare Workers as a monitoring backend for self-hosted Linux

## Context

Running one or a handful of Linux VPS hosts as a solo operator. Need uptime + suspicious-behavior detection. External SaaS (Datadog, New Relic) is overkill and costly. Self-hosting another monitoring server (Uptime Kuma, Prometheus stack) defeats the purpose, because the monitor itself needs monitoring.

Cloudflare's free tier (Workers, D1, KV, Cron Triggers, Analytics Engine) turns out to be the sweet spot for this scale: no server to run, generous free quotas, and the natural topology puts monitoring outside the hosts it watches.

## The pattern

```
┌──────────────────────┐          ┌───────────────────────────┐
│   Each VPS           │          │   Cloudflare              │
│                      │          │                           │
│  Agent (cron 1min):  │ ──POST──▶│  Ingest Worker            │
│   • collect signals  │ HTTPS    │   verify HMAC             │
│   • diff vs baseline │ (HMAC)   │   write to D1             │
│   • sign + POST      │          │   run rules               │
│                      │          │   → enqueue alert         │
│                      │          │                           │
│                      │   ◀Cron  │  External Prober Worker   │
│                      │    5min  │   (TCP/HTTP probe)        │
│                      │          │                           │
│                      │          │  Alert Dispatcher         │
│                      │          │   → Discord / Telegram    │
└──────────────────────┘          └───────────────────────────┘
```

Three independent Workers, one agent per host, three CF products (Workers, D1, KV). Fits in the free tier comfortably for up to ~10 hosts at 1-min cadence.

## Why this shape works

**The agent is dumb on purpose.** It collects raw signals and ships them. All rule logic lives in the Worker, which is easier to iterate on than SSHing into N hosts to redeploy agent logic.

**External prober + internal agent are complementary.** If the VPS is down, the agent cannot tell you. If the agent is compromised and lying, the external prober still notices unreachability. Two independent failure modes, two independent detectors.

**HMAC over TLS** for ingest auth. The agent holds one secret (per-host ideally), Worker verifies on every request. No mTLS complexity, no certificate rotation.

**Dedup in KV, not D1.** Active-alert deduplication is TTL-based (15-min window is a good default); KV's TTL support is exactly right. D1 stores the historical record; KV stores the live state.

## Why not the usual options

| Option | Blocker |
|---|---|
| Uptime Kuma on another VPS | Correlated failure + ops burden for the watcher |
| Netdata Cloud | Great for metrics, weak for host-intrusion rules |
| Prometheus stack | Over-engineered for < 10 hosts; on-disk TSDB does not fit serverless |
| Wazuh / OSSEC | Heavy manager; not a solo-operator shape |
| UptimeRobot / Better Stack | Uptime only; cannot see inside the box |

The CF path is specifically good at this scale because:

- You already need outbound HTTPS from the VPS for updates, so no firewall changes.
- Free tier numbers (100k Worker requests/day, 100k D1 writes/day, 5 GB D1) are absurd compared to what one agent at 1/min produces (1440/day).
- Workers can be deployed via `wrangler publish` from a laptop in seconds; the iteration loop on rule tuning is faster than SSH-redeploy on a host.

## When this breaks down

- **Hosts that cannot reach Cloudflare** (air-gapped, heavy egress filtering). The whole design assumes outbound HTTPS works.
- **Compliance needing on-premises log retention.** D1 stores data in Cloudflare's infra; not suitable if that is a blocker.
- **Sub-second detection needs.** 1-min cadence + ~1-2s Worker write means detection latency is measured in tens of seconds at best. Fine for host-level anomalies; wrong for HFT-style infra.
- **Fleet > ~20 hosts at 1-min cadence.** Free tier starts getting tight. Move to paid or reduce cadence.

## Minimum implementation shape

```
Worker 1: ingest          POST /v1/snapshot  (HMAC-verified)
Worker 2: external-prober Cron */5 * * * *
Worker 3: dispatcher      Queue consumer, POSTs to Discord/Telegram
D1:       hosts, snapshots, alerts, baselines, login_ip_allowlist
KV:       dedup:{host}:{rule}, host_config:{host}
Agent:    one Python stdlib script, HMAC + JSON + subprocess, ~200 lines
```

Worker count is an implementation detail; could be one Worker with routes. Three is clearer for reasoning about failure boundaries.

## How to spot when this pattern applies

You are running 1-20 Linux hosts, you already use Cloudflare for something, you are a solo or small-team operator, and you want host-level anomaly detection without running a dedicated monitoring server. If any one of those is not true, reach for a different tool.

## Takeaway

Cloudflare's free tier is enough of a "poor-man's monitoring SaaS" that rolling your own backend is faster than installing one of the heavy OSS stacks. The agent is the only part that needs care; the rest is plumbing that Wrangler deploys in seconds.

## Related

- [[hids-lite-rule-set-for-single-operator-vps]] - companion note; this one is the runtime shape, that one is the detection logic that runs inside the Worker
- [[saas-cto-security-checklist]] - the monitoring-and-response items this pattern operationalizes for a solo shop
- [[the-sre-model]] - institutional SRE contrast; this pattern is what "solo SRE" looks like at the other extreme of scale
