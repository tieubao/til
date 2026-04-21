# Log

Append-only record of wiki operations. Each entry: `## [date] operation | description`

---

## [2026-04-21] refactor | Framework/content separation: move domain folders under `notes/`

**Decision:** Repo root now holds framework only (CLAUDE.md, README.md, index.md, log.md, `_docs/`, `_templates/`, `_inbox/`). All 26 domain folders and `assets/` moved under `notes/`.

**Rationale:**
1. **Asymmetry fixed.** Underscore-prefixed folders (`_inbox/`, `_templates/`, `_docs/`) already signaled "framework, not content." Flat domain folders at root had no equivalent signal, so a newcomer reading `ls` saw 25+ folders with no way to tell the wiki's operating system apart from the wiki's contents. Moving domains under `notes/` makes the split explicit.
2. **Forkable template.** The LLM-wiki pattern (per `notes/pkm/llm-wiki-pattern-compilation-over-retrieval.md`) is the kind of thing other people will want to copy. Separating framework from content means someone can clone this repo, delete `notes/`, and have a working wiki-framework template with zero cleanup.
3. **The "more files at top level looks impressive" instinct was vanity.** Acknowledged by the user and overruled by the clarity argument.

**Cost:** one-time mechanical refactor. 289 files renamed (all `R100`), 270 links rewritten in index.md, 16+26 links rewritten in README.md, framework docs updated.

**Invariants preserved:**
- `[[wikilinks]]` unaffected because Obsidian resolves by filename, not path
- Git history preserved (all moves are pure renames)
- Note count unchanged (273 before, 273 after)
- `notes/assets/` stayed co-located with notes so Obsidian vault-root image resolution continues to work

**What did NOT move:**
- `_inbox/`, `_templates/`, `_docs/` stayed at root (they're framework, not content)
- Root markdown files (CLAUDE.md, README.md, index.md, log.md) stayed at root

**Stale artifacts removed:**
- Dropped the per-folder note-count table from `_docs/guide.md` (counts churn every ingest; pointer to README Topics table now)

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
