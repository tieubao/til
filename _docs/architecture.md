# Architecture

How the wiki is built, the decisions behind the structure, and the conventions that hold it together.

## System overview

```
                    ┌─────────────────────────────────────────┐
                    │            Obsidian (viewer)             │
                    │  Graph view, backlinks, Dataview, edit   │
                    └──────────────────┬──────────────────────┘
                                       │ reads/writes
                    ┌──────────────────▼──────────────────────┐
                    │         Git repo (source of truth)       │
                    │                                          │
                    │  _inbox/     ← Obsidian Clipper (raw)    │
                    │  _templates/ ← Note templates            │
                    │  _docs/      ← Project documentation     │
                    │  ai/         ← Topic notes (refined)     │
                    │  diaspora/   ← Topic notes (refined)     │
                    │  ...         ← More topic folders         │
                    │  CLAUDE.md   ← Schema (LLM instructions) │
                    │  README.md   ← Auto-generated index      │
                    │                                          │
                    └───┬──────────────┬──────────────┬───────┘
                        │              │              │
                   Clipper ingest  Claude Code   Claude AI skill
                   (raw → _inbox)  (refined)    (refined via MCP)
```

## Three-layer model

Adapted from Karpathy's LLM Wiki pattern:

| Layer | Karpathy's version | Our version |
|-------|-------------------|-------------|
| **Raw sources** | Immutable source documents | `_inbox/` (raw Clipper captures) |
| **Wiki** | LLM-generated markdown pages | Topic folders with refined notes + wikilinks |
| **Schema** | CLAUDE.md / AGENTS.md | `CLAUDE.md` at repo root |

Key difference: we don't maintain a separate raw sources archive. Once a raw note is processed, the refined version replaces it. The git history preserves the original if needed.

## Folder conventions

### Infrastructure folders (underscore prefix)

| Folder | Purpose | In index? | In graph? |
|--------|---------|-----------|-----------|
| `_inbox/` | Raw capture landing zone | No | No |
| `_templates/` | Obsidian note templates | No | No |
| `_docs/` | Project documentation | No | No |

### Content folders (topic-based)

Named by domain. Lowercase, hyphenated. Examples: `ai/`, `ai-tooling/`, `diaspora/`, `wealth/`.

Rules:
- Domain over tool (YouTube extraction -> `youtube/`, not `nodejs/`)
- No date-based paths
- Small folders (1-2 notes) are fine; they represent legitimate domains that will grow
- Merge only when domains genuinely overlap

### Special files at root

| File | Purpose | Who maintains it |
|------|---------|-----------------|
| `CLAUDE.md` | Schema: tells Claude how the wiki works | Claude + human co-evolve |
| `README.md` | Auto-generated note index | MCP Worker or Claude |
| `.gitignore` | Excludes Obsidian local config | Human |

## Note anatomy

```yaml
---
title: "lowercase with selective Capitalization for proper nouns"
date: 2026-04-13
captured: 2026-04-13T10:30:00.000Z
tags: ["topic", "subtopic"]
source: "article URL, conversation, book, etc."
aliases: ["alternate-name", "old-filename-if-renamed"]
status: refined        # raw | refined | evergreen
---

## Context / Definition / Overview
(content varies by note type)

## The Problem / Discovery / etc.
(content sections per template)

## Related

- [[linked-note]] - brief explanation of connection
- [[another-note]] - why this relates
```

## Linking strategy

### Wikilink format

`[[filename-without-extension]]` (Obsidian shortest-path resolution, no folder prefix)

### Where links live

Every refined/evergreen note has a `## Related` section at the bottom. Each link includes a relationship phrase:

```markdown
## Related

- [[llm-memory-systems-three-competitive-battlegrounds]] - compares the same memory architectures
- [[why-knowledge-notes-need-context-not-just-facts]] - meta-insight about what makes notes useful
```

### Link density targets

- 2-4 outgoing links per note (don't force connections)
- Cross-folder links encouraged (this is where emergence happens)
- Synthesis pages link to all notes they cover

### Rename safety

When renaming a note:
1. Add old filename to `aliases` array in frontmatter
2. Grep for `[[old-name]]` across all `.md` files
3. Update references to new name
4. Obsidian also resolves aliases, so links work even before updating

## Operations model

### Ingest

```
Source → _inbox/ (raw) → Claude processes → topic folder (refined) → linked
```

Or for Claude Code / AI skill:
```
Conversation → Claude creates note → topic folder (refined) → linked
```

### Query and file back

When a conversation produces a reusable synthesis:
```
Conversation → Claude writes note from answer → topic folder → linked
```

This is how explorations compound. Good answers become wiki pages.

### Lint (periodic)

Health check that audits:
1. Orphan notes (no links in or out)
2. Broken `[[wikilinks]]`
3. Notes in `raw` status outside `_inbox/`
4. Refined notes missing `## Related` section
5. Clusters with 4+ notes but no synthesis page
6. Stale notes (90+ days, heavily linked)
7. Contradictions between notes

### Synthesis (on demand)

When a topic cluster is dense enough (4+ notes), create a synthesis page:
- `type: synthesis` in frontmatter
- Narrative connecting the cluster, not a summary list
- Identifies patterns, contradictions, evolving thesis
- Always discussed with the human before writing

## Decision log

Major architectural decisions are recorded in `_docs/changelog.md`. See the `[2026-04-13] refactor | Zettelkasten migration` entry for the founding decisions.

Future decisions should be logged there when they change how the wiki works (not just adding content).
