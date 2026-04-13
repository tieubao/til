# Log

Chronological record of content operations: ingests, queries, lints, synthesis. The LLM reads this at session start to understand recent wiki activity.

For project/structural decisions, see `_docs/changelog.md`.

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

## [2026-04-13] ingest | Batch ingest 10 notes from GitHub issues (engineering, cs, leadership)

Ingested issues #609, #592, #544, #540, #539, #538, #536, #534, #524, #512.

**New folder created:** `engineering/` (7 notes)
**Existing folders extended:** `cs/` (+2), `leadership/` (+1)

**Notes by folder:**
- `engineering/`: data-drives-code-structure, no-primitives-domain-modeling, antipattern-scripting-language, what-if-github-is-the-devil, purple-developer-10x-myth, programming-practices-principles, discipline-doesnt-scale
- `cs/`: why-linked-list-interview-questions, why-vim-uses-hjkl
- `leadership/`: consulting-secret-ask-the-ics

Wiki: 91 -> 101 notes. Synthesis page: engineering/ has 7 notes, eligible for synthesis.
