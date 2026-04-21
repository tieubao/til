# engineering/

Software engineering practices, principles, languages, and architecture. Reorganized into 6 sub-folders on 2026-04-19 (was 107 notes flat at the root).

## Sub-folder taxonomy

| Sub-folder | Scope | Notes |
|---|---|---|
| `go/` | Go language: syntax, idioms, performance, error handling, generics, concurrency, comparisons with adjacent languages (Elixir, Swift) | 27 |
| `functional/` | Functional programming concepts, Elixir/Elm specifics, anti-OOP critiques | 12 |
| `architecture/` | Distributed systems, microservices, monorepos, performance, security, SRE, DevOps topologies, HTTP caching, CSS architecture | 14 |
| `code-quality/` | Code reviews, deletion, type systems, naming, documentation, commit messages, PRs, conference proposals | 18 |
| `careers/` | Career advice, seniority, problem-solving, ethics, asking for help, learning to read papers, ThoughtWorks-style lessons | 19 |
| `principles/` | General engineering principles, language opinions, history pieces, communication tools, productivity rules of thumb | 17 |

Wikilinks use shortest-path resolution, so `[[zen-of-go]]` and `[[zen-of-python]]` continue to resolve after the move without backlink updates.

## Why the split

The flat 107-note structure was a category-smell, not a topic. The 2026-04-19 lint pass surfaced 40 orphans concentrated here (37%). The split is intended to make orphans more visible by domain so future repair passes can target one cluster at a time, and to make the folder navigable in Obsidian without scrolling past 100+ files.

## What lives at the root

Nothing currently. New engineering notes should be filed into the appropriate sub-folder. If a note doesn't fit any sub-folder, the right move is usually to add a new sub-folder rather than dump it at the root.
