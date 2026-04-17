---
title: "Zed global agent rules live in the Rules Library, not AGENTS.md"
date: 2026-04-17
captured: 2026-04-17T13:43:34.349Z
tags: ["zed", "ai-agents", "claude", "editor-config", "rules"]
source: "Claude Code session while auditing a new AGENTS.md added to a chezmoi dotfiles repo"
---
## Context

I added `AGENTS.md` to the root of my chezmoi dotfiles repo hoping it would stop Zed's Claude agent from using em dashes across every project I open. The em dashes kept coming. I assumed a missing rule, wrong phrasing, or the wrong file. The actual issue was that I had the wrong mental model of how Zed loads rules.

## The problem

Zed's agent panel kept producing em dashes globally, regardless of which repo was open. I had already put a "never use em dashes" instruction in a repo-root `AGENTS.md`, same folder as `CLAUDE.md`. It had no effect outside that one repo, which is exactly correct behaviour, I just didn't realize it.

## What I found

Zed has two completely separate rule systems, and only one is global:

```
┌────────────────────────────────────────────────────────────┐
│  Zed rule scopes                                           │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  Project-level  →  .rules  or  AGENTS.md  (repo root)      │
│                    Auto-loaded ONLY when that project is   │
│                    the active workspace.                   │
│                                                            │
│  User-level     →  Rules Library (LMDB database)           │
│                    ~/.config/zed/prompts/                  │
│                       prompts-library-db.0.mdb/            │
│                    Loaded into every thread across every   │
│                    project, IF you mark the rule as        │
│                    "default" via the paperclip icon.       │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

**There is no global file equivalent of `AGENTS.md`.** The Rules Library is a UI-only feature backed by an LMDB database. You can't hand-edit it, and you can't version-control it cleanly with chezmoi.

### How to configure a default rule

1. Open Zed, open the agent panel.
2. `cmd-alt-l` (or `...` menu → `Rules...`). Command palette name: `agent: open rules library`.
3. Click `+` to create a new rule.
4. **Click the paperclip icon** in the top-right of the rule editor. That is the "make this a default" button. Without it, the rule only activates when you `@`-mention it.

### The em dash nuance

A soft "avoid em dashes" rule isn't strong enough. Claude's pretraining leans heavily toward em dashes, so the rule needs absolute framing:

> Never use em dashes (—) in any output. This is absolute, not a stylistic preference. Use a hyphen (-), comma, semicolon, or split the sentence.

The assertive phrasing beats the concise one.

### Why this is a sharp edge

- `settings.json` has an `agent` block (`default_model`, `default_profile`, `always_allow_tool_actions`) but no `system_prompt` or `rules` field. There is no JSON escape hatch.
- Zed's own docs describe project `.rules` files and the Rules Library, but don't connect the dots that **the Rules Library IS the global mechanism**. Easy to read the docs and still not realize this.
- The LMDB storage means you cannot sync these rules across machines via dotfiles. If you reinstall, you re-enter them in the UI. Budget for that.

## How to spot this again

- You edit a rule file and behavior only changes for one repo → you're in the project-scope trap. Move the content into the Rules Library.
- You want a Claude-style global system prompt and reach for `settings.json` → there isn't one. Rules Library, paperclip icon.
- You set up a new machine and Zed's agent "forgets" your style → the LMDB didn't come along. Recreate the default rule.
- Em dashes keep appearing despite a rule → your rule phrasing is too soft. Use absolute language.

## Related

- Project-root `AGENTS.md` / `CLAUDE.md` still earn their keep for repo-specific context (architecture, conventions, verification rules). They stack on top of the global default rule, they don't replace it.
- The agents.md spec is followed by Cursor, Aider, and Zed's agent panel. Keep one at repo root for any serious project, same as a README.

#zed #ai-agents #claude #editor-config