# Learned

A personal knowledge base following the [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) methodology, maintained by LLMs using the [LLM Wiki](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) pattern.

91 interlinked notes across 18 topics. Browse in [Obsidian](https://obsidian.md/) for graph view and backlinks, or read the [full index](index.md).

## Recent additions

- [LLM Wiki pattern: compilation over retrieval](pkm/llm-wiki-pattern-compilation-over-retrieval.md) — Karpathy's LLM Wiki pattern analyzed against our implementation
- [**Vietnamese diaspora synthesis**](diaspora/vietnamese-diaspora-synthesis.md) — First synthesis page: 7 notes woven into a structural argument
- [The Munger Operating System for life](life/munger-operating-system.md) — Charlie Munger's 16 life principles from his USC commencement speech
- [Always be quitting](life/always-be-quitting.md) — 10 practices to make yourself replaceable and unlock career growth
- [Masayoshi Son and the SoftBank Vision Fund](leadership/masayoshi-son-softbank-vision.md) — $100B fund betting AI runs the planet
- [Simple burnout triage](life/simple-burnout-triage.md) — One question: can you sustain the last 2 months forever?
- [How and why I invest in startups](investing/how-and-why-i-invest-in-startups.md) — Fund the best people on the hardest problems
- [Vipassana for hackers](life/vipassana-for-hackers.md) — 10-day silent meditation explained for rational minds
- [Vitamins and longevity stack](health/vitamins-and-longevity-stack.md) — Daily supplement stack with dosages
- [What it feels like to become poor](life/what-it-feels-like-to-become-poor.md) — Lost $3M in 2008; humility from the car wash

## How it works

```
Obsidian Clipper (raw) ──┐
Claude Code (refined) ───┤──► this repo ──► Obsidian reads/lints/links
Claude AI skill (refined)┘
```

- **Ingest**: new knowledge arrives from web clips, coding sessions, or research conversations
- **Compile**: the LLM checks for overlaps, flags contradictions, updates cross-references and synthesis pages
- **Query**: ask questions against the compiled wiki; good answers get filed back as new notes
- **Lint**: periodic health checks for orphans, broken links, stale claims

The human thinks and curates. The LLM handles the bookkeeping.

## Topics

| Folder | Notes | Domain |
|--------|-------|--------|
| [life/](life/) | 24 | Life philosophy, habits, mindset, career wisdom |
| [dwarves-kit/](dwarves-kit/) | 9 | Dwarves Kit architecture and design |
| [ai-tooling/](ai-tooling/) | 8 | AI developer tools and evaluations |
| [ai/](ai/) | 7 | AI concepts, memory systems, agent patterns |
| [diaspora/](diaspora/) | 6 | Vietnamese and Asian diaspora analysis |
| [leadership/](leadership/) | 6 | Management, negotiation, business leadership |
| [claude-code/](claude-code/) | 5 | Claude Code hooks, skills, workflows |
| [history/](history/) | 5 | Chinese civilization, empires, historical patterns |
| [wealth/](wealth/) | 5 | Trust-building, business relationships |
| [geopolitics/](geopolitics/) | 3 | Oil crises, government structures |
| [health/](health/) | 2 | Wellness, nutrition |
| [investing/](investing/) | 2 | Compound interest, startup investing |
| [mcp/](mcp/) | 2 | Model Context Protocol |
| [pkm/](pkm/) | 2 | Personal knowledge management |
| [devtools/](devtools/) | 2 | Developer tools and config |
| [cs/](cs/) | 1 | Computer science fundamentals |
| [patterns/](patterns/) | 1 | Software patterns and anti-patterns |
| [youtube/](youtube/) | 1 | YouTube tooling |

## Documentation

- [Full note index](index.md) — catalog of all 91 notes with one-line summaries
- [Usage guide](_docs/guide.md) — how to add notes, use Obsidian, work with Claude
- [Architecture](_docs/architecture.md) — system design, folder conventions, operations model
- [Requirements](_docs/requirements.md) — feature tracker, design principles, scaling triggers
- [Changelog](_docs/changelog.md) — project decisions log
- [Operations log](log.md) — chronological record of ingests, queries, lints
