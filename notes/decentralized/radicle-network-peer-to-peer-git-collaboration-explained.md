---
title: "Radicle network: peer-to-peer git collaboration explained"
date: 2026-04-29
captured: 2026-04-29T02:14:11.293Z
tags: ["git", "decentralized", "p2p"]
source: "Claude.ai chat"
---
## Definition

Radicle is a peer-to-peer protocol for hosting and collaborating on Git repositories without GitHub, GitLab, or any central server. Repositories are replicated across peers in a decentralized network, with cryptographic identity, code reviews, and issues built into the protocol itself.

Think BitTorrent for Git, but with multi-sig governance and CRDT-based social artifacts.

## Why it exists

Centralized forges (GitHub, GitLab) create a single point of trust, control, and failure. If the platform censors, deplatforms, or simply goes down, the project dies with it. Self-hosted forges like Gitea or Forgejo solve sovereignty but fragment collaboration: each instance has its own user accounts, profiles, and identity. You lose network effects.

Radicle keeps sovereignty AND keeps a unified identity across the network, because identity is a key pair, not a username on someone's server.

![GitHub centralized vs Radicle peer-to-peer topology](https://assets.han-ws.workers.dev/i/2026/04/radicle-topology.svg)

The brilliance of the design: Radicle solves the trust problem by assigning stable identities to repositories that can be verified locally, allowing repositories to be served by untrusted parties. You don't need to trust the server because the repo signs itself.

## Architecture

Radicle is a layered protocol stack. Every node runs the same components.

![Radicle protocol stack](https://assets.han-ws.workers.dev/i/2026/04/radicle-stack.svg)

Three things to internalize from this:

**Everything is Git.** Issues, patches, code reviews, even the repo's identity document are stored as Git objects. All social artifacts are signed using public-key cryptography. Discussions and PRs work offline; push them when you reconnect.

**Collaborative Objects (COBs).** Issues, patches, and the repo identity are CRDTs (conflict-free replicated data types) encoded as a directed acyclic graph of Git commits. Two people can comment offline at the same time and the system converges to the same final state without a server arbitrating. This is the same primitive Linear, Figma, and Notion use, but applied to code review.

**Noise XK.** End-to-end encrypted transport between peers. Not blockchain, not Web3 (despite the marketing). Just modern cryptographic networking.

## Identity model

This is where Radicle is genuinely different from anything else.

![Radicle identity model](https://assets.han-ws.workers.dev/i/2026/04/radicle-identity.svg)

The core trick that makes Radicle work without a central server:

> Lacking a central location where repositories are hosted, the canonical branch is established dynamically based on the signature threshold defined in the repository's identity document. For example, if a threshold of two out of three delegates is set, with the default branch set to master, and two delegates have pushed the same commit to their master branches, that commit is recognized as the authoritative, canonical state of the repository.

In other words, "main is whatever a quorum of maintainers say it is." Multi-sig governance for code, baked directly into the protocol. No GitHub admin to revoke push access. No platform to censor a repo.

Every change is verified via the `refs/rad/sigrefs` ref, which holds cryptographic signatures over the entirety of a node's references. The repo is self-certifying: verification doesn't require any inputs other than the repository itself.

## Collaborative Objects (COBs)

The technically interesting innovation. COBs let Radicle store mutable social artifacts (issues, comments, patches) in immutable Git, while supporting concurrent edits without a central arbiter.

How it works:

- Each COB is a DAG of Git commits, disjoint from the source code branches
- Issues point to the identity document graph (so you can see who had write access at the time)
- Patches point to both the identity document and source code commits
- Edits don't conflict because the data structure is a CRDT - concurrent operations always converge to the same state when peers gossip their refs

This is the durable lesson from Radicle, even if the network itself stays niche: CRDTs in Git is a powerful pattern for any decentralized collaboration tool that wants to keep Git's content-addressed durability while adding mutable social state on top.

## Patch lifecycle

A patch (Radicle's name for a pull request) flows like this:

![Radicle patch lifecycle](https://assets.han-ws.workers.dev/i/2026/04/radicle-patch-flow.svg)

Notice what's missing: no merge button on a server. The commit becomes "merged" the moment a quorum of delegates have signed it. The protocol decides; no one operator does.

## Comparison

| Dimension | GitHub/GitLab | Self-hosted Gitea/Forgejo | Radicle |
|-----------|---------------|---------------------------|---------|
| Trust model | Trust the company | Trust the server operator | Trust math (signatures) |
| Censorship resistance | None | Server-level only | Built into protocol |
| Network effects | Massive | Fragmented per instance | Unified but tiny |
| Discoverability | Excellent | Poor across instances | Poor, no global index |
| CI/CD | First-class | First-class | Limited, build-it-yourself |
| Issues/PRs offline | No | No | Yes (COBs in Git) |
| Identity portability | Locked to platform | Locked per instance | Cryptographic, portable |
| Production maturity | 15+ years | 8+ years | 2 years (Heartwood gen) |
| Adoption | 100M+ users | Many large deployments | ~2000 repos, ~200 nodes |

Self-hosted forges provide more sovereignty but fragment collaboration. Radicle keeps sovereignty AND a unified identity across the entire network, because identity is a key pair rather than a username.

## Current state (April 2026)

**The good.** Version 1.6.0 shipped January 2026 with steady release cadence. HardenedBSD officially migrated its core repos (HardenedBSD-src, HardenedBSD-ports, HardenedBSD-pkg) to Radicle, validating real production use. The protocol spec is solid and well-documented. Active development team funded through Radworks.

**The reality check.** Adoption is still tiny: ~2000 repos and ~200 weekly active nodes as of late 2024. No CI/CD ecosystem, no notifications layer, no social graph, no `npm publish` integration. Large file support (git-annex, git-lfs) is still being considered. NAT traversal relies on seed nodes for now; hole-punching is in development.

**The Web3 baggage.** Radicle is funded by Radworks, which has an associated token (RAD). Early framing leaned heavily into Web3 narratives. The current Heartwood protocol is NOT on a blockchain, doesn't need a token to use, and works with plain Git plus Noise XK. The association still confuses people.

## When this matters

Radicle starts to make sense when:

- You're publishing critical infrastructure that could be censored (HardenedBSD's exact reason)
- You're a privacy-focused team that already runs your own everything
- You want offline-first code review (genuinely useful for low-connectivity contexts)
- You're building dev tools and want to support the alternative to GitHub on principle

It does NOT make sense for typical product teams whose bottleneck is CI velocity, code review throughput, or integration with the rest of their stack. GitHub gives all that for free.

## Key takeaway

The thing worth tracking is the **Collaborative Objects pattern**. CRDT-based social artifacts stored in Git is a durable architectural pattern. Even if Radicle itself stays niche, the COB approach is going to influence how decentralized collaboration tools get built for the next decade.

Radicle's specific innovation: making "what is canonical?" a function of cryptographic quorum rather than server access. That removes the central trust point entirely without sacrificing the ability to have an authoritative branch.

## Related

- [[double-spending]] - the same cryptographic-quorum primitive applied to value (Bitcoin) instead of code (Radicle)
- [[age-modern-file-encryption-cli]] - lightweight modern crypto in the same spirit (small surface, opinionated, X25519); both rely on key pairs as identity
- [[chezmoi-source-vs-target-two-layer-mental-model]] - dotfiles via plain Git; Radicle is what "plain Git plus identity plus social state" could look like end to end
- [[when-to-add-tailscale-to-a-personal-dev-surface]] - the proprietary-control-plane critique that applies to Tailscale also applies (in reverse) to GitHub; Radicle is what "all clients, no operator" looks like