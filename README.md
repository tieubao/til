# Learned

A personal knowledge base following the [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) methodology, maintained by LLMs using the [LLM Wiki](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) pattern.

Interlinked notes organized by topic. Browse in [Obsidian](https://obsidian.md/) for graph view and backlinks, or read the [full index](index.md).

## Recent additions

- `2026-09-17` [An upstream identifier can be duplicated](notes/patterns/an-upstream-identifier-can-be-duplicated.md) - A source's own printed identifier is not a uniqueness guarantee; prefer a key the traversal itself produces
- `2026-09-17` [An incremental job must record confirmed absent](notes/patterns/an-incremental-job-must-record-confirmed-absent.md) - A fetch that returned nothing is a result; collapsing it into "not yet done" makes a job rescan its finished backlog forever
- `2026-09-17` [A prompt constraint is a request, not an enforcement](notes/ai-tooling/a-prompt-constraint-is-a-request-not-an-enforcement.md) - A rule in the prompt sets a probability; only a validator on the output sets a floor
- `2026-09-17` [Skill selection runs on the description text alone](notes/claude-code/skill-selection-runs-on-the-description-text-alone.md) - Only the description text is read when a skill competes with a command; write it as a router entry, not a label
- `2026-09-17` [wrangler --env deploys a second worker when no env block exists](notes/cloudflare/wrangler-env-flag-deploys-a-second-worker.md) - Wrangler synthesises and deploys a stray `<name>-<env>` Worker instead of erroring
- `2026-09-17` [Push plus reconcile beats polling an address pool](notes/crypto/push-plus-reconcile-beats-polling-an-address-pool.md) - Push handles the normal case at cost proportional to real deposits; a slow idempotent reconcile sweep covers what the push drops
- `2026-09-17` [A scripted click carries no user activation](notes/browser-automation/a-scripted-click-carries-no-user-activation.md) - `element.click()` over CDP fires the handler but carries no transient user activation; dispatch at the input layer with coordinates instead
- `2026-09-17` [Optimize Mac Storage turns disk pressure into an iCloud loop](notes/macos/optimize-mac-storage-turns-disk-pressure-into-an-icloud-loop.md) - Disk pressure evicts iCloud files to dataless stubs; a periodic job that reads them re-downloads the same bytes forever
- `2026-09-17` [Cargo and rustup directories are not caches](notes/devtools/cargo-and-rustup-directories-are-not-caches.md) - `~/.cargo/bin` and `~/.rustup` hold installs, not caches; check for a `bin/` and PATH resolution before deleting
- `2026-09-17` [An escaped delimiter is still the delimiter byte](notes/engineering/code-quality/an-escaped-delimiter-is-still-the-delimiter-byte.md) - `\|` in a markdown cell is a rendering convention, not a transformation; a byte-level split still splits on it
- `2026-05-25` [LKY on why Singapore can never build a Google: Vietnam comparison](notes/vietnam/lky-on-why-singapore-can-never-build-a-google-vietnam-comparison.md) - LKY's five constraints (size, brain drain, Confucian culture, comfort, takeovers); Vietnam inverts market size and risk culture, mirrors brain drain and scholar pull, lacks rule of law and capital

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
| [cloudflare/](notes/cloudflare/) | Cloudflare Workers, Durable Objects, wrangler operational gotchas |
| [browser-automation/](notes/browser-automation/) | CDP and browser-automation debugging |
| [ci/](notes/ci/) | CI pipeline behavior, GitHub Actions gotchas |

## Documentation

- [Full note index](index.md) - catalog of all notes with one-line summaries
- [Usage guide](_docs/guide.md) - how to add notes, use Obsidian, work with Claude
- [Architecture](_docs/architecture.md) - system design, folder conventions, operations model
- [Requirements](_docs/requirements.md) - feature tracker, design principles, scaling triggers
- [Changelog](_docs/changelog.md) - project decisions log
- [Operations log](log.md) - chronological record of ingests, queries, lints
