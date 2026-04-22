---
title: HIDS-lite rule set for a single-operator Linux VPS
type: article
date: 2026-04-22
tags: [security, monitoring, hids, linux, self-hosted]
status: refined
---

# HIDS-lite rule set for a single-operator Linux VPS

## Context

Full host-intrusion detection (Wazuh, OSSEC) is institutional-scale tooling. A solo operator with a few VPS hosts gets most of the value from a much smaller rule set, running out-of-band on a separate system (e.g. serverless) so a compromised host cannot silence its own alarms.

This note catalogues the rule set I use, what each one catches, what the baseline source is, and what noise to expect. Language is implementation-agnostic; the rules map cleanly to any rule engine.

## Signal sources (what the agent on the host collects)

- `ss -tlnp` and `ss -tunap state established` for listeners and active peers
- `ps -eo pid,comm,user,args` for the process list
- `/etc/passwd`, `/etc/shadow` (hash only), `/etc/cron.*/`, per-user crontabs
- `systemctl list-unit-files` for installed units
- `last -a`, `lastb -a` for login and failed-login records
- `wg show` for WireGuard peer state (if applicable)
- `df`, `free`, `/proc/net/dev` for resource and interface counters
- `fail2ban-client status` for active jails
- AIDE nightly diff against `/var/lib/aide/aide.db` for file integrity
- `/var/log/dpkg.log` for package changes

Each of these is cheap to sample (< 100ms total). Snapshots fit in ~3 KB JSON.

## Rule catalog

| ID | Severity | Condition | Baseline | Known noise |
|---|---|---|---|---|
| `host-unreachable` | CRIT | External prober: TCP/HTTP fail 2 cycles | n/a | Transient blips; require 2/2 |
| `agent-silent` | CRIT | No snapshot received > 10 min | n/a | Agent cron skew |
| `new-listener` | CRIT | Listener set hash changed; new ip:port not in allowlist | `listeners` per host | Docker dev; none on lean boxes |
| `new-user` | CRIT | `/etc/passwd` line count increased | File hash | Rare; package installs almost never add users |
| `new-systemd-unit` | WARN | Unit file set diff | Hash | apt installs; filter by unit name |
| `new-cron` | CRIT | Any user crontab or `/etc/cron.*/` hash changed | Hash | Package post-install scripts |
| `new-login-ip` | CRIT | Successful login from IP not in 30-day allowlist | Rolling query | ISP IP rotation; auto-adds after trusted login |
| `file-integrity-critical` | CRIT | AIDE touches `/bin`, `/sbin`, `/etc`, `/usr/bin` | AIDE db | apt upgrades; re-baseline after upgrade |
| `file-integrity-other` | WARN | AIDE touches anywhere else | AIDE db | Exclude logs, tmp, caches in AIDE conf |
| `new-process` | WARN | Process name unseen 7 days, persists 3 samples | Rolling set | Kernel threads; filter `[]`-named |
| `auth-fail-spike` | WARN | `lastb` 5-min delta > 3σ above 7-day p95 | Rolling | Baseline noise is high if SSH open to internet |
| `outbound-new-ip` | WARN | Established outbound peer IP not in allowlist, port not 443/80/53 | 30-day rolling | Maintain per-host role allowlist |
| `outbound-miner-port` | CRIT | Outbound to port 3333/4444/5555/7777/14444 | n/a | No legitimate reason |
| `bandwidth-spike` | WARN | 5-min egress > rolling 7-day p95 × 3 | Time series | Per-role threshold needed |
| `disk-warn` / `disk-crit` | WARN / CRIT | Mount > 80% / > 90% | n/a | None |
| `dpkg-change` | INFO | New entries in `/var/log/dpkg.log` | n/a | Expected ~1/day from unattended-upgrades |

## Severity routing

- **CRIT**: immediate notification to two channels (primary + redundant).
- **WARN**: primary channel only.
- **INFO**: logged, no notification. Queryable for post-hoc.

## First-run grace period

On a new host, collect baselines for **48 hours** with all rules muted except `host-unreachable` and `agent-silent`. Day 3+: rules active.

A fresh Ubuntu install during its first day does enough legitimate churn (apt upgrades, package post-install scripts, cloud-init finalization) to fire half the rules if not gated. The grace period is not optional.

## Per-host role profiles

Rules need per-role tuning. A process list on an egress/tunnel-only box looks nothing like an engine/compute box.

```
egress role:  allow listeners [22, 51820 (WG)]
              expect processes: sshd, systemd, networkd, wg
              expect outbound: tunnel peers only

compute role: allow listeners [22, app ports]
              expect processes: language runtime, app
              expect outbound: API endpoints allowlist
```

Encode as a per-host profile; the same rule engine reads host role from config.

## What this rule set deliberately does not catch

- **Kernel rootkits** that modify `ss`/`ps` output before the agent reads it. Mitigation: network-level anomaly detection (enp counters via `/proc/net/dev` deltas that the rootkit would need to lie about too) and AIDE against `/bin`, `/sbin`.
- **Memory-only malware**. AIDE is file-based. Process anomalies help but not completely.
- **Sophisticated supply-chain compromise of packages**. Out of scope; falls to package signing, reproducible builds, SBOM work.
- **Application-layer attacks** (SQL injection, XSS, etc.). Not this layer. Needs WAF or app-level logging.

The point is to catch the 80% of compromises that look like script-kiddie miners, stolen-credential SSH logins, or forgotten-config drift. It will not catch a nation-state adversary, and if one is attacking your personal VPS, this rule set is not the weakest link in your life anyway.

## False-positive budget

Target after 2 weeks of tuning: **< 1 alert per host per week**.

Higher than that and you stop reading alerts. Lower and you are probably suppressing real signal. The tuning week is non-negotiable; ship with all rules firing, accept the noise, tune weekly until the budget is hit.

## How to spot when to deploy this

You run 1-10 Linux hosts, you are the only operator, you cannot justify Wazuh's complexity, and you have either (a) already been surprised by unexpected behavior on a host or (b) want to not be. The rule set is small enough to implement in a weekend, large enough to catch the common compromise patterns.

## Takeaway

Most of HIDS value for a single operator is in ~15 rules over signals that are all one-line shell commands away. The rest of Wazuh/OSSEC is multi-tenant, compliance-driven, and correlation-heavy, valuable at enterprise scale and dead weight for a solo shop.

## Related

- [[cloudflare-workers-as-monitoring-backend-for-self-hosted-linux]] - companion note; that one is the runtime shape, this one is the detection logic that runs inside the Worker
- [[saas-cto-security-checklist]] - institutional-scale equivalent of these monitoring-and-response items
- [[wireguard-static-ip-exchange-whitelist]] - source of the `wg show` signal consumed by the rule engine on tunnel hosts
