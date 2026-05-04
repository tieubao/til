# Learned

A personal knowledge base following the [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) methodology, maintained by LLMs using the [LLM Wiki](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) pattern.

Interlinked notes organized by topic. Browse in [Obsidian](https://obsidian.md/) for graph view and backlinks, or read the [full index](index.md).

## Recent additions

- `2026-05-04` [Opt-in beats all-in for coding-agent sandboxing on a developer laptop](notes/coding-agents/opt-in-beats-all-in-for-coding-agent-sandboxing.md) - Wrap-every-call sandboxing kills adoption; per-trigger opt-in survives because the host integrations Claude Code relies on are exactly what doesn't work in a sandbox
- `2026-05-04` [Threat-model split: cross-tenant isolation vs per-agent damage containment](notes/security/threat-model-split-cross-tenant-isolation-vs-per-agent-damage-containment.md) - Two threats around AI-agent sandboxing look similar and need different solutions; "isolate from whom?" splits the conflation
- `2026-05-04` [Apple Containers: the macOS-native microVM runtime](notes/macos/apple-containers-overview-the-macos-native-microvm-runtime.md) - apple/container runs each container as its own Linux VM via the Apple Virtualization framework; OCI-compatible, macOS 26+ Apple Silicon only, pre-1.0
- `2026-05-04` [agentkernel --no-network, --dir, --secret-file silently no-op on Apple Containers](notes/agentkernel/agentkernel-broken-flags-on-apple-containers.md) - Three documented isolation flags accept input and have zero effect on v0.16.0/v0.18.1 with Apple Containers backend; default isolation still works
- `2026-05-04` [macOS multi-user cost myth: it's the GUI session that's heavy, not the user](notes/macos/macos-multi-user-cost-myth-gui-vs-service-users.md) - 161 system service users coexist on one laptop for ~935 MB; multi-user GUI is heavy, multi-user services is essentially free
- `2026-05-02` [Vibe-Trading evaluation](notes/finance-tooling/vibe-trading-evaluation.md) - HKUDS multi-agent finance research workspace; 11/15 BOOKMARK; published vibe-trading-mcp is the highest-leverage entry point for Claude-Code-native users
- `2026-04-30` [portless competitive landscape: no exact 1-to-1 competitor](notes/networking/portless-competitive-landscape-no-exact-1-to-1-competitor.md) - Quadrant map across reverse proxies, tunnels, and Tailscale; portless wins by being the only tool that explicitly aimed at the monorepo .localhost niche
- `2026-04-29` [Tailscale VPN On Demand feature overview and rule semantics](notes/networking/tailscale-vpn-on-demand-feature-overview-and-rule-semantics.md) - iOS/macOS-only auto-connect on network change; "Except On home_wifi" + Cellular "Always" eliminates the "is Tailscale on?" cognitive overhead
- `2026-04-29` [Radicle network: peer-to-peer git collaboration](notes/decentralized/radicle-network-peer-to-peer-git-collaboration-explained.md) - Cryptographic-quorum canonical branch (no merge button on a server); CRDT-based Collaborative Objects store issues and patches in plain Git
- `2026-04-29` [chezmoi source vs target two-layer mental model](notes/devtools/chezmoi-source-vs-target-two-layer-mental-model.md) - Source is the spec (`~/.local/share/chezmoi`), target is the build artifact (`~`); four verbs traverse the gap

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
| [youtube/](notes/youtube/) | YouTube tooling |
| [zed/](notes/zed/) | Zed editor agent rules and configuration |

## Documentation

- [Full note index](index.md) - catalog of all notes with one-line summaries
- [Usage guide](_docs/guide.md) - how to add notes, use Obsidian, work with Claude
- [Architecture](_docs/architecture.md) - system design, folder conventions, operations model
- [Requirements](_docs/requirements.md) - feature tracker, design principles, scaling triggers
- [Changelog](_docs/changelog.md) - project decisions log
- [Operations log](log.md) - chronological record of ingests, queries, lints
