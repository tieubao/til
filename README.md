# Learned

A personal knowledge base following the [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) methodology, maintained by LLMs using the [LLM Wiki](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) pattern.

Interlinked notes organized by topic. Browse in [Obsidian](https://obsidian.md/) for graph view and backlinks, or read the [full index](index.md).

## Recent additions

- `2026-04-19` [How LLM agents do web research: the ReAct loop](ai/how-llm-agents-do-web-research-the-react-loop.md) - Agent research is a ReAct loop, not an algorithm; biggest failure is under-weighting Reddit/HN/Twitter
- `2026-04-18` [Hermes Agent comprehensive briefing April 2026](ai-tooling/hermes-agent-comprehensive-briefing-april-2026.md) - Nous Research's self-hosted agent with auto-generated skills; 0 to 95.6K stars in seven weeks
- `2026-04-18` [Hermes vs OpenClaw competitive scene](ai-tooling/hermes-vs-openclaw-competitive-scene-april-2026.md) - OpenClaw wins metrics, Hermes wins narrative; equilibrium is to run both and bridge via ACP
- `2026-04-18` [Why developers migrate to Hermes, real vs hype](ai-tooling/why-developers-migrate-to-hermes-ranked-real-vs-hype.md) - Push factor (CVEs + subscription cliff) beats pull factor; steal the auto-skill pattern
- `2026-04-18` [OpenClaw virtual company pattern](ai-tooling/openclaw-virtual-company-pattern.md) - The "CEO/CTO/PM" multi-agent idiom is a convention, not a feature; six failure modes
- `2026-04-18` [OpenClaw multi-persona dev team setup playbook](ai-tooling/openclaw-multi-persona-dev-team-setup-playbook.md) - End-to-end config for a Telegram-led PM/Engineer/QA team on Docker
- `2026-04-18` [TurboQuant KV cache compression](ai/turboquant-kv-cache-compression.md) - Random rotation + two-stage quantizer cuts KV cache to 3-4 bits with unbiased inner products
- `2026-04-17` [Zed global agent rules live in the Rules Library](zed/zed-global-agent-rules-live-in-the-rules-library-not-agents-md.md) - No global file escape hatch; user-scope rules live in an LMDB database
- `2026-04-14` [Transformer internals: FFN as graph database (LARQL)](ai/transformer-internals-for-software-engineers-ffn-as-graph-database-larql.md) - FFN as sparse KNN lookup over ~348K "edges"; factual knowledge editable without retraining
- `2026-04-14` [How the bond market controls housing, stocks, and jobs](finance/how-the-bond-market-controls-housing-stocks-and-jobs.md) - Yield seesaw sets mortgage rates, ERP, and corporate refinancing costs; one chain from the 10-year

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

| Folder | Domain |
|--------|--------|
| [engineering/](engineering/) | Software engineering practices, principles, languages |
| [life/](life/) | Life philosophy, habits, mindset, career wisdom |
| [leadership/](leadership/) | Management, negotiation, business leadership |
| [cs/](cs/) | Computer science fundamentals |
| [dwarves-kit/](dwarves-kit/) | Dwarves Kit architecture and design |
| [hiring/](hiring/) | Candidate assessment and hiring |
| [ai-tooling/](ai-tooling/) | AI developer tools and evaluations |
| [ai/](ai/) | AI concepts, memory systems, agent patterns |
| [diaspora/](diaspora/) | Vietnamese and Asian diaspora analysis |
| [history/](history/) | History, civilizations, geopolitical patterns |
| [claude-code/](claude-code/) | Claude Code hooks, skills, workflows |
| [crypto/](crypto/) | Cryptocurrency, blockchain, DeFi, tokenomics |
| [wealth/](wealth/) | Trust-building, business relationships |
| [geopolitics/](geopolitics/) | Oil crises, government structures |
| [startup/](startup/) | Startup strategy, product focus |
| [health/](health/) | Wellness, nutrition |
| [investing/](investing/) | Compound interest, startup investing |
| [mcp/](mcp/) | Model Context Protocol |
| [pkm/](pkm/) | Personal knowledge management |
| [devtools/](devtools/) | Developer tools and config |
| [patterns/](patterns/) | Software patterns and anti-patterns |
| [finance/](finance/) | Bond markets, capital structure, compounding knowledge |
| [youtube/](youtube/) | YouTube tooling |
| [zed/](zed/) | Zed editor agent rules and configuration |

## Documentation

- [Full note index](index.md) - catalog of all notes with one-line summaries
- [Usage guide](_docs/guide.md) - how to add notes, use Obsidian, work with Claude
- [Architecture](_docs/architecture.md) - system design, folder conventions, operations model
- [Requirements](_docs/requirements.md) - feature tracker, design principles, scaling triggers
- [Changelog](_docs/changelog.md) - project decisions log
- [Operations log](log.md) - chronological record of ingests, queries, lints
