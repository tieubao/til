# Learned

A personal knowledge base following the [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) methodology, maintained by LLMs using the [LLM Wiki](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) pattern.

Interlinked notes organized by topic. Browse in [Obsidian](https://obsidian.md/) for graph view and backlinks, or read the [full index](index.md).

## Recent additions

- `2026-05-17` [Why Quantum Computing Talks About Decision Problems](notes/quantum/why-quantum-computing-talks-about-decision-problems.md) - Decision problems are the assembly language of complexity theory; YES/NO falls out of the Born rule; apples-to-apples quantum-vs-classical comparison requires shared output shape
- `2026-05-17` [History and motivation of major quantum algorithms](notes/quantum/history-and-motivation-of-major-quantum-algorithms.md) - Shor / Grover / HHL / VQE-QAOA as a story of outsiders importing intuition from other fields; speedup requires structure; HHL dequantization shows caveats are the whole story
- `2026-05-17` [State preparation is half the quantum algorithm](notes/quantum/state-preparation-is-half-the-quantum-algorithm.md) - Every algorithm has three stages; preparation is where exponential speedup claims die; the headlines focus on operations, the engineering reality lives in encoding
- `2026-05-17` [Quantum superposition state and QFT for beginners](notes/quantum/quantum-superposition-state-and-qft-for-beginners.md) - Quantum speedup is not "try all options in parallel"; it's interference canceling wrong answers; QFT is the engine inside Shor that extracts hidden periods
- `2026-05-17` [Complexity classes P NP BQP QMA explained](notes/quantum/complexity-classes-p-np-bqp-qma-explained.md) - Classical P/NP hierarchy extended with BQP and QMA; Shor lands factoring in BQP, P vs NP and BQP ⊆ NP both open; RSA's safety rests on empirical P ≠ NP
- `2026-05-17` [What polynomial time actually means](notes/quantum/what-polynomial-time-actually-means.md) - Polynomial = highest exponent is a fixed constant; n^10 is polynomial, 2^n is not; the threshold complexity theory draws between tractable and intractable
- `2026-05-17` [Vietnamese terminology for quantum computing](notes/quantum/vietnamese-terminology-for-quantum-computing.md) - Working glossary: chồng chập for superposition, vướng víu vs rối for entanglement, when to keep English (algorithm names, acronyms)
- `2026-05-17` [Monomial polynomial term Vietnamese terminology breakdown](notes/math/monomial-polynomial-term-vietnamese-terminology-breakdown.md) - `thức` in `đơn thức` is NOT `công thức`; etymology of `-nomial` from Latin `nomen`; Ising Hamiltonians as degree-2 polynomials in Pauli operators
- `2026-05-15` [iCloud Advanced Data Protection: coverage, exclusions, and recovery model](notes/security/macos-icloud-advanced-data-protection-coverage-and-recovery.md) - ADP extends E2EE to almost everything in iCloud, but Mail/Contacts/Calendar stay Apple-readable forever; three-axis decision (coverage, recovery, operations) instead of one binary switch
- `2026-05-09` [Hermes Agent v0.13.0 release evaluation: top features ranked for 3-tier ecosystem](notes/ai-tooling/hermes-agent-v0-13-0-release-evaluation-top-features-ranked-for-3-tier-ecosystem.md) - Selective adoption verdict; CLONE Kanban + cron no_agent, ADOPT /goal + session resume, STUDY security wave; skip i18n / Google Chat / video tool hype

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
| [youtube/](notes/youtube/) | YouTube tooling |
| [zed/](notes/zed/) | Zed editor agent rules and configuration |

## Documentation

- [Full note index](index.md) - catalog of all notes with one-line summaries
- [Usage guide](_docs/guide.md) - how to add notes, use Obsidian, work with Claude
- [Architecture](_docs/architecture.md) - system design, folder conventions, operations model
- [Requirements](_docs/requirements.md) - feature tracker, design principles, scaling triggers
- [Changelog](_docs/changelog.md) - project decisions log
- [Operations log](log.md) - chronological record of ingests, queries, lints
