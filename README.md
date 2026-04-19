# Learned

A personal knowledge base following the [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) methodology, maintained by LLMs using the [LLM Wiki](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) pattern.

Interlinked notes organized by topic. Browse in [Obsidian](https://obsidian.md/) for graph view and backlinks, or read the [full index](index.md).

## Recent additions

- `2026-04-19` [AI tooling stack synthesis April 2026](ai-tooling/ai-tooling-stack-synthesis-april-2026.md) - Synthesis: 3 layers wired through one rubric; growth and adoption-readiness are inversely correlated
- `2026-04-19` [LLM agent memory synthesis April 2026](ai/llm-agent-memory-synthesis-april-2026.md) - Synthesis: 5-stage pipeline + 3 battlegrounds + harness hooks form one stack with a broken evaluation floor
- `2026-04-19` [OpenBB evaluation](finance-tooling/openbb-evaluation.md) - Python-first financial SDK; 11/15 BOOKMARK; crypto value thin, TradFi inflection point
- `2026-04-19` [OSS trading stack survey, April 2026](finance-tooling/oss-trading-stack-survey-april-2026.md) - 3-category synthesis; Freqtrade + VectorBT canonical for semi-pro crypto; ai-hedge-fund as first agentic pilot
- `2026-04-19` [FinceptTerminal evaluation](finance-tooling/fincept-terminal-evaluation.md) - AGPL-3 Qt6 Bloomberg-alternative; 10/15 BOOKMARK; §13 blocks integration into any network-facing trading stack
- `2026-04-19` [How LLM agents do web research: the ReAct loop](ai/how-llm-agents-do-web-research-the-react-loop.md) - Agent research is a ReAct loop, not an algorithm; biggest failure is under-weighting Reddit/HN/Twitter
- `2026-04-18` [Hermes Agent comprehensive briefing April 2026](ai-tooling/hermes-agent-comprehensive-briefing-april-2026.md) - Nous Research's self-hosted agent with auto-generated skills; 0 to 95.6K stars in seven weeks
- `2026-04-18` [Hermes vs OpenClaw competitive scene](ai-tooling/hermes-vs-openclaw-competitive-scene-april-2026.md) - OpenClaw wins metrics, Hermes wins narrative; equilibrium is to run both and bridge via ACP
- `2026-04-18` [Why developers migrate to Hermes, real vs hype](ai-tooling/why-developers-migrate-to-hermes-ranked-real-vs-hype.md) - Push factor (CVEs + subscription cliff) beats pull factor; steal the auto-skill pattern
- `2026-04-18` [OpenClaw virtual company pattern](ai-tooling/openclaw-virtual-company-pattern.md) - The "CEO/CTO/PM" multi-agent idiom is a convention, not a feature; six failure modes
- `2026-04-18` [OpenClaw multi-persona dev team setup playbook](ai-tooling/openclaw-multi-persona-dev-team-setup-playbook.md) - End-to-end config for a Telegram-led PM/Engineer/QA team on Docker
- `2026-04-18` [TurboQuant KV cache compression](ai/turboquant-kv-cache-compression.md) - Random rotation + two-stage quantizer cuts KV cache to 3-4 bits with unbiased inner products

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

## Commands

Type these to Claude in this repo (exact wording doesn't matter, intent does):

| Command | What it does |
|---------|--------------|
| `process inbox` | Refine raw `_inbox/` captures into the right folder with links |
| `capture this as a note: ...` | Save a learning moment from the current chat |
| `compile recent commits` / `re-ingest` | Run compilation on notes pushed directly to GitHub (backlinks, index, README, log) |
| `answer from the wiki: ...` | Synthesize across notes, cite with `[[wikilinks]]` |
| `file that answer as a wiki page` | Commit a synthesis from chat as a new note |
| `reorganize the wiki` | Clean up orphans, folders, backlinks |
| `lint the wiki` | Health check: orphans, broken links, raw stragglers |

Full reference: [commands cheatsheet in the usage guide](_docs/guide.md#commands-cheatsheet).

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
| [finance-tooling/](finance-tooling/) | Financial tool evaluations: terminals, data providers, broker platforms, frameworks |
| [youtube/](youtube/) | YouTube tooling |
| [zed/](zed/) | Zed editor agent rules and configuration |

## Documentation

- [Full note index](index.md) - catalog of all notes with one-line summaries
- [Usage guide](_docs/guide.md) - how to add notes, use Obsidian, work with Claude
- [Architecture](_docs/architecture.md) - system design, folder conventions, operations model
- [Requirements](_docs/requirements.md) - feature tracker, design principles, scaling triggers
- [Changelog](_docs/changelog.md) - project decisions log
- [Operations log](log.md) - chronological record of ingests, queries, lints
