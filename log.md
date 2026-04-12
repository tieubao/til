# Log

Chronological record of content operations: ingests, queries, lints, synthesis. The LLM reads this at session start to understand recent wiki activity.

For project/structural decisions, see `_docs/changelog.md`.

---

## [2026-04-13] refactor | Zettelkasten migration

Initial migration from flat TIL collection to Zettelkasten wiki. 60 notes updated with frontmatter fields (`aliases`, `status`). 205 wikilinks added across all notes. `predictive-history/` merged into `history/`.

## [2026-04-13] ingest | LLM Wiki pattern: compilation over retrieval

Added to `pkm/`. Captures Karpathy's LLM Wiki pattern, four key insights, scaling limits, and how it applies to our wiki. Linked to: why-knowledge-notes-need-context, llm-memory-systems-three-competitive-battlegrounds, llm-agent-memory-systems-landscape-2026, memory-systems-as-agent-harness-plugins. Synthesis page: none (pkm/ has only 2 notes).

## [2026-04-13] synthesis | Vietnamese diaspora: from subsistence to bridge-building

First synthesis page in the wiki. Synthesizes all 7 diaspora notes into a layered argument: structural diagnosis -> hollowing mechanism -> trajectory projections -> bridge-builder prescription. Identified one contradiction (urgency window: 10-15 vs 15-20 years) and one major gap (community-level institutional prescription is underspecified). Cross-linked to wealth/, history/ notes.

## [2026-04-13] refactor | Upgrade README.md index with one-line summaries

README.md now has a one-line summary for every note (Karpathy index.md pattern). LLM reads the index first to find relevant pages without opening files. Also added Query operation to CLAUDE.md schema: search wiki, synthesize answer, offer to file back as page.

## [2026-04-13] refactor | Dedup 3 note pairs

Deleted `ai-tooling/ai-dev-stack-8-layer-model-march-2026.md` (strict subset of expanded version). Deleted `diaspora/four-asian-diasporas-30-year-projection.md` (90% overlap with 2055 trajectories note). Merged `diaspora/vietnamese-vs-chinese-diaspora-why-one-builds-economic-hubs-and-the-other-doesnt.md` into the structural analysis note (added escape-through-education and bridge-builder sections). Updated all backlinks across 10 files. Wiki now at 59 notes.

## [2026-04-13] lint | First wiki health check

Results: 1 orphan (health/alkaline-water, acceptable), 0 broken links in content, 0 raw stragglers, 0 missing Related sections, 0 stale notes. 6 clusters eligible for synthesis pages (dwarves-kit, ai-tooling, ai, claude-code, history, wealth). 1 known contradiction (diaspora urgency window 10-15 vs 15-20 years). Overall health: good.
