# Wiki usage guide

A manual for using this personal knowledge wiki. Covers all workflows, conventions, and tooling.

## Quick start

This repo is a personal knowledge base in Zettelkasten style. It works as:
- A **git repo** of markdown files (the source of truth)
- An **Obsidian vault** (for browsing, graph view, and local editing)
- An **LLM-maintained wiki** (Claude handles the bookkeeping)

Open the repo folder in Obsidian as a vault. That's it.

## Repo layout

```
til/
  _docs/             # Project documentation
    requirements.md  # What the wiki should do
    architecture.md  # How it's built, decisions
    guide.md         # This file (usage manual)
    changelog.md     # Chronological operations log
  _inbox/            # Raw captures land here (Obsidian Clipper)
  _templates/        # Obsidian templates for new notes
  assets/            # Images and attachments
  ai/                # Topic folder
  ai-tooling/        # Topic folder
  claude-code/       # Topic folder
  ...                # More topic folders
  CLAUDE.md          # Schema: tells Claude how the wiki works
  README.md          # Auto-generated index of all notes
  .gitignore         # Ignores Obsidian local config
```

## How to add notes

There are three ways to get notes into the wiki:

### 1. Obsidian Clipper (web captures)

For saving articles, blog posts, or web pages you're reading.

1. Install the [Obsidian Web Clipper](https://obsidian.md/clipper) browser extension
2. Configure it to save to the `_inbox/` folder in this vault
3. Set up the Clipper template (see below)
4. When you find something worth saving, clip it
5. The raw note lands in `_inbox/` with frontmatter from the template
6. Later, ask Claude to "process inbox" to refine and file the notes

**Recommended Clipper template:**

In Obsidian Clipper settings, set the note template to:

```
---
title: "{{title}}"
date: {{date}}
captured: {{date}}T{{time}}
tags: []
source: "{{url}}"
aliases: []
status: raw
---

{{content}}
```

This ensures raw captures arrive with `status: raw` and the source URL pre-filled, so Claude knows where the content came from during processing.

### 2. Claude Code (from terminal)

For capturing learning moments during coding sessions.

```bash
# Claude will create the note, pick the right folder, add links
claude "capture this as a note: [describe what you learned]"

# Or use the knowledge-capture skill
claude "/learned"
```

Claude creates the note directly in the correct topic folder with proper frontmatter and wikilinks. No inbox step needed.

### 3. Claude AI (from claude.ai chat)

For capturing insights during research/discussion sessions.

Use the knowledge-capture skill to push notes to GitHub via the MCP Worker. Notes arrive already refined in the correct topic folder.

### 4. Manual (in Obsidian)

1. Navigate to the right topic folder
2. Create a new note using one of the templates in `_templates/`:
   - **Atomic Note** (default): for concepts with context and insight
   - **TIL**: for quick facts and gotchas
   - **Article**: for deep dives and debugging journeys
   - **Definition**: for reference cards
3. Fill in the frontmatter and content
4. Add a `## Related` section with `[[wikilinks]]` to connected notes

## Note format

Every note has YAML frontmatter:

```yaml
---
title: "lowercase with selective Capitalization"
date: 2026-04-13
captured: 2026-04-13T10:30:00Z
tags: ["topic", "subtopic"]
source: "where this came from"
aliases: ["alternate name"]
status: refined
---
```

### Status lifecycle

| Status | Meaning | Where it lives |
|--------|---------|----------------|
| `raw` | Just captured, unprocessed | `_inbox/` |
| `refined` | Has frontmatter, correct folder, has links | Topic folder |
| `evergreen` | Mature, well-linked, updated over time | Topic folder |

### Depth types

| Type | When to use | Length |
|------|-------------|--------|
| TIL | Pure gotcha, config flag, no investigation | < 200 words |
| Atomic Note | Single concept with nuance (default) | 200-500 words |
| Article | Multi-concept discovery, debugging journey | 500+ words |
| Definition | "What is X" concept lookup | 100-300 words |

## Wikilinks and connections

This wiki uses Obsidian-compatible `[[wikilinks]]` for connections.

### How to link

```markdown
## Related

- [[note-filename-without-extension]] - brief explanation of the connection
- [[another-note]] - why this is related
```

### Rules

- Use `[[filename]]` without folder path (Obsidian resolves it)
- Every refined note should have a `## Related` section at the bottom
- Link to concepts, not just list related files
- 2-4 links per note is ideal; don't force connections
- Cross-folder links are encouraged (e.g., diaspora note linking to wealth note)

### After moving or renaming a note

1. Add the old filename to the `aliases` array in frontmatter
2. Search for `[[old-name]]` across all notes and update references
3. Or ask Claude to handle it: "I renamed X to Y, update all backlinks"

## Working with Claude

### Process inbox

When `_inbox/` has raw captures waiting:

```
process inbox
```

Claude will: add/fix frontmatter, determine the correct topic folder, restructure content to match templates, add wikilinks, move the note out of inbox, set status to `refined`.

### Reorganize

Ask Claude to reorganize when the wiki feels messy:

```
reorganize the wiki
```

Claude will: check for orphan notes, suggest folder merges/splits, update all backlinks after moves, commit changes separately from content.

### Lint (planned)

Periodic health check:

```
lint the wiki
```

Claude will check for: orphan notes (no links in or out), broken wikilinks, notes still in `raw` status, stale notes, clusters missing synthesis pages, contradictions between notes.

### Query and file back

When a conversation produces a useful synthesis or analysis:

```
file that answer as a wiki page
```

Claude will create a note from the conversation output, properly linked and filed. This is how explorations compound into the knowledge base.

## Topic folders

### Choosing a folder

1. Check existing folders first (`ls` or browse in Obsidian)
2. Organize by **domain**, not by tool (YouTube extraction goes in `youtube/`, not `nodejs/`)
3. Never use date-based paths
4. When in doubt, ask Claude; it knows the existing structure

### Current folders

| Folder | Domain | Notes |
|--------|--------|-------|
| `ai/` | AI concepts, memory systems, agent patterns | 7 |
| `ai-tooling/` | AI tool evaluations, dev stack analysis | 8 |
| `claude-code/` | Claude Code hooks, skills, workflows | 5 |
| `cs/` | Computer science fundamentals | 1 |
| `devtools/` | Developer tools and config | 2 |
| `diaspora/` | Vietnamese/Asian diaspora analysis | 6 |
| `dwarves-kit/` | Dwarves Kit architecture and design | 9 |
| `geopolitics/` | Geopolitical analysis | 3 |
| `health/` | Health and wellness | 1 |
| `history/` | Historical analysis (China, civilizations) | 5 |
| `investing/` | Personal finance and investing | 1 |
| `mcp/` | Model Context Protocol | 2 |
| `patterns/` | Software patterns and anti-patterns | 1 |
| `pkm/` | Personal knowledge management meta | 2 |
| `wealth/` | Trust-building, business relationships | 5 |
| `youtube/` | YouTube-related tooling | 1 |

## Obsidian setup

### Core settings

These settings should be configured when first opening the vault:

1. **Settings > Files and links > New link format**: "Shortest path when possible"
2. **Settings > Files and links > Use [[Wikilinks]]**: ON
3. **Settings > Files and links > Default location for new attachments**: "In the folder specified below" -> `assets`
4. **Settings > Templates > Template folder location**: `_templates`

### Image handling

After clipping a web article with images:

1. **Settings > Hotkeys** > search "Download" > bind "Download all images" to `Ctrl+Shift+D` (or `Cmd+Shift+D` on Mac)
2. After clipping an article, press the hotkey to download all images to `assets/`
3. This makes images local so they survive broken URLs and the LLM can view them directly

Note: LLMs can't read markdown with inline images in one pass. The workaround is to have the LLM read the text first, then view referenced images separately.

### Recommended plugins

| Plugin | Purpose |
|--------|---------|
| **Linter** | Enforces frontmatter format, heading style, trailing spaces |
| **Obsidian Clipper** | Web capture to `_inbox/` |
| **Dataview** | Query notes by tags, date, status, type |
| **Graph Analysis** | Visualize connections, find orphans and clusters |
| **Consistent Attachments and Links** | Fix broken links when moving notes |
| **Templates** | Use files from `_templates/` for new notes |

### Dataview examples

List all raw notes waiting for processing:
```dataview
TABLE date, source
FROM "_inbox"
WHERE status = "raw"
SORT date DESC
```

List orphan candidates (notes with no outgoing links):
```dataview
TABLE date, tags
FROM "" AND -"_inbox" AND -"_templates"
WHERE !contains(file.outlinks, "")
SORT date DESC
```

List all notes by topic:
```dataview
TABLE length(file.outlinks) as "Links out", length(file.inlinks) as "Links in"
FROM "" AND -"_inbox" AND -"_templates"
SORT file.folder ASC
```

### Graph view tips

- Use Graph View (`Ctrl/Cmd + G`) to see the full wiki structure
- Color-code by folder to see topic clusters
- Orphan notes (disconnected dots) need attention
- Hub notes (many connections) are candidates for synthesis pages

## Git workflow

### Commit conventions

| Type | Format | Example |
|------|--------|---------|
| New note | `learned: <title>` | `learned: LLM memory systems three competitive battlegrounds` |
| Delete | `delete: <path>` | `delete: old-folder/stale-note.md` |
| Index update | `docs: update note index` | |
| Reorganize | `refactor: <description>` | `refactor: merge predictive-history into history` |
| Link updates | `docs: update backlinks` | |
| Meta changes | `chore: <description>` | `chore: add frontmatter fields to all notes` |

### What to commit

- Note content changes and new notes: commit individually
- Bulk operations (frontmatter updates, link passes): commit as a batch
- Folder moves + backlink updates: commit together

## The changelog

`_docs/changelog.md` tracks all wiki operations chronologically. Format:

```markdown
## [YYYY-MM-DD] operation | description

What happened and why. Decisions made. Files affected.
```

Operations: `ingest`, `refactor`, `lint`, `synthesis`, `query`.

Update the log after any significant wiki operation. Claude will do this automatically when processing inbox or reorganizing.

## Philosophy

This wiki follows a hybrid approach:

- **Zettelkasten**: atomic notes, wikilinks, note maturity lifecycle, emergence through connections
- **LLM Wiki** (Karpathy pattern): LLM handles bookkeeping (linking, filing, cross-referencing, linting)
- **Human in the loop**: you do the thinking and decide what's worth capturing; Claude does the grunt work

The human's job: curate sources, direct analysis, ask good questions, think about what it means.
The LLM's job: summarizing, cross-referencing, filing, bookkeeping, maintenance.

Key principle: **writing is load-bearing for learning.** Don't just have Claude generate synthesis pages silently. Discuss topics with Claude, form your own thesis, then ask Claude to write it up. A wiki you never read is just a database.
