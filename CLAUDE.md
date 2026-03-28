# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A personal TIL (Today I Learned) knowledge repository. Markdown notes organized by topic folders, with an auto-generated `README.md` index.

## Repo structure

- Each topic is a folder at the root (e.g., `ai-tooling/`, `patterns/`, `mcp/`, `claude-code/`)
- Notes are markdown files with YAML frontmatter: `title`, `date`, `captured`, `tags`, `source`
- `README.md` is an auto-generated index of all notes, grouped by folder. Do not manually edit it.

## Note format

Notes follow specific templates defined in the user's global CLAUDE.md (knowledge capture rules). Key points:

- **Frontmatter**: title (lowercase with selective capitalization), date, captured (ISO timestamp), tags (array), source
- **Depth types**: TIL (shallow), Atomic Note (default/medium), Article (deep), Definition (reference)
- **Context is mandatory** for Atomic Notes and Articles
- **Filename convention**: lowercase, hyphen-separated, descriptive (e.g., `redundant-api-pre-checks-in-wrapper-functions.md`)

## Commit message convention

```
learned: <note title in sentence case>
```

For deletions: `delete: <path>`. For index updates: `docs: update note index`.

## Adding a new note

1. Choose or create a topic folder (check existing folders first; prefer domain over technique)
2. Create the markdown file with proper frontmatter and content sections
3. The README.md index is managed separately (via MCP worker or manual update)

## Topic folder selection

- Reuse existing folders when content fits
- Organize by domain, not by tool (e.g., YouTube extraction goes in `youtube/`, not `nodejs/`)
- Never use date-based paths for topic organization
