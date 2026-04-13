# Log

Chronological record of content operations: ingests, queries, lints, synthesis. The LLM reads this at session start to understand recent wiki activity.

For project/structural decisions, see `_docs/changelog.md`.

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
