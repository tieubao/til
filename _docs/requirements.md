# Requirements

What this wiki should do, what it should not do, and the design principles behind it.

## Purpose

A personal knowledge base that compounds over time. Not a note-taking app. Not a bookmark dump. A structured, interlinked collection of knowledge that gets richer with every note added.

## Design principles

1. **Knowledge compounds through connections.** Individual notes are useful; linked notes are powerful. Every note should connect to related concepts.
2. **Writing is load-bearing for learning.** The human thinks and decides; the LLM handles bookkeeping. Never fully automate synthesis.
3. **Multiple ingest, one structure.** Notes arrive from different sources (web clipper, Claude Code, Claude AI, manual) but converge into the same format and linking convention.
4. **Obsidian-native.** The repo is an Obsidian vault. All conventions (wikilinks, frontmatter, templates) must work in Obsidian without plugins beyond the recommended set.
5. **Git-native.** The repo is a git repository. All content is plain markdown. No database, no proprietary format, no lock-in.
6. **Low maintenance cost.** The LLM does the tedious work (linking, filing, cross-referencing, linting). If the wiki requires manual bookkeeping to stay healthy, something is wrong.
7. **Domain over tool.** Organize by what the knowledge is about, not what tool was used to discover it.

## Functional requirements

### Ingest

| ID | Requirement | Status |
|----|-------------|--------|
| I-1 | Obsidian Clipper saves raw web captures to `_inbox/` | Done |
| I-2 | Claude Code creates refined notes directly in topic folders | Done |
| I-3 | Claude AI skill pushes refined notes via GitHub MCP Worker | Done |
| I-4 | Manual note creation via Obsidian templates | Done |
| I-5 | "Process inbox" command: Claude refines raw notes, picks folder, adds links, moves out of inbox | Done (convention), not yet tested |

### Organization

| ID | Requirement | Status |
|----|-------------|--------|
| O-1 | Topic-based folder structure (domain, not tool) | Done |
| O-2 | Note lifecycle: raw -> refined -> evergreen | Done |
| O-3 | Obsidian-compatible `[[wikilinks]]` with `## Related` sections | Done |
| O-4 | `aliases` in frontmatter for link resolution after renames | Done |
| O-5 | Claude can reorganize folders, merge/split, and update all backlinks | Done (convention) |
| O-6 | Synthesis pages for dense note clusters | Planned |

### Maintenance

| ID | Requirement | Status |
|----|-------------|--------|
| M-1 | Lint operation: orphans, broken links, stale notes, missing synthesis | Planned |
| M-2 | Auto-generated README.md index | Done (via MCP Worker) |
| M-3 | Chronological changelog of wiki operations | Done |
| M-4 | Contradiction detection between notes | Planned |
| M-5 | Auto-compile notes pushed directly to GitHub (via Claude Code Action) | Planned (see below) |

### M-5: Auto-compile via GitHub Actions (future)

Right now, notes pushed directly to GitHub (via Claude.ai skill, manual `git push`, or Obsidian sync) skip the compilation step. Claude Code catches this at session start, but only when you next open a session. If the gap becomes painful (e.g., you push from Claude.ai several times a week), automate it.

**Sketch:**

- Workflow triggers on `push` to `master` with path filter on `**/*.md`, excluding `_inbox/**`, `log.md`, `index.md`, `README.md` (to avoid loops)
- Uses `anthropics/claude-code-action@v1` with `ANTHROPIC_API_KEY` repo secret
- Reads `CLAUDE.md`, runs the 5 compilation steps on the pushed notes
- Opens a PR titled `compile: <note titles>` instead of auto-committing to master (human still reviews overlap/contradiction flags)
- `concurrency: { group: compile-notes }` to serialize parallel pushes that would race on `log.md`
- Explicit "do not auto-create synthesis pages" instruction in the action prompt (synthesis needs human thinking, per design principle 2)

**Adoption trigger**: when direct-to-GitHub pushes exceed ~1/week or the "open Claude Code to compile" friction becomes noticeable. Not worth building before then; cost is complexity, not dollars (~$2-5/month for personal volume).

### Tooling

| ID | Requirement | Status |
|----|-------------|--------|
| T-1 | Obsidian as primary viewer (graph view, backlinks, Dataview) | Done |
| T-2 | Git for version history and collaboration | Done |
| T-3 | `.gitignore` for Obsidian local config | Done |
| T-4 | Templates for all note types | Done |
| T-5 | Search tooling (qmd or similar) | Not needed at current scale (59 notes) |

## Non-requirements (decided against)

| What | Why not |
|------|---------|
| Numeric Zettelkasten IDs | Filenames serve as IDs; `aliases` handle renames |
| Separate raw sources layer | Inbox -> refined pipeline is sufficient |
| Confidence scoring on facts | Over-engineering for personal wiki |
| Supersession tracking | Same |
| Embedding-based search | grep + index sufficient at < 500 notes |
| `.obsidian/` config in git | Personal tooling, not repo convention |
| Automated synthesis (no human) | Writing is load-bearing for learning |

## Influences

- **Zettelkasten** (Luhmann): atomic notes, permanent links, emergence through connections
- **Karpathy's LLM Wiki** (April 2026): LLM as wiki maintainer, compilation layer, ingest/query/lint operations
- **Obsidian PKM ecosystem**: graph view, backlinks, Dataview, local-first markdown

## Scale expectations

- Current: ~59 notes across 16 topic folders
- Near-term: ~200 notes. Index file + grep still sufficient.
- Synthesis pages should appear when clusters reach 4+ notes

### Tooling adoption triggers

These tools are not needed now but should be adopted when specific thresholds are hit:

| Tool | What it does | Adopt when | Why not now |
|------|-------------|------------|-------------|
| **qmd** | Local markdown search with hybrid BM25/vector search and LLM re-ranking. Has CLI + MCP server. [github.com/tobi/qmd](https://github.com/tobi/qmd) | ~500 notes, or when README.md index exceeds context window (~200 entries with summaries) | grep + README summaries work fine at 59 notes. Adding search infra before it's needed creates maintenance cost for zero benefit. |
| **Marp** | Markdown-to-slides in Obsidian. Karpathy uses it to generate presentations from wiki content. | When you want quick slide previews inside Obsidian without leaving the vault | User already has a `slide-deck` skill (React+Vite+Framer Motion) that's more capable for polished decks. Marp is only useful for quick-and-dirty in-vault previews. |
| **Obsidian Marp plugin** | Renders Marp slides inside Obsidian as a preview pane | Same as above | Same as above |
| **Immutable raw archive** (`_raw/` folder) | Preserve original source documents so wiki pages can be re-derived if the LLM got something wrong | When ingesting long research papers, book chapters, or reports where the original matters | Currently `_inbox/` is a processing queue, not an archive. Git history preserves originals as fallback. A dedicated raw archive adds value when sources are complex enough that re-derivation is likely. |
