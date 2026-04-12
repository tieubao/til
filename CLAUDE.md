# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A personal knowledge base following Zettelkasten methodology. Markdown notes organized by topic folders, with wikilinks for connections between notes. Compatible with Obsidian as a vault. Multiple ingest paths: Obsidian Clipper (raw web captures), Claude Code, and Claude AI skills.

## Repo structure

- Each topic is a folder at the root (e.g., `ai-tooling/`, `patterns/`, `mcp/`, `claude-code/`)
- `_inbox/` is the landing zone for raw, unprocessed notes (Obsidian Clipper dumps here)
- `_templates/` contains Obsidian templates for new notes
- `_docs/` contains project documentation (requirements, architecture, guide, changelog)
- `assets/` holds images and attachments
- Notes are markdown files with YAML frontmatter
- `README.md` is an auto-generated index of all notes, grouped by folder. Do not manually edit it.

## Session start check

At the start of every Claude Code session in this repo, check for uncompiled additions:

1. Run `git log --oneline -10` to see recent commits
2. Check if any notes were added via Claude.ai skill or direct git push that haven't been compiled (look for commits without corresponding `log.md` entries)
3. If uncompiled notes exist, offer: "I see X new notes added since last compilation. Want me to run the compilation step on them?"

This catches notes pushed from Claude.ai or other sources that bypass Claude Code's compilation step.

## Note format

Notes follow specific templates defined in the user's global CLAUDE.md (knowledge capture rules). Key points:

- **Frontmatter fields**:
  - `title` (lowercase with selective capitalization)
  - `date` (YYYY-MM-DD)
  - `captured` (ISO timestamp)
  - `tags` (array)
  - `source` (where the knowledge came from)
  - `aliases` (array, optional; alternate titles for Obsidian link resolution)
  - `status`: one of `raw` | `refined` | `evergreen`
    - `raw`: just captured, unprocessed (typical for Clipper notes in `_inbox/`)
    - `refined`: reviewed, has frontmatter, placed in correct folder, has links
    - `evergreen`: mature note, well-linked, updated over time
- **Depth types**: TIL (shallow), Atomic Note (default/medium), Article (deep), Definition (reference)
- **Context is mandatory** for Atomic Notes and Articles
- **Filename convention**: lowercase, hyphen-separated, descriptive (e.g., `redundant-api-pre-checks-in-wrapper-functions.md`)

## Wikilinks and backlinks

This repo uses Obsidian-compatible `[[wikilinks]]` for connections between notes.

- Use `[[filename-without-extension]]` to link to another note (Obsidian resolves shortest path)
- Every refined/evergreen note should have a `## Related` section at the bottom with links to connected notes
- When linking, prefer linking to the concept, not just listing related files. Write a brief phrase explaining the relationship:
  ```
  ## Related
  - [[turing-completeness]] - foundational concept behind this pattern
  - [[redundant-api-pre-checks-in-wrapper-functions]] - another example of this anti-pattern
  ```
- When moving or renaming a note, update all backlinks that reference it. Use `aliases` in frontmatter so Obsidian can still resolve old names.

## Inbox workflow

The `_inbox/` folder is the landing zone for raw captures:

1. **Obsidian Clipper** saves web clips directly to `_inbox/` with whatever format it produces
2. **Processing**: When asked to "process inbox" or "reorganize", Claude will:
   - Add/fix frontmatter (title, date, tags, status)
   - Determine the correct topic folder
   - Rewrite or restructure content to match note templates
   - Add `[[wikilinks]]` to related existing notes
   - Move the note from `_inbox/` to the topic folder
   - Set status to `refined`
   - Run the compilation step (see below)
3. Notes pushed via Claude Code/AI skill bypass `_inbox/` (they arrive already refined) but still trigger the compilation step

## Compilation step (run on every note addition)

This is the core of the LLM Wiki pattern. Filing a note is not enough; the wiki must compile the new knowledge into the existing structure. After adding or refining a note, Claude will:

1. **Check for overlapping claims.** Search for existing notes that cover the same topic. If the new note adds to, refines, or contradicts existing notes, update the `## Related` sections on both sides with a note about the relationship.

2. **Flag contradictions.** If the new note contradicts an existing note, add a callout to both notes:
   ```markdown
   > [!warning] Contradiction
   > This note claims X, but [[other-note]] claims Y. See context for which may be more current.
   ```
   Do not silently resolve contradictions. Flag them for human review.

3. **Update synthesis pages.** If a synthesis page exists for this topic cluster, update it to incorporate the new note. Add the new note to its links, and revise the narrative if the new information changes the synthesis. If no synthesis page exists but the cluster now has 4+ notes, suggest creating one.

4. **Update `README.md` index.** Add or remove the note entry with a one-line summary. Keep the note count in the header accurate. On deletions or merges, remove stale entries and update the count.

5. **Append to `log.md`.** Record what was ingested and what pages were touched:
   ```
   ## [YYYY-MM-DD] ingest | <note title>
   Added to <folder>. Updated links on: <list>. Synthesis page: <updated/suggested/none>.
   ```

## Query operation

When the user asks a question that requires synthesizing across multiple notes, Claude will:

1. **Read `log.md`** first to understand recent activity and context.
2. **Read `README.md`** index to find relevant pages by summary, then drill into them.
3. **Synthesize an answer** from the compiled wiki, citing specific notes with `[[wikilinks]]`.
4. **Offer to file the answer back.** If the synthesis is substantial (not a quick lookup), ask: "Want me to file this as a wiki page?" Good answers become new notes. This is how explorations compound.
5. **If filed**, run the compilation step: check for overlaps, flag contradictions, update synthesis pages, append to `log.md` with operation type `query`.

The key principle: answers that required real synthesis across multiple notes are themselves valuable knowledge. They should not vanish into chat history.

## Commit message convention

```
learned: <note title in sentence case>
```

For deletions: `delete: <path>`. For index updates: `docs: update note index`.
For reorganization: `refactor: reorganize <description>`.
For link updates: `docs: update backlinks`.

## Adding a new note

1. Choose or create a topic folder (check existing folders first; prefer domain over technique)
2. Create the markdown file with proper frontmatter and content sections
3. Add `## Related` section with `[[wikilinks]]` to connected notes
4. The README.md index is managed separately (via MCP worker or manual update)

## Topic folder selection

- Reuse existing folders when content fits
- Organize by domain, not by tool (e.g., YouTube extraction goes in `youtube/`, not `nodejs/`)
- Never use date-based paths for topic organization
- When reorganizing, Claude may merge, split, or rename folders. Always update all backlinks after moves.

## Synthesis pages

Synthesis pages weave multiple related notes into a coherent narrative. They are the "compounding layer" that turns a note collection into a knowledge base.

- **Frontmatter**: same as regular notes, but add `type: synthesis` field
- **Filename**: `<topic>-synthesis.md` (e.g., `memory-systems-synthesis.md`)
- **Location**: in the topic folder of the primary cluster
- **Content**: not a summary list; a narrative that connects the notes, identifies patterns, flags contradictions, and states the author's evolving thesis
- **Links**: should link to all the notes it synthesizes, plus cross-folder connections
- **When to create**: when a topic folder has 4+ notes that form a coherent cluster
- **Human in the loop**: always discuss the synthesis with the user before writing. The thinking is the point, not the output.

## Lint operation

When asked to "lint the wiki" or "health check", Claude will audit:

1. **Orphan notes**: notes with no incoming or outgoing wikilinks
2. **Broken links**: `[[wikilinks]]` that point to non-existent notes
3. **Raw stragglers**: notes still in `status: raw` outside of `_inbox/`
4. **Missing Related sections**: refined notes without a `## Related` section
5. **Thin clusters**: topic folders with 4+ notes but no synthesis page
6. **Stale notes**: notes not updated in 90+ days that are heavily linked (may need refresh)
7. **Contradictions**: notes that make conflicting claims (flag for human review)

Output: a markdown report with actionable items, grouped by severity.

## Logs (two separate files)

There are two logs serving different purposes:

**`log.md` (root)** - Content operations log. Records ingests, queries, lints, synthesis. The LLM reads this at session start to understand recent wiki activity. Append after every content operation.

**`_docs/changelog.md`** - Project decisions log. Records structural changes to the wiki itself (new conventions, folder restructures, tooling changes). Updated only when the wiki's design changes.

Format for both:
```markdown
## [YYYY-MM-DD] operation | description

What happened and why.
```

`log.md` operations: `ingest`, `query`, `lint`, `synthesis`.
`changelog.md` operations: `refactor`, `decision`, `migration`.

## Project documentation

All project docs live in `_docs/`:

- `requirements.md` - what the wiki should do, feature tracker, non-requirements
- `architecture.md` - how it's built, folder conventions, operations model
- `guide.md` - usage manual for all workflows
- `changelog.md` - project decisions log (not content operations; those go in `log.md`)

## Reorganization guidelines

Claude may reorganize notes when asked. Rules:

- Check for orphaned notes (no incoming or outgoing links) and suggest connections
- Merge small folders only if they genuinely overlap in domain (don't force it)
- After any move/rename, grep for `[[old-name]]` across all notes and update references
- Commit moves and link updates separately from content changes
- Update `log.md` after every reorganization
