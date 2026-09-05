---
title: "Pushing Workers logs (OTel export or Logpush) vs polling the telemetry API for error alerting"
date: 2026-09-05
captured: 2026-09-05T08:40:00Z
tags: [cloudflare, workers, observability, logpush, opentelemetry, alerting]
source: "Claude Code session"
aliases: [Workers Logs Destinations beta, workers_trace_events logpush filter, cloudflare telemetry query API polling]
status: refined
---

Cloudflare has two mechanisms that both answer to "push my Workers logs somewhere", and they are easy to conflate. This compares them against the third option, polling the Workers Observability telemetry query API, for one job: raising an alert when a Worker logs at ERROR level.

## The three options

| | OTel export (dashboard name: "Destinations", beta) | Logpush, dataset `workers_trace_events` (GA) | Poll the telemetry query API |
|---|---|---|---|
| What it is | Workers push OTLP-shaped logs and traces to any OTLP HTTP endpoint, configured per Worker in `wrangler.jsonc` (`observability.logs.destinations`) plus a dashboard-created destination | The older Logpush job pushing batched Trace Event records to a destination, created via `POST /accounts/{id}/logpush/jobs` | Your own probe calls `POST /accounts/{id}/workers/observability/telemetry/query` on a schedule |
| Filter before send | None documented; only `head_sampling_rate` | Yes, a `filter` JSON on Logpush fields, e.g. `{"where":{"key":"Outcome","operator":"eq","value":"exception"}}` | Yes, the query's `calculations` view groups and filters on `$metadata.level` and `$metadata.service` |
| Payload | OTLP logs data model | Logpush's own batched JSON record schema | Whatever your probe extracts |
| Auth to a custom HTTP destination | Custom headers set in the dashboard | `header_*` query parameters on the destination URL | Whatever your probe already uses |
| One-time setup | Dashboard destination plus per-Worker wiring | Ownership challenge: Cloudflare POSTs a token to the destination, which must serve it back before the job is created | None beyond the probe |
| Latency | "A few minutes", specifics undocumented | Batched, historically minutes | Your tick interval; a 5-minute tick with a 15-minute lookback covers ingest lag |
| Plan and cost | Workers Paid; 10M events a month included, then $0.05 per million; free until 2026-10-01 | Workers Paid | One query per tick, plus whatever sample lookups you add |
| Failure mode | Silent: no level filter means every log ships or nothing does, and a broken destination just stops receiving with no signal on your side | The challenge blocks job creation if the destination cannot serve the token; a bad filter silently ships nothing | Your probe is your signal; a stuck probe is a missed heartbeat |
| Token permissions needed | Workers Observability write on the account | Logpush edit, plus Logpush read to list jobs and dataset fields | Workers Observability read |

## Verdict

For "alert me on ERROR-level events from these Workers", polling wins today, for three reasons that do not depend on each other.

1. Only Logpush can filter by level before sending, and it adds the ownership-challenge handshake; the feature literally named "Destinations" cannot filter at all, so it would ship every level to your receiver.
2. Neither push path speaks your alert format. Both require a new receiver that parses OTLP or the Logpush record schema and translates it into whatever your alerting takes. Polling already produces that shape.
3. Push needs its own token scopes (Logpush read and edit, or Observability write). An account token minted for other work will 403 on both `GET /accounts/{id}/logpush/jobs` and the telemetry query endpoint while still passing on unrelated endpoints, which is easy to misread as a dead token.

Revisit push when OTel export documents a severity filter, or when you hold a token with Logpush read and `workers_trace_events` fields confirm a level or outcome field usable in a `filter`. The latency win (minutes instead of a 5 to 15 minute window) is real but rarely what an error-alert flow needs.
