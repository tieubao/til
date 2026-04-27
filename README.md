# Learned

A personal knowledge base following the [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) methodology, maintained by LLMs using the [LLM Wiki](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) pattern.

Interlinked notes organized by topic. Browse in [Obsidian](https://obsidian.md/) for graph view and backlinks, or read the [full index](index.md).

## Recent additions

- `2026-04-27` [Optimization as the bridge to computational finance](notes/comp-fin/optimization-as-the-bridge-to-computational-finance.md) - Comp-fin rests on three pillars (stochastic calculus, numerical methods, optimization); convex opt + DP + stochastic control are the workhorses, not MILP
- `2026-04-27` [Operations Research and MILP for software engineers](notes/optimization/operations-research-and-milp-for-software-engineers.md) - OR is declarative problem solving with a solver; MILP is one technique inside mathematical optimization, the rest is modeling
- `2026-04-27` [macOS Input Method Kit (IMK) architecture and lifecycle](notes/macos/macos-input-method-kit-imk-architecture-and-lifecycle.md) - Out-of-process IM model, Mach IPC keystroke routing, Secure Input pain points
- `2026-04-25` [Claude Code surfaces - CLI vs web vs desktop and resource usage](notes/claude-code/claude-code-surfaces-cli-vs-web-vs-desktop-and-resource-usage.md) - Four surfaces, three runtimes; desktop Electron costs ~2x RAM and runs hot, CLI + web is the leaner pattern
- `2026-04-25` [How to win at office politics (BusinessCringe)](notes/career/how-to-win-at-office-politics-businesscringe.md) - The invisible scoreboard runs on perception, not performance; can't opt out, three offensive tactics to defend against, three defensive plays to run
- `2026-04-24` [age and 1password as complementary encryption tiers](notes/engineering/architecture/age-and-1password-complementary-encryption-tiers.md) - Two-tier backup: 1P for availability, age for offline recovery; key-separation principle (key and ciphertext must have different failure modes)
- `2026-04-22` [age, a modern file-encryption CLI](notes/devtools/age-modern-file-encryption-cli.md) - Small opinionated replacement for GPG-for-files; native SOPS backend, SSH-key identities, no keyring state
- `2026-04-22` [Cloudflare Workers as a monitoring backend for self-hosted Linux](notes/engineering/architecture/cloudflare-workers-as-monitoring-backend-for-self-hosted-linux.md) - Agent + Worker + D1/KV free tier beats Kuma/Netdata/Prometheus for the 1-20 host solo operator
- `2026-04-22` [HIDS-lite rule set for a single-operator Linux VPS](notes/engineering/architecture/hids-lite-rule-set-for-single-operator-vps.md) - 15 rules over cheap shell signals catch 80% of script-kiddie-tier compromises; out-of-band engine required
- `2026-04-21` [Leadership in the agentic era](notes/leadership/leadership-in-the-agentic-era.md) - When agents absorb execution capacity, taste + trust + context become the scarce leadership work; coordination time collapses
- `2026-04-21` [Tao Te Ching + I Ching on timing and hidden preparation](notes/philosophy/tao-te-ching-i-ching-on-timing-and-hidden-preparation.md) - The hidden dragon still trains: wu wei is preparation during a red light, not idleness
- `2026-04-20` [GeckoTerminal API evaluation](notes/finance-tooling/geckoterminal-evaluation.md) - Keyless H1/H4 OHLCV for DEX pools; 14/15 ADOPT; same data as $129/mo CoinGecko Pro
- `2026-04-19` [Static outbound IP solutions for crypto trading bots, ten options compared](notes/finance-tooling/static-ip-solutions-compared-for-trading-bots.md) - 10 options across 5 categories; every commercial static-IP service re-bundles "rent a VPS" with markup
- `2026-04-19` [Why rotating ISP IPs break Binance API keys, and how to fix it with WireGuard](notes/finance-tooling/wireguard-static-ip-exchange-whitelist.md) - Cheap VPS + WireGuard beats every bundled static-IP product for exchange whitelisting
- `2026-04-19` [deepagents vs OpenClaw vs Hermes: category positioning](notes/ai-tooling/deepagents-vs-openclaw-vs-hermes-category-positioning.md) - Library vs runtime distinction; the three are not peers and stack rather than compete
- `2026-04-19` [AI tooling stack synthesis April 2026](notes/ai-tooling/ai-tooling-stack-synthesis-april-2026.md) - Synthesis: 3 layers wired through one rubric; growth and adoption-readiness are inversely correlated
- `2026-04-19` [OpenBB evaluation](notes/finance-tooling/openbb-evaluation.md) - Python-first financial SDK; 11/15 BOOKMARK; crypto value thin, TradFi inflection point
- `2026-04-19` [OSS trading stack survey, April 2026](notes/finance-tooling/oss-trading-stack-survey-april-2026.md) - 3-category synthesis; Freqtrade + VectorBT canonical for semi-pro crypto; ai-hedge-fund as first agentic pilot

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
| [engineering/](notes/engineering/) | Software engineering practices, principles, languages |
| [life/](notes/life/) | Life philosophy, habits, mindset, career wisdom |
| [leadership/](notes/leadership/) | Management, negotiation, business leadership |
| [cs/](notes/cs/) | Computer science fundamentals |
| [dwarves-kit/](notes/dwarves-kit/) | Dwarves Kit architecture and design |
| [hiring/](notes/hiring/) | Candidate assessment and hiring |
| [ai-tooling/](notes/ai-tooling/) | AI developer tools and evaluations |
| [ai/](notes/ai/) | AI concepts, memory systems, agent patterns |
| [diaspora/](notes/diaspora/) | Vietnamese and Asian diaspora analysis |
| [history/](notes/history/) | History, civilizations, geopolitical patterns |
| [claude-code/](notes/claude-code/) | Claude Code hooks, skills, workflows |
| [crypto/](notes/crypto/) | Cryptocurrency, blockchain, DeFi, tokenomics |
| [wealth/](notes/wealth/) | Trust-building, business relationships |
| [geopolitics/](notes/geopolitics/) | Oil crises, government structures |
| [startup/](notes/startup/) | Startup strategy, product focus |
| [health/](notes/health/) | Wellness, nutrition |
| [investing/](notes/investing/) | Compound interest, startup investing |
| [mcp/](notes/mcp/) | Model Context Protocol |
| [pkm/](notes/pkm/) | Personal knowledge management |
| [devtools/](notes/devtools/) | Developer tools and config |
| [patterns/](notes/patterns/) | Software patterns and anti-patterns |
| [philosophy/](notes/philosophy/) | Taoism, I Ching, timing and preparation |
| [finance/](notes/finance/) | Bond markets, capital structure, compounding knowledge |
| [finance-tooling/](notes/finance-tooling/) | Financial tool evaluations: terminals, data providers, broker platforms, frameworks |
| [comp-fin/](notes/comp-fin/) | Computational finance: optimization, stochastic control, learning paths |
| [optimization/](notes/optimization/) | Operations research, MILP, mathematical optimization fundamentals |
| [career/](notes/career/) | Career strategy, workplace dynamics, office politics |
| [macos/](notes/macos/) | macOS frameworks and platform-specific architecture |
| [youtube/](notes/youtube/) | YouTube tooling |
| [zed/](notes/zed/) | Zed editor agent rules and configuration |

## Documentation

- [Full note index](index.md) - catalog of all notes with one-line summaries
- [Usage guide](_docs/guide.md) - how to add notes, use Obsidian, work with Claude
- [Architecture](_docs/architecture.md) - system design, folder conventions, operations model
- [Requirements](_docs/requirements.md) - feature tracker, design principles, scaling triggers
- [Changelog](_docs/changelog.md) - project decisions log
- [Operations log](log.md) - chronological record of ingests, queries, lints
