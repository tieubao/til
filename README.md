# Learned

A personal knowledge base following the [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) methodology, maintained by LLMs using the [LLM Wiki](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) pattern.

213 interlinked notes across 21 topics. Browse in [Obsidian](https://obsidian.md/) for graph view and backlinks, or read the [full index](index.md).

## Recent additions

- `2026-04-13` [Grand unified theory of the AI hype cycle](ai/grand-unified-theory-of-ai-hype-cycle.md) - AI hype follows a repeating 13-step cycle driven by novel mechanisms
- `2026-04-13` [History of regular expressions](cs/history-of-regular-expressions.md) - From 1950s neuroscience to UNIX tooling via AI winters
- `2026-04-13` [Israel, Palestine va Jerusalem](history/israel-palestine-va-jerusalem.md) - 3000 years of religious and territorial conflict in the Middle East
- `2026-04-13` [The next century of computing](cs/the-next-century-of-computing.md) - Post-Moore's Law predictions: tiled architectures, memory arenas, AR over VR
- `2026-04-13` [What's next in computing](cs/whats-next-in-computing.md) - Chris Dixon's 2016 framework for computing eras and the AI+hardware wave
- `2026-04-13` [The history of Hadoop](engineering/history-of-hadoop.md) - From Lucene to 42,000-node clusters via two Google papers
- `2026-04-13` [A brief totally accurate history of programming languages](cs/brief-totally-accurate-history-of-programming-languages.md) - Satirical timeline of programming language development
- `2026-04-13` [History of software - resource collection](cs/history-of-software-resources.md) - Curated links covering software history from multiple angles

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
| [engineering/](engineering/) | 92 | Software engineering practices, principles, languages |
| [life/](life/) | 25 | Life philosophy, habits, mindset, career wisdom |
| [cs/](cs/) | 14 | Computer science fundamentals |
| [dwarves-kit/](dwarves-kit/) | 9 | Dwarves Kit architecture and design |
| [leadership/](leadership/) | 15 | Management, negotiation, business leadership |
| [ai-tooling/](ai-tooling/) | 8 | AI developer tools and evaluations |
| [ai/](ai/) | 8 | AI concepts, memory systems, agent patterns |
| [diaspora/](diaspora/) | 6 | Vietnamese and Asian diaspora analysis |
| [claude-code/](claude-code/) | 5 | Claude Code hooks, skills, workflows |
| [history/](history/) | 6 | Chinese civilization, empires, Middle East, historical patterns |
| [wealth/](wealth/) | 5 | Trust-building, business relationships |
| [geopolitics/](geopolitics/) | 3 | Oil crises, government structures |
| [health/](health/) | 2 | Wellness, nutrition |
| [investing/](investing/) | 2 | Compound interest, startup investing |
| [mcp/](mcp/) | 2 | Model Context Protocol |
| [pkm/](pkm/) | 2 | Personal knowledge management |
| [devtools/](devtools/) | 2 | Developer tools and config |
| [startup/](startup/) | 3 | Startup strategy, product focus, fraud patterns |
| [hiring/](hiring/) | 1 | Candidate assessment and hiring |
| [patterns/](patterns/) | 1 | Software patterns and anti-patterns |
| [youtube/](youtube/) | 1 | YouTube tooling |

## Documentation

- [Full note index](index.md) - catalog of all 213 notes with one-line summaries
- [Usage guide](_docs/guide.md) - how to add notes, use Obsidian, work with Claude
- [Architecture](_docs/architecture.md) - system design, folder conventions, operations model
- [Requirements](_docs/requirements.md) - feature tracker, design principles, scaling triggers
- [Changelog](_docs/changelog.md) - project decisions log
- [Operations log](log.md) - chronological record of ingests, queries, lints
