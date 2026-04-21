# Log

Chronological record of content operations: ingests, queries, lints, synthesis. The LLM reads this at session start to understand recent wiki activity.

For project/structural decisions, see `_docs/changelog.md`.

---

## [2026-04-21] refactor | Move all content folders under `notes/`

Separated framework from content. Root now holds only the wiki operating system (CLAUDE.md, README.md, index.md, log.md, `_docs/`, `_templates/`, `_inbox/`); all domain folders and `assets/` live under `notes/`. Motivation: the underscore-prefixed framework folders already signaled "this is framework, not content" but content folders had no such signal, leaking the asymmetry. Makes the LLM-wiki pattern forkable as a standalone template.

**Scope:** 27 folders moved via `git mv` (26 domains + `assets/`), 273 notes relocated, 0 content changes. All 289 file moves staged as `R100` (pure renames), so `git log --follow` traces through cleanly.

**Path updates:**
- `index.md`: 270 markdown links rewritten to prefix every domain path with `notes/`
- `README.md`: 16 Recent-additions links + 26 Topics-table paths rewritten
- `CLAUDE.md`: rewrote the "Repo structure" section to describe framework-at-root + content-under-`notes/` split
- `_docs/architecture.md`: updated three-layer model + folder-convention sections
- `_docs/guide.md`: updated the ASCII tree, Obsidian attachments path (`notes/assets`), and dropped the stale per-folder note-count table in favor of a pointer to README Topics
- No changes to `_docs/requirements.md` or `_docs/changelog.md` content (changelog entry added separately)

**Invariants preserved (verified by test script):**
- 273 content notes before, 273 after
- 0 broken markdown links in root-level docs (README, CLAUDE, log, all `_docs/*.md`)
- 270/270 index.md paths resolve to existing files
- 547 wikilinks scanned; 0 broken (Obsidian resolves by filename, path-independent)
- `notes/assets/` kept together with notes so Obsidian vault-root image resolution still works

**Known non-issues surfaced by the scan:**
- Two folder-level `README.md` files (`notes/engineering/README.md`, `notes/finance-tooling/README.md`) share a basename. Not a wikilink target; no code writes `[[README]]`.
- Regex false-positive: `list[[ts_ms, o, h, l, c, v]]` inside a ccxt code-shape note matched the wikilink pattern. It's Python type-annotation syntax, not a link.

Commit strategy: single `refactor: move content folders under notes/` commit so the rename diff is reviewable in one place. Structural decision logged in `_docs/changelog.md`.

---

## [2026-04-21] ingest | Leadership in the agentic era + Tao Te Ching on timing

Two notes pushed via Claude.ai landed on `master` without compilation (commits `0647907`, `f24d681`). Compiled both in one pass, cross-linked them through the shared "substance before visibility" thesis.

**New notes:**
- `leadership/leadership-in-the-agentic-era.md` - Era framing (industrial -> knowledge -> platform -> agentic), the five-layer leadership leverage stack (strategy/taste/trust/culture/context), and the "great with agents, broken as a leader" anti-pattern
- `philosophy/tao-te-ching-i-ching-on-timing-and-hidden-preparation.md` - First note in new `philosophy/` folder. Six curated Tao Te Ching + I Ching passages around 潛龍勿用 (hidden dragon, do not act); key insight that wu wei is preparation during a red light, not idleness

**Compilation work:**
- Added `## Related` to the leadership note: cto-vs-vp-engineering, in-pursuit-of-excellence, managing-people-smarter-than-you, hr-evaluation-unique-value, nguyen-tac-truc-giac, dwarves-kit-design-philosophy-and-architecture, multi-agent-coding-brain-rot-scan-design, ai-tooling-stack-synthesis-april-2026, plus a cross-folder link to the Taoist note
- Rebuilt `## Related` on the Taoist note with wikilinks to simple-burnout-triage, munger-operating-system, be-dispassionate-about-software-careers, in-pursuit-of-excellence, the-three-gates-what-elders-screen-for, the-12-month-progression-deposit-to-partnership, dang-le-nguyen-vu-nhan-tinh-the-thai, and the leadership note. Moved Mitchell/I Ching citations into a separate `## Sources` section
- Stripped 7 em dashes from the Taoist note per repo rule (hard rule #1)
- Created new `## philosophy` section in `index.md` (between patterns and pkm); added leadership entry to `## leadership`
- Added both to `README.md` Recent additions (dropped the two oldest 2026-04-18 rows: Hermes briefing and TurboQuant); added `philosophy/` row to Topics table

**Contradictions / overlaps:** None found. Leadership note complements `nguyen-tac-truc-giac` (intuition theme) and `hr-evaluation-unique-value` (Differentiation x Influence = taste x trust) rather than contradicting. Taoist note is the first in its cluster; wisdom-adjacent notes currently live in `life/` and `wealth/`.

**Synthesis page:** Not yet. Leadership folder has 17 notes but spans multiple sub-clusters (agentic-era, consulting mechanics, PM/EM roles, Vietnamese business wisdom); sub-cluster synthesis is the right move, not a single leadership synthesis. Philosophy folder has 1 note; synthesis requires 4+.

---

## [2026-04-20] ingest | GeckoTerminal API evaluation

Fills the DEX OHLCV gap surfaced by a signals-engine integration where Binance-only klines silently drop coverage on Solana SPL and EVM DeFi tokens without a CEX pair. Evaluated six free providers and ten paid candidates; GeckoTerminal wins the free tier decisively with keyless auth + native H1/H4 + 30 rpm + 1000 bars/call. Same underlying DEX data as CoinGecko Pro Analyst ($129/mo), which makes the paid tier a common footgun.

**New note:**
- `finance-tooling/geckoterminal-evaluation.md` - 14/15 ADOPT verdict; category comparison table (6 free + 10 paid); specific call-out of Moralis Pro $49 as the paid upgrade for >6mo history; onboarding path; Reddit consensus summary citing DexScreener rate-limit pain + Freqtrade issue #10377 (no framework-native SPL OHLCV path)

**Compilation work:**
- Ran D-051 privacy checklist end-to-end before publishing; all 7 items PASS (no owner dollar amounts, no engine paths, no 1P URIs, no position data, no Dwarves detail, no real names, no owner-shape tells)
- Added `## Related` wikilinks to openbb-evaluation, fincept-terminal-evaluation, oss-trading-stack-survey, tool-evaluation-5-question-rubric
- Added to `index.md` under `## finance-tooling` (cluster now 6 notes)
- Added to `README.md` Recent additions

---

## [2026-04-19] ingest | Static outbound IP solutions, ten options compared

Companion landscape note to the WireGuard static-IP pattern. Where the earlier note answers "how to build the winning solution," this one walks through every competing option a semi-pro trader might consider and explains why each wins or loses. Useful as a decision-reference when someone pitches the trader a new proxy / VPN / PaaS "static IP" product.

**New note:**
- `finance-tooling/static-ip-solutions-compared-for-trading-bots.md` - Article-depth comparison of 10 options across 5 categories (VPS, consumer VPN, enterprise network, static proxy, PaaS / ISP); decision matrix + category-level scoring; meta-takeaway that B-E are all re-bundles of A with markup

**Compilation work:**
- Ran D-051 privacy checklist end-to-end before publishing; all 7 items PASS (same scan pattern as the WireGuard how-to; no dollar amounts tied to owner, no engine paths, no 1P URIs, no position data, no Dwarves detail, no real names, no owner-shape tells masquerading as generic)
- Added `## Related` wikilinks to the WireGuard how-to (category A winner) and to `oss-trading-stack-survey-april-2026` (where the bot that uses this IP gets built)
- Added to `index.md` under `## finance-tooling` (cluster now 5 notes)
- Added to `README.md` Recent additions
- Cross-reference expectation: the WireGuard how-to note's `## Related` section could be updated later to link back to this comparison; deferred until someone actually traverses that link

**External link:** same underlying trading-repo setup as the previous note; this eval stays generic. No trading-repo internals referenced.

---

## [2026-04-19] ingest | WireGuard static-IP pattern for exchange API whitelisting

Generic how-to note derived from a real trading-infra setup (Mac-local engine + Tokyo VPS egress tunnel). Bridges the gap for semi-pro traders stuck on the "my ISP rotates, my Binance key keeps breaking" problem with a concrete $5/mo pattern.

**New note:**
- `finance-tooling/wireguard-static-ip-exchange-whitelist.md` - Article-depth pattern note; compares VPN services, proxy services, Cloudflare, fly.io, and cheap VPS + WireGuard; concludes "static IP is never a product, always a feature of rented compute"

**Compilation work:**
- Ran D-051 privacy checklist end-to-end before publishing; all 7 items PASS (no dollar amounts tied to owner, no engine paths, no 1P URIs, no position data, no Dwarves detail, no real names, no owner-shape tells)
- Added `## Related` wikilinks to `oss-trading-stack-survey-april-2026` (engine context) and `crypto/double-spending` (why key hardening exists at all)
- Added to `index.md` under `## finance-tooling` (cluster now 4 notes)
- Added to `README.md` Recent additions

**External link:** private operational runbook for the actual owner setup lives in the `tieubao/trading` private repo at `operations/wg-egress-tunnel-runbook.md` and the design SPEC at `docs/specs/SPEC-012-wireguard-egress-tunnel.md`. Not linked from this public note.

---

## [2026-04-19] ingest | OpenBB Platform evaluation

Second finance-tooling tool evaluation, paired with a private memo in the trading repo. Python-first financial SDK covering equities, options, macro, and FRED via opt-in extensions; AGPL-3.0 on the Platform, closed commercial on the Workspace tier.

**New note:**
- `finance-tooling/openbb-evaluation.md` - OpenBB Platform SDK; 5-question rubric scored 11/15 = BOOKMARK; genuine TradFi research tool but thin value for pure-crypto shape

**Compilation work:**
- Applied 5-question rubric (Q1 2/3, Q2 2/3, Q3 3/3, Q4 3/3, Q5 1/3 = 11/15)
- Added head-to-head comparison table with FinceptTerminal (category peer, 10/15)
- Ran privacy checklist before publishing; all 7 items PASS
- Added `## Related` with wikilinks to tool-evaluation-5-question-rubric, fincept-terminal-evaluation, oss-trading-stack-survey-april-2026
- Added to `index.md` under `## finance-tooling` (cluster now 3 notes)
- Added to `README.md` Recent additions

**Contradictions flagged:** `oss-trading-stack-survey-april-2026` placed OpenBB in the agentic/AI category with a "read-only CLI only" verdict driven by AGPL concerns. This new eval reframes OpenBB as a research-SDK category (not agentic) and disentangles the AGPL trigger from commercial-license clauses that were separately a Fincept-specific issue. The OSS survey's categorization still holds; this eval adds nuance.

**External link:** private "applied to my engine" memo lives in `tieubao/trading` at `research/tools/openbb.md` (private repo; not linked here). Verdict there is `pilot-30d` scoped to research-only use outside the engine runtime.

---

## [2026-04-19] ingest | deepagents vs OpenClaw vs Hermes: category positioning (compile + synthesis refinement)

The note `ai-tooling/deepagents-vs-openclaw-vs-hermes-category-positioning.md` was added externally (commit 30b98bf) during the same session that produced the ai-tooling synthesis. Compile pass to weave it into the cluster.

**Compilation work:**
- Added `## Related` section to the deepagents note (it had none): cross-links to both Hermes notes, OpenClaw virtual-company pattern, the synthesis, the 8-layer stack, and the rubric
- Added backlinks from `hermes-agent-comprehensive-briefing-april-2026.md`, `hermes-vs-openclaw-competitive-scene-april-2026.md`, and `openclaw-virtual-company-pattern.md`
- Refined `ai-tooling-stack-synthesis-april-2026.md` cluster 4: reframed from "5 notes about runtime alternatives" to "6 notes split across L3 library and L3-L5 runtime, not all peers"; added a 6th key insight about category positioning being the most-skipped rubric question
- Updated `index.md` with the new note entry under ai-tooling
- Updated `README.md` Recent additions

**Contradiction worth flagging:** the deepagents note frames OpenClaw and Hermes as runtime peers in the same category, while `hermes-vs-openclaw-competitive-scene-april-2026.md` treats them as direct competitors. Both are correct at different abstraction levels: at the runtime layer they compete, but at the broader stack they share a layer that is distinct from library-level tools like deepagents. The synthesis now spells out both framings.

---

## [2026-04-19] refactor | split engineering/ into 6 sub-folders

`engineering/` had grown to 107 notes flat at the root. Split into 6 domain sub-folders to make the cluster navigable and to make orphans visible by domain so future repair passes can target one cluster at a time.

**New sub-folders:**
- `engineering/go/` (27 notes) - Go language: syntax, idioms, performance, error handling, generics, concurrency, comparisons with Elixir and Swift
- `engineering/functional/` (12) - FP concepts, Elixir/Elm specifics, anti-OOP critiques
- `engineering/architecture/` (14) - distributed systems, microservices, monorepos, perf, security, SRE, DevOps topologies, HTTP caching, CSS architecture
- `engineering/code-quality/` (18) - reviews, deletion, type systems, naming, docs, commit messages, PRs, conference proposals
- `engineering/careers/` (19) - career advice, seniority, problem-solving, ethics, ThoughtWorks-style lessons
- `engineering/principles/` (17) - general engineering principles, language opinions, history pieces, productivity rules of thumb

Total: 27+12+14+18+19+17 = 107 ✓ (no notes lost)

**Compilation work:**
- Used `git mv` so blame/history is preserved on every file
- Wrote `engineering/README.md` documenting the new taxonomy
- Updated `index.md`: replaced the flat engineering section with 6 sub-sections (one per sub-folder)
- README.md Topics table unchanged (the parent `engineering/` link still works)

**No backlink updates needed.** Obsidian wikilinks (`[[note-name]]`) resolve via shortest path, so `[[zen-of-go]]` continues to find the note at its new path. No grep-and-replace pass was required.

**Side effect:** `engineering/` is no longer the worst orphan cluster in the repo. Orphans are still there (40 of 107 had no incoming/outgoing links), but they're now distributed across sub-folders where future repair passes can target them by domain.

---

## [2026-04-19] synthesis | LLM agent memory synthesis April 2026

Wove 4 ai/ memory notes (landscape-2026, three-battlegrounds, harness-plugins, benchmarks-and-evaluation-crisis) into a single synthesis page. The 4-note cluster was already coherent; the synthesis spells out the vertical relationship (5-stage pipeline → 3 battlegrounds → harness lifecycle hooks → broken evaluation layer) that no individual note made explicit.

**New note:**
- `ai/llm-agent-memory-synthesis-april-2026.md` (type: synthesis) - architecture stack with light-theme ASCII diagram, agreements across the 4 notes, contradictions flagged, what the synthesis adds beyond individual notes

**Compilation work:**
- Cross-linked to `dwarves-kit-design-philosophy-and-architecture` (same hook architecture pattern, applied to spec enforcement instead of memory)
- Cross-linked to `claude-code-hook-lifecycle-and-event-system` (same before/after lifecycle pattern)
- Cross-linked to the parallel `ai-tooling-stack-synthesis-april-2026` (the rubric whose "kill question" framing the synthesis borrows)
- Cross-linked to `hermes-agent-comprehensive-briefing-april-2026` (Hermes's 3-layer memory is an applied example of the architecture)

**Contradictions flagged inside the synthesis:** the landscape note cites Mem0's LoCoMo score as 68.5%, while the benchmarks note flags this same number as untrustworthy. Not a real contradiction; a known limitation that the synthesis spells out so future readers don't trust the scores.

---

## [2026-04-19] synthesis | AI tooling stack synthesis April 2026

First synthesis page in `ai-tooling/`. Covers all 13 notes in the folder. Thesis: 3 layers (Claude Code workflow packs / context layer / agent stack alternatives) wired through one rubric (the 5-question evaluation framework), plus AutoResearch as a cross-cutting optimization pattern.

**New note:**
- `ai-tooling/ai-tooling-stack-synthesis-april-2026.md` (type: synthesis) - 4 sub-clusters with verdict tables, key contradictions flagged (especially the Hermes coordinated-promotion warning), 5 actionable insights, 4 open questions

**Compilation work:**
- Cross-linked to `oss-trading-stack-survey-april-2026`, `fincept-terminal-evaluation`, `openbb-evaluation` as cross-domain rubric applications (proves the rubric is portable, not domain-specific)
- Cross-linked to `dwarves-kit-v1-2-verification-pipeline-architecture` as a candidate target for the AutoResearch ratchet pattern

**Headline insight surfaced by synthesis (not visible in any individual note):** in April 2026, growth metrics and adoption-readiness move in opposite directions. The fastest-growing tool (Hermes) is the least battle-tested. The most stable tool (OpenClaw) is the most security-cratered. The most starred tool (gstack) is criticized as "just a bunch of prompts." Star count is roughly meaningless as a quality signal in this market.

---

## [2026-04-19] lint | full wiki health check + crypto/ orphan repair

Ran the 7-check lint pass across 251 content notes in 25 folders. Headline: capture pipeline is healthy (zero broken wikilinks, zero raw stragglers, zero missing `## Related` headings, zero stale notes), but ~88 legacy notes are true orphans (no incoming and no outgoing links) and ~121 have empty `## Related` sections. The orphan problem is concentrated in pre-2026 bulk-imported folders: `engineering/` (40 of 107), `life/` (16 of 25), `crypto/` (11 of 11), `leadership/` (10 of 16), `cs/` (5 of 14). Modern post-2026 capture folders (ai/, ai-tooling/, claude-code/, dwarves-kit/, hiring/, wealth/, mcp/, pkm/, diaspora/) have zero orphans, confirming the new pipeline is working.

**Crypto/ repair (this session):**
- Wove `## Related` sections across all 11 crypto notes into 3 sub-clusters:
  - Consensus/security: `asynchronous-byzantine-fault-tolerance`, `double-spending`, `runtime-verification-for-blockchain-security`, `stellar-vs-nano-comparison`
  - Bitcoin discourse: `bitcoin-investment-paradox`, `ray-dalio-on-bitcoin`, `stripe-on-bitcoin`, `cobie-on-33-and-crypto-incentives`
  - Tokenomics/DeFi: `token-emission-models`, `undercollateralized-loans-in-defi`, `ethereum-token-standards-and-security-tokens`
- Added cross-folder backlinks on 3 external notes: `investing/how-and-why-i-invest-in-startups`, `finance/how-the-bond-market-controls-housing-stocks-and-jobs`, `finance/financial-knowledge-as-compound-information-advantage`
- Crypto/ goes from 11 orphans to 0 orphans, 0 empty Related sections

**Thin-cluster synthesis gaps surfaced:**
- High-value (well-linked, ready): `ai-tooling/` (13), `ai/` (11), `dwarves-kit/` (9), `claude-code/` (5), `wealth/` (5)
- Repair-then-synthesize: `engineering/` (107 - too big, recommend split), `life/` (25), `leadership/` (16), `cs/` (14), `hiring/` (9), `history/` (6)

**Contradictions flagged:** none auto-detectable. Semantic contradiction check would need per-cluster reads.

**Open follow-ups (deferred for user input):**
1. Synthesis page drafts for `ai-tooling/` and `ai/` (project rule requires discussion before writing)
2. Sub-folder split of `engineering/` (107 notes is a category-smell; needs taxonomy decision)
3. Repair work on `life/`, `leadership/`, `cs/` orphan clusters

---

## [2026-04-19] synthesis | OSS trading stack survey, April 2026 (migrated from trading repo)

Sanitized a multi-tool trading-tooling survey originally in a private trading repo. Now a public synthesis note in `finance-tooling/`. Strips owner-specific SPEC / decision / experiment IDs, engine paths, account sizing, and "our repo" framing. Keeps the generic category framework (execution frameworks / agentic AI / infra libs), comparison tables, tool deep-dives, mermaid ecosystem + positioning + lifecycle diagrams, and recommendations applicable to any semi-pro crypto trader.

**New note:**
- `finance-tooling/oss-trading-stack-survey-april-2026.md` (type: synthesis) - 3-category survey with 4 mermaid diagrams; Freqtrade + VectorBT recommended; ai-hedge-fund as first agentic pilot

**Compilation work:**
- Ran through a strict privacy checklist before publishing (no dollar amounts, no engine paths, no SPEC/EXP IDs, no 1P URIs, no position data, no owner-shape tells); all 7 items PASS
- Added to `index.md` under `## finance-tooling` (between FinceptTerminal eval and the geopolitics section)
- Linked to existing synthesis note `ai-tooling/ai-dev-stack-8-layer-model-with-tool-evaluations-march-2026.md` as a parallel-shape peer in a different vertical
- Linked to `hermes-agent-comprehensive-briefing-april-2026.md` and `hermes-vs-openclaw-competitive-scene-april-2026.md` since Hermes appears as an LLM backend candidate for vendor pilots
- Tool evaluation rubric `tool-evaluation-5-question-rubric.md` cross-referenced since individual tool deep-dives follow that framing

**Contradictions flagged:** none. This is the first finance-tooling synthesis note; no overlapping claims in existing notes.

**Synthesis opportunity noted:** when `finance-tooling/` grows past 4 notes, consider a meta-synthesis connecting it to `ai-tooling/` (both are tool-evaluation domains following the same rubric; Venn overlap on agentic trading tools is substantial).

---

## [2026-04-19] ingest | FinceptTerminal evaluation (new finance-tooling folder)

First note in a new `finance-tooling/` folder, parallel to `ai-tooling/`. Scoped specifically to financial tool reviews (terminals, data providers, broker platforms, frameworks), distinct from `finance/` (concepts) and `investing/` (personal investing philosophy).

**New folder:**
- `finance-tooling/` with `README.md` explaining scope

**New note:**
- `finance-tooling/fincept-terminal-evaluation.md` - AGPL-3 Qt6 Bloomberg-alternative; 5-question rubric scored 10/15 = BOOKMARK; AGPL §13 blocks integration into any network-facing trading stack

**Compilation work:**
- Applied the `ai-tooling/tool-evaluation-5-question-rubric.md` scoring system to a finance tool (first cross-folder use of the rubric)
- Added `## Related` wikilinks to the rubric, to `ai-dev-stack-8-layer-model`, and to `how-the-bond-market-controls-housing-stocks-and-jobs` (macro research is the target use case)
- Added `## finance-tooling` section to `index.md` between `finance` and `geopolitics`
- Added `finance-tooling/` row to `README.md` Topics table
- Added this note to `README.md` Recent additions

**Contradictions flagged:** none. This is a new folder and a new tool; no prior claims to conflict with.

**Synthesis opportunity noted:** the 8-layer AI dev stack doesn't naturally accommodate financial tooling categories (research terminals, execution frameworks, data providers). If the cluster grows to 4+ finance-tooling notes, consider a `finance-tooling-stack-synthesis.md` analogous to the AI dev stack note.

**External link:** this evaluation is the public half of a pair. The private "applied to my engine" memo lives in `tieubao/trading` at `research/tools/fincept-terminal.md` (private repo; not linked here).

---

## [2026-04-19] ingest | How LLM agents do web research: the ReAct loop

One note pushed via Claude.ai between the previous compilation and this session.

**New note:**
- `ai/how-llm-agents-do-web-research-the-react-loop.md` - agent research as ReAct loop; biggest failure mode is under-weighting Reddit/HN/Twitter

**Compilation work:**
- Stripped 2 em dashes
- Added `## Related` with 5 wikilinks (multi-agent scan, claude dispatch, autoresearch, prompt improvement, memory landscape)
- Added back-reference from `ai-tooling/autoresearch-the-karpathy-loop-pattern.md` distinguishing ReAct (soft) vs Karpathy (hard) loops
- Updated `index.md` (ai/ 10 -> 11, header 258 -> 259)
- Updated `README.md` Recent additions and Topics table

**Contradictions flagged:** none. Complements existing multi-agent notes rather than contradicting them.

---

## [2026-04-19] ingest | Compile 10 notes pushed via Claude.ai (Hermes/OpenClaw cluster + LLM infra + finance + zed)

Notes were pushed direct to master between 2026-04-14 and 2026-04-18 without running the compilation step. Compiled them today.

**New notes (ai-tooling/, Hermes/OpenClaw cluster):**
- `ai-tooling/hermes-agent-comprehensive-briefing-april-2026.md` - Nous Research's self-hosted agent, auto-generated skills, growth trajectory
- `ai-tooling/hermes-vs-openclaw-competitive-scene-april-2026.md` - side-by-side metrics, verdict, source credibility filter
- `ai-tooling/why-developers-migrate-to-hermes-ranked-real-vs-hype.md` - 5 migration drivers ranked by substance vs narrative
- `ai-tooling/openclaw-virtual-company-pattern.md` - conceptual breakdown of the CEO/CTO/PM idiom and its 6 failure modes
- `ai-tooling/openclaw-multi-persona-dev-team-setup-playbook.md` - end-to-end JSON5 + SOUL/AGENTS/TOOLS playbook for a Telegram-led team

**New notes (ai/):**
- `ai/transformer-internals-for-software-engineers-ffn-as-graph-database-larql.md` - FFN reframed as a graph database of ~348K edges, LARQL tooling
- `ai/turboquant-kv-cache-compression.md` - ICLR 2026 KV cache quantization via random rotation + QJL correction

**New notes (finance/ - new folder):**
- `finance/financial-knowledge-as-compound-information-advantage.md` - Bille Finance narrative on information compounding like capital
- `finance/how-the-bond-market-controls-housing-stocks-and-jobs.md` - Vincent Chan: yield seesaw, ERP, refinancing trap

**New notes (zed/ - new folder):**
- `zed/zed-global-agent-rules-live-in-the-rules-library-not-agents-md.md` - Rules Library (LMDB) is the only global mechanism in Zed

**Compilation work:**
- Added `## Related` sections with wikilinks to all 10 new notes (none had proper Related sections)
- Stripped em dashes from `transformer-internals-...larql.md` (9) and `openclaw-multi-persona-...playbook.md` (16) per repo style rule
- Updated backlinks on: `ai/multi-agent-coding-brain-rot-scan-design-externalized-state-clean-handoffs.md`, `ai-tooling/ai-dev-stack-8-layer-model-with-tool-evaluations-march-2026.md`, `ai/claude-dispatch-workflows-and-async-ai-orchestration-from-mobile.md`, `investing/compound-interest-levels-and-lifestyle-progression.md`
- Added `finance/` and `zed/` sections to `index.md`; bumped ai/ to 10 and ai-tooling/ to 13
- Updated header count to 258 across 24 folders
- README.md refreshed: new Recent additions, Topics table, doc line

**Contradictions flagged:** none. The Hermes/OpenClaw cluster is internally consistent and new; no prior notes covered these tools.

**Synthesis page:** ai-tooling/ now has 13 notes, a mature cluster. Strong candidate for a synthesis page (layered dev-tool landscape, Hermes vs OpenClaw, evaluation rubric connective tissue). Suggested for next session.

---

## [2026-04-13] ingest | Batch ingest 8 notes from GitHub issues (label: hiring)

Triaged 8 issues (#545, #518, #361, #310, #304, #298, #229, #106). Created 8 notes, skipped 0.

**New notes (hiring/):**
- `hiring/developer-happiness-index.md` (#545) - Developer happiness survey data and retention factors
- `hiring/40-best-questions-to-ask-in-an-interview.md` (#518) - 40 high-signal interview questions by category
- `hiring/how-to-hire-programmers.md` (#361) - 4-step process for hiring programmers and outsourced devs
- `hiring/company-culture-is-who-you-hire-fire-promote.md` (#310) - Culture defined by hire/fire/promote decisions
- `hiring/facebook-hiring-strengths-builders-learners.md` (#304) - Facebook's three hiring factors
- `hiring/developers-guide-to-interviewing.md` (#298) - Developer's guide to evaluating employers
- `hiring/dont-hire-0x-engineers.md` (#229) - Against 10x engineer mythology
- `hiring/how-to-hire.md` (#106) - Six hiring principles

**Updated links on:** `hiring/assessing-software-engineering-candidates.md` (added 4 backlinks). All 8 new notes cross-linked to each other and existing note.

**Synthesis page:** hiring/ now has 9 notes. Synthesis page suggested.

---

## [2026-04-13] ingest | Batch ingest 6 notes from GitHub issues (label: architecture)

Triaged 10 issues (#464, #445, #291, #253, #250, #247, #245, #221, #111, #92). Created 6 notes, skipped 4.

**New notes (engineering/):**
- `engineering/creating-a-microservice-ten-questions.md` (#221) - 10-question operational checklist for new microservices
- `engineering/software-architecture-guide-fowler.md` (#445) - Martin Fowler's architecture guide overview
- `engineering/microservice-testing-strategies.md` (#253) - Five-layer test pyramid for microservices
- `engineering/css-architecture-first-steps.md` (#247) - BEM, SMACSS, ITCSS methodologies
- `engineering/apache-zookeeper-distributed-coordination.md` (#291) - ZooKeeper coordination primitives

**New notes (patterns/):**
- `patterns/backend-for-frontend-pattern.md` (#92) - BFF pattern for client-specific backends

**Skipped:**
- #464 (Computer Architecture course) - bare academic URL, no article content
- #111 (Design Patterns in Swift) - bare GitHub repo link, no article content
- #250 (Distributed Logging Architecture) - SSL certificate error, site migrated
- #245 (Optimistic Models / Spinners) - 404 after redirect, site dead

**Cross-links updated:** hidden-dividends-of-microservices, conways-law, history-of-hadoop. Synthesis page: none (engineering/ microservices sub-cluster growing but no synthesis yet).

---

## [2026-04-13] ingest | Batch ingest 8 notes from GitHub issues (label: history)

Triaged 22 issues (#591, #581, #578, #577, #564, #550, #477, #453, #436, #418, #396, #380, #347, #213, #197, #170, #169, #168, #166, #156, #141, #47). Created 8 notes, skipped 14.

**New notes (ai/):**
- `ai/grand-unified-theory-of-ai-hype-cycle.md` (#591) - AI hype follows a repeating 13-step cycle

**New notes (cs/):**
- `cs/history-of-regular-expressions.md` (#564) - From neuroscience to UNIX tooling
- `cs/the-next-century-of-computing.md` (#578) - Post-Moore's Law predictions
- `cs/whats-next-in-computing.md` (#168) - Chris Dixon's computing eras framework
- `cs/brief-totally-accurate-history-of-programming-languages.md` (#347) - Satirical PL timeline
- `cs/history-of-software-resources.md` (#436) - Curated link collection

**New notes (engineering/):**
- `engineering/history-of-hadoop.md` (#166) - From Lucene to distributed computing

**New notes (history/):**
- `history/israel-palestine-va-jerusalem.md` (#550) - 3000 years of Middle East conflict (Vietnamese)

**Skipped:** #581 (paywall), #577 (PDF only), #477 (SVG link), #453 (old academic URL+PDF), #418 (video link), #396 (Reddit, unfetchable), #380 (YouTube), #213 (dead blog), #197 (dead tumblr), #170 (dead Facebook), #169 (old Vietnamese site), #156 (YouTube), #141 (PDF only), #47 (bare URL)

Updated cross-links: `history-of-regular-expressions` <-> `grand-unified-theory-of-ai-hype-cycle`, `the-next-century-of-computing` <-> `whats-next-in-computing`. Synthesis page: none (cs/ has 14 notes but spans multiple domains).

---

## [2026-04-13] ingest | Batch ingest 14 notes from GitHub issues (label: management)

Triaged 19 issues (#598, #589, #545, #484, #443, #435, #433, #416, #402, #373, #358, #349, #346, #300, #243, #154, #76, #67, #51). Created 14 notes, skipped 5.

**New notes (leadership/):**
- `leadership/note-to-new-design-managers.md` (#598) - Hardik Pandya's guide for new design managers
- `leadership/why-you-need-engineering-managers.md` (#589) - Charity Majors on why EMs are necessary
- `leadership/a-decade-of-remote-work.md` (#433) - Viktor Petersson's remote work lessons
- `leadership/rise-of-the-interim-cto.md` (#402) - When startups need a temporary CTO
- `leadership/how-to-charge-clients.md` (#358) - Paul Boag's honest pricing method
- `leadership/nguyen-tac-truc-giac.md` (#349) - Nguyên tắc trực giác trong lãnh đạo (Vietnamese)
- `leadership/managing-people-smarter-than-you.md` (#76) - HBR advice on managing smarter reports

**New notes (engineering/):**
- `engineering/conways-law.md` (#416) - Org structure constrains system design
- `engineering/devops-team-topologies.md` (#346) - Matthew Skelton's DevOps team framework
- `engineering/agile-documentation-best-practices.md` (#243) - Scott Ambler's agile doc practices
- `engineering/heisenberg-developers.md` (#154) - Measuring developers changes their behavior

**New notes (startup/):**
- `startup/tap-trung-vao-san-pham.md` (#443) - Focus on fixing product first (Vietnamese)
- `startup/anatomy-of-software-frauds.md` (#484) - Three-layer architecture of tech fraud
- `startup/tesla-gm-founders-vs-managers.md` (#373) - Founders vs professional managers pattern

**New folder:** `startup/` created for startup-specific content.

**Skipped:**
- #545 (Developer Happiness Index) - cult.honeypot.io DNS dead
- #435 (Doctrine Patterns) - wardleypedia.org TLS connection failed
- #300 (Top 10 leadership competencies) - image only, no text content
- #67 (H-1B Visa Program) - NYTimes paywall, WebFetch blocked
- #51 (How to legally own another person) - Dropbox link dead

**Cross-links added:** devops-team-topologies -> conways-law, rise-of-the-interim-cto -> cto-vs-vp-engineering, managing-people-smarter-than-you -> tips-on-working-with-talents.

Synthesis page: none. Leadership cluster now has 15 notes; synthesis recommended.

---

## [2026-04-13] ingest | Batch ingest 10 notes from GitHub issues (label: golang)

Ingested issues #470, #429, #398, #397, #377, #353, #320, #312, #303, #287 via WebFetch. All placed in `engineering/`.

**New notes:**
- `engineering/million-websockets-and-go.md` - Mail.Ru's optimization journey for 3M concurrent WebSocket connections
- `engineering/go-testing-principles-dave-cheney.md` - Dave Cheney's GopherChina 2019 testing talk principles
- `engineering/go-type-system-closer-look.md` - named vs unnamed types, underlying types, assignability rules
- `engineering/go2-error-handling-draft-design.md` - check/handle proposal (not accepted), Go error handling evolution
- `engineering/go-concurrency-through-illustrations.md` - visual intro to goroutines, channels, select
- `engineering/building-worker-pool-in-go.md` - bounded concurrency pattern with job queue and dispatcher
- `engineering/typed-nils-in-go.md` - interface nil gotcha when concrete nil is stored in interface
- `engineering/four-days-of-go.md` - newcomer critique of Go's strictness vs flexibility trade-off
- `engineering/go-vs-swift-comparison.md` - side-by-side language comparison (typing, concurrency, paradigm)
- `engineering/comparing-elixir-and-go.md` - concurrency models, fault tolerance, when to choose which

**Skipped:** #353 URL dead (geeks.uniplaces.com DNS gone). Note written from known article content.
**Partial:** #429 PDF not parseable via WebFetch; note written from talk title and known Dave Cheney testing principles. #303 GitHub PDF page; note written from known comparison content.

Updated cross-links on all 10 new notes to existing notes: understanding-nil-in-go, between-golang-and-elixir, channels-in-golang, error-handling-in-upspin, effective-error-handling-in-go, good-and-bad-elixir, swifty-code, elixir-concepts-for-go-developers, zen-of-go, go-proverbs, go-best-practices-six-years-in, debating-type-systems, swift-pattern-matching-case-let. Synthesis page: none (engineering/ Go cluster now has 15+ notes; sub-cluster synthesis recommended).

---

## [2026-04-13] ingest | Batch ingest 5 notes from GitHub issues (label: better dev)

Ingested issues #616, #615, #614, #556, #552 via WebFetch. All placed in `engineering/`.

**New notes:**
- `engineering/egoless-engineering.md` - ego and parochialism destroy engineering orgs
- `engineering/choose-boring-technology.md` - innovation tokens and boring tech advocacy
- `engineering/why-big-tech-is-slow.md` - feature interaction complexity explains slowness
- `engineering/good-and-bad-elixir.md` - Elixir anti-patterns and positive practices
- `engineering/bit-twiddling-hacks.md` - Stanford bitwise manipulation reference

Updated links on: effective-code-reviews, discipline-doesnt-scale, mastering-programming, lessons-learned-in-software-dev, data-drives-code-structure, hidden-dividends-of-microservices, monorepo-advantages, code-for-readability, programming-practices-principles, zen-of-python. Synthesis page: none (engineering/ has 40 notes but no synthesis yet; suggest creating one for the "better dev" cluster).

---

## [2026-04-13] refactor | Zettelkasten migration

Initial migration from flat TIL collection to Zettelkasten wiki. 60 notes updated with frontmatter fields (`aliases`, `status`). 205 wikilinks added across all notes. `predictive-history/` merged into `history/`.

## [2026-04-13] ingest | LLM Wiki pattern: compilation over retrieval

Added to `pkm/`. Captures Karpathy's LLM Wiki pattern, four key insights, scaling limits, and how it applies to our wiki. Linked to: why-knowledge-notes-need-context, llm-memory-systems-three-competitive-battlegrounds, llm-agent-memory-systems-landscape-2026, memory-systems-as-agent-harness-plugins. Synthesis page: none (pkm/ has only 2 notes).

## [2026-04-13] ingest | Batch ingest 32 notes from GitHub issues (label: life)

Triaged all 50 GitHub issues with `life` label. Result: 20 body-ingest, 11 url-ingest (WebFetch), 11 youtube-skip, 8 skip. Created 32 new notes total.

**New folders created:** `life/` (24 notes), `leadership/` (6 notes)
**Existing folders extended:** `health/` (+1), `investing/` (+1)

**Notes by folder:**
- `life/`: always-be-quitting, average-joe, be-dispassionate-about-software-careers, chon-nguoi-hop-tac-va-ket-giao, dang-le-nguyen-vu-nhan-tinh-the-thai, great-minds-discuss-ideas, hygge-danish-concept-of-cosiness, john-vu-on-world-class-quality, laziness-does-not-exist, learning-to-say-no, munger-operating-system, navagraha-nine-celestial-bodies, pavel-durov-secrets-for-success, simple-burnout-triage, to-chat-lanh-dao-kinh-doanh, vipassana-for-hackers, we-used-to-just-live, what-it-feels-like-to-become-poor, when-and-how-to-ask-for-help, why-explore-space-stuhlinger-letter, why-we-lie-about-being-retired, working-attitude-principles, time-is-the-only-real-currency, 100-little-ideas
- `leadership/`: steve-jobs-negotiation-tactics, tips-on-working-with-talents, lam-an-kieu-cu-ho, masayoshi-son-softbank-vision, hr-evaluation-unique-value, in-pursuit-of-excellence
- `health/`: vitamins-and-longevity-stack
- `investing/`: how-and-why-i-invest-in-startups

**Issues skipped (19):** #606, #604, #600, #588, #562, #561, #505 youtube, #501, #499, #495 fetched, #475, #468, #454, #448 fetched, #446, #444, #439, #426, #410, #401, #368, #366, #364

Wiki: 59 -> 91 notes. Synthesis page: life/ has 24 notes but topics are diverse (career, philosophy, health, spirituality, finance); sub-cluster synthesis recommended rather than one page.

## [2026-04-13] synthesis | Vietnamese diaspora: from subsistence to bridge-building

First synthesis page in the wiki. Synthesizes all 7 diaspora notes into a layered argument: structural diagnosis -> hollowing mechanism -> trajectory projections -> bridge-builder prescription. Identified one contradiction (urgency window: 10-15 vs 15-20 years) and one major gap (community-level institutional prescription is underspecified). Cross-linked to wealth/, history/ notes.

## [2026-04-13] refactor | Upgrade README.md index with one-line summaries

README.md now has a one-line summary for every note (Karpathy index.md pattern). LLM reads the index first to find relevant pages without opening files. Also added Query operation to CLAUDE.md schema: search wiki, synthesize answer, offer to file back as page.

## [2026-04-13] refactor | Dedup 3 note pairs

Deleted `ai-tooling/ai-dev-stack-8-layer-model-march-2026.md` (strict subset of expanded version). Deleted `diaspora/four-asian-diasporas-30-year-projection.md` (90% overlap with 2055 trajectories note). Merged `diaspora/vietnamese-vs-chinese-diaspora-why-one-builds-economic-hubs-and-the-other-doesnt.md` into the structural analysis note (added escape-through-education and bridge-builder sections). Updated all backlinks across 10 files. Wiki now at 59 notes.

## [2026-04-13] lint | First wiki health check

Results: 1 orphan (health/alkaline-water, acceptable), 0 broken links in content, 0 raw stragglers, 0 missing Related sections, 0 stale notes. 6 clusters eligible for synthesis pages (dwarves-kit, ai-tooling, ai, claude-code, history, wealth). 1 known contradiction (diaspora urgency window 10-15 vs 15-20 years). Overall health: good.

## [2026-04-13] ingest | Batch ingest 77 notes from GitHub issues (better dev label)

Triaged 100 GitHub issues with `better dev` label. Result: 44 body-ingest, 31 url-ingest (WebFetch), 2 youtube-skip, 12 skip, 5 already ingested, 7 URLs dead (404/DNS gone).

**New folders created:** `engineering/` (64 notes), `hiring/` (1 note)
**Existing folders extended:** `cs/` (+5), `leadership/` (+3), `life/` (+1)

**Dead URLs (skipped):** #285 (pragprog 404), #284 (framer blog removed), #272 (SSL error), #263 (SE blocked), #212 (rosettacode 403), #206 (Google Docs JS), #200 (DNS gone)

Wiki: 91 -> 168 notes. Synthesis page: engineering/ now has 64 notes, sub-cluster synthesis recommended (code quality, career growth, language philosophy, system design).
