# Learned

A personal knowledge base following the [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) methodology, maintained by LLMs using the [LLM Wiki](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) pattern.

191 interlinked notes across 20 topics. Browse in [Obsidian](https://obsidian.md/) for graph view and backlinks, or read the [full index](index.md).

## Recent additions

- `2026-04-13` [Egoless engineering](engineering/egoless-engineering.md) - Replace ego with empathy; treat code as shared, not owned
- `2026-04-13` [Choose boring technology](engineering/choose-boring-technology.md) - Innovation tokens are finite; spend them on your product, not your stack
- `2026-04-13` [CTO vs VP Engineering](leadership/cto-vs-vp-engineering.md) - CTO owns the technical vision; VP Eng owns the people and delivery machine
- `2026-04-13` [The Munger Operating System for life](life/munger-operating-system.md) - Charlie Munger's 16 life principles from his USC commencement speech
- `2026-04-13` [LLM Wiki pattern: compilation over retrieval](pkm/llm-wiki-pattern-compilation-over-retrieval.md) - Karpathy's LLM Wiki pattern analyzed against our implementation
- `2026-04-13` [**Vietnamese diaspora synthesis**](diaspora/vietnamese-diaspora-synthesis.md) - First synthesis page: 7 notes woven into a structural argument
- `2026-04-13` [Write code easy to delete, not extend](engineering/write-code-easy-to-delete.md) - Layers of disposability from no code to thin wrappers to isolated modules
- `2026-04-13` [Mastering programming](engineering/mastering-programming.md) - Kent Beck's habits: slicing, one thing at a time, concrete then abstract
- `2026-04-13` [Always be quitting](life/always-be-quitting.md) - 10 practices to make yourself replaceable and unlock career growth
- `2026-04-13` [Go To Statement considered harmful](cs/goto-considered-harmful.md) - Dijkstra's 1968 argument that goto breaks the ability to reason about code

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
| [engineering/](engineering/) | 87 | Software engineering practices, principles, languages |
| [life/](life/) | 25 | Life philosophy, habits, mindset, career wisdom |
| [cs/](cs/) | 9 | Computer science fundamentals |
| [dwarves-kit/](dwarves-kit/) | 9 | Dwarves Kit architecture and design |
| [leadership/](leadership/) | 9 | Management, negotiation, business leadership |
| [ai-tooling/](ai-tooling/) | 8 | AI developer tools and evaluations |
| [ai/](ai/) | 7 | AI concepts, memory systems, agent patterns |
| [diaspora/](diaspora/) | 6 | Vietnamese and Asian diaspora analysis |
| [claude-code/](claude-code/) | 5 | Claude Code hooks, skills, workflows |
| [history/](history/) | 5 | Chinese civilization, empires, historical patterns |
| [wealth/](wealth/) | 5 | Trust-building, business relationships |
| [geopolitics/](geopolitics/) | 3 | Oil crises, government structures |
| [health/](health/) | 2 | Wellness, nutrition |
| [investing/](investing/) | 2 | Compound interest, startup investing |
| [mcp/](mcp/) | 2 | Model Context Protocol |
| [pkm/](pkm/) | 2 | Personal knowledge management |
| [devtools/](devtools/) | 2 | Developer tools and config |
| [hiring/](hiring/) | 1 | Candidate assessment and hiring |
| [patterns/](patterns/) | 1 | Software patterns and anti-patterns |
| [youtube/](youtube/) | 1 | YouTube tooling |

## Documentation

- [Full note index](index.md) - catalog of all 191 notes with one-line summaries
- [Usage guide](_docs/guide.md) - how to add notes, use Obsidian, work with Claude
- [Architecture](_docs/architecture.md) - system design, folder conventions, operations model
- [Requirements](_docs/requirements.md) - feature tracker, design principles, scaling triggers
- [Changelog](_docs/changelog.md) - project decisions log
- [Operations log](log.md) - chronological record of ingests, queries, lints
