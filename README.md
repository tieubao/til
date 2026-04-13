# Learned

A personal knowledge base following the [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) methodology, maintained by LLMs using the [LLM Wiki](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) pattern.

242 interlinked notes across 22 topics. Browse in [Obsidian](https://obsidian.md/) for graph view and backlinks, or read the [full index](index.md).

## Recent additions

- `2026-04-13` [Railway Oriented Programming](engineering/railway-oriented-programming.md) - Chain operations as a pipeline; errors travel a separate track
- `2026-04-13` [Functional Programming for the rest of us](engineering/fp-for-the-rest-of-us.md) - FP explained without math; closures, HOFs, currying, laziness
- `2026-04-13` [Ray Dalio on Bitcoin](crypto/ray-dalio-on-bitcoin.md) - Bridgewater's assessment: Bitcoin as digital gold with real risks
- `2026-04-13` [Token emission models](crypto/token-emission-models.md) - Six models from fixed supply to algorithmic rebasing
- `2026-04-13` [Egoless engineering](engineering/egoless-engineering.md) - Replace ego with empathy; treat code as shared, not owned
- `2026-04-13` [Choose boring technology](engineering/choose-boring-technology.md) - Innovation tokens are finite; spend them on your product
- `2026-04-13` [CTO vs VP Engineering](leadership/cto-vs-vp-engineering.md) - CTO owns technical vision; VP Eng owns people and delivery
- `2026-04-13` [LLM Wiki pattern: compilation over retrieval](pkm/llm-wiki-pattern-compilation-over-retrieval.md) - Karpathy's LLM Wiki pattern analyzed against our implementation
- `2026-04-13` [**Vietnamese diaspora synthesis**](diaspora/vietnamese-diaspora-synthesis.md) - First synthesis page: 7 notes woven into a structural argument
- `2026-04-13` [The Munger Operating System for life](life/munger-operating-system.md) - Charlie Munger's 16 life principles from his USC commencement speech

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
| [engineering/](engineering/) | 107 | Software engineering practices, principles, languages |
| [life/](life/) | 25 | Life philosophy, habits, mindset, career wisdom |
| [leadership/](leadership/) | 16 | Management, negotiation, business leadership |
| [cs/](cs/) | 14 | Computer science fundamentals |
| [dwarves-kit/](dwarves-kit/) | 9 | Dwarves Kit architecture and design |
| [hiring/](hiring/) | 9 | Candidate assessment and hiring |
| [ai-tooling/](ai-tooling/) | 8 | AI developer tools and evaluations |
| [ai/](ai/) | 8 | AI concepts, memory systems, agent patterns |
| [diaspora/](diaspora/) | 6 | Vietnamese and Asian diaspora analysis |
| [history/](history/) | 6 | History, civilizations, geopolitical patterns |
| [claude-code/](claude-code/) | 5 | Claude Code hooks, skills, workflows |
| [crypto/](crypto/) | 5 | Cryptocurrency, blockchain, tokenomics |
| [wealth/](wealth/) | 5 | Trust-building, business relationships |
| [geopolitics/](geopolitics/) | 3 | Oil crises, government structures |
| [startup/](startup/) | 3 | Startup strategy, product focus |
| [health/](health/) | 2 | Wellness, nutrition |
| [investing/](investing/) | 2 | Compound interest, startup investing |
| [mcp/](mcp/) | 2 | Model Context Protocol |
| [pkm/](pkm/) | 2 | Personal knowledge management |
| [devtools/](devtools/) | 2 | Developer tools and config |
| [patterns/](patterns/) | 2 | Software patterns and anti-patterns |
| [youtube/](youtube/) | 1 | YouTube tooling |

## Documentation

- [Full note index](index.md) - catalog of all 242 notes with one-line summaries
- [Usage guide](_docs/guide.md) - how to add notes, use Obsidian, work with Claude
- [Architecture](_docs/architecture.md) - system design, folder conventions, operations model
- [Requirements](_docs/requirements.md) - feature tracker, design principles, scaling triggers
- [Changelog](_docs/changelog.md) - project decisions log
- [Operations log](log.md) - chronological record of ingests, queries, lints
