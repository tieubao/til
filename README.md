# Learned

A personal knowledge base following the [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) methodology, maintained by LLMs using the [LLM Wiki](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) pattern.

Interlinked notes organized by topic. Browse in [Obsidian](https://obsidian.md/) for graph view and backlinks, or read the [full index](index.md).

## Recent additions

- `2026-05-26` [LKY operating system: how to pick what to work on](notes/vietnam/lky-operating-system-how-to-pick-what-to-work-on.md) - Mid-career operating system applied to Vietnamese founder: six mental models, eight-question filter, concentration over portfolio; the 2055 question overrides everything
- `2026-05-26` [The capital portfolio framework: beyond money](notes/vietnam/the-capital-portfolio-framework-beyond-money.md) - Seven capital forms (economic, trust, time, knowledge, network, symbolic, optionality); time is the only irreplaceable input; mid-career work is mostly withdrawal-prevention
- `2026-05-26` [The Greek prefix para- means beside](notes/etymology/the-greek-prefix-para-means-beside.md) - `para-` = beside; paragraph was originally the margin stroke beside the text, not the text block; "beside what?" unlocks paramedic / paranormal / paradox / parasite
- `2026-05-25` [LKY on why Singapore can never build a Google: Vietnam comparison](notes/vietnam/lky-on-why-singapore-can-never-build-a-google-vietnam-comparison.md) - LKY's five constraints (size, brain drain, Confucian culture, comfort, takeovers); Vietnam inverts market size and risk culture, mirrors brain drain and scholar pull, lacks rule of law and capital
- `2026-05-25` [What young Vietnamese entrepreneurs should learn from LKY](notes/vietnam/what-young-vietnamese-entrepreneurs-should-learn-from-lky.md) - LKY-flavored founder advice for VN 20s-30s; drop the billion-dollar framing, pick a defensible niche, Singapore as legal venue, plan for acquisition exit
- `2026-05-24` [Managing Claude Code's agent view (background sessions)](notes/claude-code/managing-claude-codes-agent-view-background-sessions.md) - TUI lifecycle, 30-day retention, the worktree-delete gotcha; do not micromanage, name jobs at dispatch and sweep orphaned worktrees
- `2026-05-23` [Da Nang's historical names: Tourane and Dogpatch](notes/history/da-nangs-historical-names-tourane-and-dogpatch.md) - Two outsider names, neither used by locals; Tourane was the French transliteration of Cửa Hàn, Dogpatch was GI slang from Al Capp's Li'l Abner
- `2026-05-18` [Claude integration with Jupyter notebooks](notes/jupyter/claude-integration-with-jupyter-notebooks.md) - Three integration paths (NotebookEdit / jupyter-ai or NBI / Jupyter MCP); MCP turns Claude into a real notebook agent that can hit errors, install packages, and re-run
- `2026-05-18` [Notebook landscape 2026: Jupyter alternatives and competitors](notes/jupyter/notebook-landscape-2026-jupyter-alternatives-and-competitors.md) - Three camps (Jupyter family, commercial cloud, post-Jupyter reactive); Marimo is the post-Jupyter answer worth migrating to for new work
- `2026-05-18` [Jupyter architecture: kernel, server, frontend](notes/jupyter/jupyter-architecture-kernel-server-frontend.md) - Three processes (frontend/server/kernel) over HTTP+WebSocket and ZeroMQ; kernel state plus arbitrary cell order is Jupyter's reproducibility footgun

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
| [coding-agents/](notes/coding-agents/) | Agent-laptop ergonomics: sandboxing, opt-in design, integration tradeoffs |
| [agentkernel/](notes/agentkernel/) | agentkernel CLI gotchas and operational notes |
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
| [local-llm/](notes/local-llm/) | Local LLM economics, Ollama Cloud, Qwen on Apple Silicon |
| [networking/](notes/networking/) | Tailscale, WireGuard, portless, mesh VPN ergonomics |
| [security/](notes/security/) | Threat modeling for AI agents, sandboxing trade-offs |
| [decentralized/](notes/decentralized/) | Decentralized git collaboration, p2p protocols |
| [math/](notes/math/) | Math vocabulary and notation, Vietnamese-English mapping for QC and comp-fin |
| [quantum/](notes/quantum/) | Quantum computing fundamentals: complexity classes, algorithms, state preparation, Vietnamese terms |
| [jupyter/](notes/jupyter/) | Jupyter architecture, usage patterns, Claude integration paths, post-Jupyter reactive notebooks |
| [vietnam/](notes/vietnam/) | LKY framework applied to Vietnamese founders and operators; capital portfolio across seven forms |
| [etymology/](notes/etymology/) | Word origins and prefix decompositions |
| [youtube/](notes/youtube/) | YouTube tooling |
| [zed/](notes/zed/) | Zed editor agent rules and configuration |

## Documentation

- [Full note index](index.md) - catalog of all notes with one-line summaries
- [Usage guide](_docs/guide.md) - how to add notes, use Obsidian, work with Claude
- [Architecture](_docs/architecture.md) - system design, folder conventions, operations model
- [Requirements](_docs/requirements.md) - feature tracker, design principles, scaling triggers
- [Changelog](_docs/changelog.md) - project decisions log
- [Operations log](log.md) - chronological record of ingests, queries, lints
