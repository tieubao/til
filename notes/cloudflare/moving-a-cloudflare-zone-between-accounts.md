---
title: "Moving a Cloudflare zone between accounts: what is account-bound and the recon order"
date: 2026-09-05
captured: 2026-09-05T08:19:00Z
tags: [cloudflare, dns, architecture, migration]
source: "Claude Code session"
status: refined
---

## Overview

A Cloudflare zone looks like one movable object: export the DNS records, create the zone in
the destination account, flip nameservers, done. Two resource kinds break that model,
because they bind to the ACCOUNT that holds them, not to the zone. Moving a zone that
carries either one cleanly requires finding and rehoming them first, in a fixed order,
before the zone itself moves.

## Components

| Resource | Binds to | Moves with a plain DNS export? |
|---|---|---|
| A/AAAA/CNAME to a non-tunnel target, MX, TXT, page rules, redirect rules | the zone | yes |
| Workers custom domain | the account | no, must be detached and reattached in the destination account |
| Tunnel DNS record (a CNAME whose target ends in `cfargotunnel.com`) | the account | no, the tunnel itself lives in the account |

Everything in the first row is a snapshot-and-replay problem. Everything in the other two
rows is a re-homing problem: the Worker or the tunnel has to exist and serve traffic in the
destination account before the record can point at it there.

## Recon order

Before moving any zone between accounts, run this in order and stop at the first hit:

1. **Registrar.** If the domain is registered through Cloudflare Registrar under the source
   account, it cannot move accounts on its own. A Registrar domain can only use the
   nameservers of the account that holds the registration, so the registration itself has
   to transfer first. A zone on a domain registered elsewhere (a third-party registrar like
   Namecheap) has no such block; only its nameservers need to change.
2. **Workers custom domains on the zone** (`GET /accounts/<account>/workers/domains?zone_id=<zone>`).
3. **Workers routes** on the zone.
4. **DNS records whose content ends in `cfargotunnel.com`** (Cloudflare Tunnel targets).
5. **Pages custom domains**, **Access apps**, **page rules**, **DNSSEC** on the zone.

Zero hits across steps 2 through 4 means the zone moves in an afternoon: export records,
create the zone in the destination account, recreate the records, flip nameservers at the
registrar. A hit at any step means that Worker or tunnel must be re-homed to the destination
account, and made to serve traffic there, before the zone itself can move.

## Cut mechanics

Re-homing a Workers custom domain hits three sharp edges:

- **Attach refuses over an existing record.** `PUT /accounts/<account>/workers/domains`
  returns error code `100117` if a DNS record already exists for that hostname. Record the
  existing record to a file, delete it, then attach.
- **The binding then owns the record.** Once a hostname is attached as a Workers custom
  domain, `PUT`/`DELETE` on its DNS record return code `1043`, "configured as read only."
  Repointing the record is not a rollback path. Rollback is detach-first: delete the custom
  domain binding, wait for the record to clear, then recreate the record you saved before
  attaching.
- **A hostname two labels deep needs its own certificate path.** Universal SSL on a Free
  zone covers only the apex and its single-level wildcard (`example.com` and
  `*.example.com`). A hostname one label deeper than that (`api.sub.example.com`) fails the
  TLS handshake with alert 40, no matter what DNS record or redirect rule sits behind it.
  Advanced Certificate Manager, or serving it via a same-account Workers custom domain, is
  the only way to front it.

The safe re-homing order is additive-then-remove either way: attach the new hostname in the
destination account, prove it serves a real response, then detach the old hostname, then
retire the old Worker or tunnel. Deleting the old side before the new side is proven live
breaks traffic while it is still routing.

## Key decisions

**Publication lag is real and asymmetric.** Flipping nameservers at the registrar updates
WHOIS almost immediately, often within a minute. Publication into the TLD's own root
servers is a separate, slower process, observed anywhere from under an hour to roughly
eight hours depending on the TLD. A fast WHOIS read is not proof the move is live
everywhere; treat the zone as still moving until the TLD's own servers resolve it.

**Universal SSL's one-label limit decides what can be fronted by the new zone at all.** Any
hostname deeper than `*.apex` needs a paid certificate product or a same-account Worker
before it can be re-created behind the destination zone, independent of whether the DNS
record itself is trivial to copy.

## Related

The reusable checklist: registrar, then Workers custom domains, then Workers routes, then
tunnel CNAMEs, then Pages/Access/page-rules/DNSSEC. Zero hits on the account-bound checks
means an afternoon's work; any hit means a re-home first.
