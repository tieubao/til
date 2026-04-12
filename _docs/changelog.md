# Log

Append-only record of wiki operations. Each entry: `## [date] operation | description`

---

## [2026-04-13] refactor | Zettelkasten migration

Migrated the TIL repo from a flat note collection to a Zettelkasten-style LLM wiki. This was a foundational restructure session.

### Decisions made

1. **Methodology: Zettelkasten + LLM Wiki hybrid**
   - Adopted Zettelkasten linking and note maturity model
   - Influenced by Karpathy's LLM Wiki pattern (April 2026 gist) but adapted for personal use
   - Key difference from pure LLM Wiki: the human stays in the loop for synthesis (writing is load-bearing for learning)

2. **Folder structure: keep as-is, topic-based**
   - Existing topic folders are healthy, no restructure needed
   - `ai/` vs `ai-tooling/` split kept (concepts vs tool evaluations)
   - Only change: merged `predictive-history/` (1 note) into `history/`
   - New folders: `_inbox/` (raw captures), `_templates/` (Obsidian templates)

3. **Wikilinks: Obsidian-compatible `[[filename]]` style**
   - No folder path in links (Obsidian resolves shortest path)
   - Every refined note gets a `## Related` section at the bottom
   - Links include a brief phrase explaining the relationship

4. **Note lifecycle: raw -> refined -> evergreen**
   - `status` field in frontmatter tracks maturity
   - `raw`: just captured, in `_inbox/`, unprocessed
   - `refined`: has frontmatter, correct folder, has links
   - `evergreen`: mature, well-linked, updated over time

5. **Frontmatter additions**
   - Added `aliases: []` (for Obsidian link resolution after renames)
   - Added `status: refined` (all existing notes start as refined)

6. **Three ingest paths**
   - Obsidian Clipper -> `_inbox/` (raw, needs processing)
   - Claude Code -> direct to topic folder (already refined)
   - Claude AI skill (knowledge-capture) -> direct to topic folder via GitHub MCP Worker

7. **What NOT to build (decided against)**
   - No note IDs (filenames serve as IDs, aliases handle renames)
   - No separate raw sources layer (inbox -> refined pipeline is sufficient)
   - No search tooling like qmd (at 60 notes, grep + index is enough)
   - No confidence scoring or supersession tracking (over-engineering)
   - No `.obsidian/` config in git (personal tooling, gitignored)

8. **Obsidian plugin recommendations**
   - Linter (frontmatter enforcement)
   - Obsidian Clipper (web capture to `_inbox/`)
   - Dataview (query notes by frontmatter fields)
   - Graph Analysis (orphan detection, cluster visualization)
   - Consistent Attachments and Links (fix broken links on moves)

### What was changed

| Change | Files affected |
|--------|---------------|
| CLAUDE.md rewritten | 1 file |
| `_inbox/` created with README | 1 file |
| `_templates/` created (atomic-note, til, article, definition) | 4 files |
| `.gitignore` created | 1 file |
| Frontmatter updated (aliases + status) | 60 notes |
| `## Related` sections added with wikilinks | 60 notes, ~205 links total |
| `predictive-history/` merged into `history/` | 1 note moved, 1 folder removed |
| `log.md` created | 1 file |
| `GUIDE.md` created | 1 file |

### Planned next steps (not yet done)

- Add synthesis page type (MOC-like pages that synthesize across note clusters)
- Create 2-3 synthesis pages for densest clusters (diaspora, AI memory, wealth)
- Build `/lint-wiki` command (orphan detection, broken links, stale notes)
- Capture the Karpathy LLM Wiki analysis as a note in `pkm/`

## [2026-04-13] refactor | Move project docs to _docs/

Moved project documentation into `_docs/` folder (underscore prefix = infrastructure, not content). Root stays clean with only `CLAUDE.md` and `README.md`.

### What moved

| Before | After |
|--------|-------|
| `GUIDE.md` (root) | `_docs/guide.md` |
| `log.md` (root) | `_docs/changelog.md` |
| (new) | `_docs/requirements.md` |
| (new) | `_docs/architecture.md` |

Updated references in `CLAUDE.md` and `_docs/guide.md` to point to new locations.

## [2026-04-13] decision | Separate content log from project changelog

Realized Karpathy's `log.md` and our changelog serve different purposes:

- `log.md` (root): content operations (ingests, queries, lints). Read by the LLM at session start. Updated frequently.
- `_docs/changelog.md` (this file): project structure decisions. Read by humans for context. Updated rarely.

Restored `log.md` at root for content operations. Kept this file for project decisions.

## [2026-04-13] decision | Add compilation step to ingest workflow

Added a "compilation step" to CLAUDE.md that runs on every note addition. The step requires Claude to: check for overlapping claims in existing notes, flag contradictions with callouts, update synthesis pages if they exist, and append to log.md. This is the core difference between a filing system and a knowledge base. Influenced by Karpathy's LLM Wiki pattern.
