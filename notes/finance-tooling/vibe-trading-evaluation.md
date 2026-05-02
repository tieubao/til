---
title: "Vibe-Trading evaluation"
date: 2026-05-02
captured: 2026-05-02T00:00:00.000Z
tags: ["finance-tooling", "evaluation", "ai-agent", "multi-agent", "crypto", "backtesting", "mcp", "langgraph", "hkuds"]
source: "Claude Code research session evaluating HKUDS/Vibe-Trading as a candidate research-agent layer for a semi-pro crypto trading workflow"
aliases: ["Vibe-Trading", "Vibe Trading", "HKUDS Vibe-Trading", "vibe-trading-ai", "vibe-trading-mcp"]
status: refined
---

## What it is

Open-source multi-agent finance research workspace built around natural-language requests. You describe an idea ("scan funding rate dislocations on majors, suggest a carry pair"); a LangGraph-based ReAct agent loop loads the relevant skill from a 72-skill bundle, calls data-source tools, runs a backtest if appropriate, and writes a report. Pitched as "one command to empower your agent with comprehensive trading capabilities."

Authored by **HKUDS** (Hong Kong University Data Science lab; same group behind several well-cited LLM / RecSys repos). Created 2026-04-01, MIT license. As of May 2026: ~4,250 stars, ~870 forks, 92 commits, 15 contributors, 3 releases, ~24 PRs. Star velocity is high, and commit cadence + contributor count back up the hype.

Three distribution surfaces:
- PyPI: `pip install vibe-trading-ai`
- Docker: `docker-compose.yml` shipped
- **MCP server**: entry point `vibe-trading-mcp`, exposes ~21 read-only research tools to any MCP-aware client (Claude Code, Cursor, etc.)

Stack: Python 3.11+, LangChain + LangGraph (agent loop and DAG orchestration), FastAPI backend, React 19 streaming frontend, DuckDB embedded for analytics, FTS5-backed cross-session memory. Headline feature counts: **6 data sources** (Tushare, AKShare, yfinance, OKX, CCXT, Futu; heavy A-share / HK-equity bias), **29 swarm presets** (`investment_committee`, `crypto_trading_desk`, `risk_committee`, etc.), **72 skills** (mostly markdown documentation; few are executable code), **13 LLM providers** (default: `deepseek/deepseek-v3.2`).

**Critical scope disclaimer from their own README**: "for research, simulation, and backtesting only. It does not execute live trades." No broker integration, no order placement. The MCP server confirms this: 0 of 21 tools touch order execution.

Skill format is **single `SKILL.md` per directory with YAML frontmatter** (`name`, `description`, `category`). Skills are prose-and-pseudocode, not executable Python; they ride on the LangGraph runtime and are loaded by the agent loop. This makes them trivially liftable into any Claude-Code-skills-shaped harness as research lenses.

## 5-question rubric

**Q1 Layer (3/3)**: Clean fit. Sits at the agent-runtime layer (L5 in the 8-layer dev-stack model) plus the finance-domain-skills layer (L7). Category peers: `OpenBB`, `FinceptTerminal`, `nautilus_trader` (research mode), `ai-hedge-fund` (`virattt/ai-hedge-fund`). Differentiator: bundled multi-agent presets and a published MCP server.

**Q2 Replace or complement? (2/3)**: For a Claude-Code-native user, the MCP server complements (adds 21 finance research tools to your existing harness with zero stack overhead). For a LangChain-native user, it could replace a hand-rolled finance research agent. Where it does **not** replace: any execution layer; any strategy-authoring discipline (their `/code <run_id>` exports LLM-written Python, which conflicts with any "human-in-the-loop strategy" guardrail). The "is it 3x better than what you have" test fails for most users; the "does it plug a felt gap" test passes weakly via the MCP server only.

**Q3 Credibility (2/3)**: Real research lab, real velocity (3 commits / day, 15 contributors, 3 releases in 30 days). MIT-licensed. But: **1 month old**, **no benchmarks, no academic paper, no Sharpe / win-rate claims**, and the default LLM is **DeepSeek** (Chinese provider; fine for technical capability, surprising as a default for finance research, worth swapping). Per the rubric's anti-shiny rule (`<2 weeks old + <500 stars = BOOKMARK`), they pass the floor, but only just. Wait one more quarter to see if velocity persists.

**Q4 Adoption cost (2/3)**:
- **MCP-server-only path**: ~30 minutes. `pip install vibe-trading-ai` in a scoped venv (uv or uvx, never global), `claude mcp add` with the `vibe-trading-mcp` entry point, sandboxed workspace dir.
- **Full stack**: half-day+. Docker compose, FastAPI server, React 19 frontend build, 13 LLM provider env wiring, A-share data source pruning if you don't trade those.
- **Concept-borrow only** (lift skill files into your own harness): minutes per skill; they are plain markdown.

The asymmetry between "MCP server" and "full stack" is huge. Always start with MCP.

**Q5 The kill question (2/3)**: Honest answer for most semi-pro crypto traders: no specific past failure jumps out. The crypto-derivatives skill subset (`perp-funding-basis`, `liquidation-heatmap`, `stablecoin-flow`, `defi-yield`, `onchain-analysis`, `crypto-derivatives` umbrella) gives you structured analytical lenses you may have been doing ad-hoc. That is leverage but not "would have prevented X." Q5 is generous at 2/3.

**Total: 11/15 → BOOKMARK**

Re-evaluate in 30 days. Anti-shiny rule: don't pile this on top of an unsettled agent layer; if you're still bedding in your primary LLM-side tool, bookmark and move on.

## Who should use it

- **Claude-Code / Cursor / MCP-aware agent users** who want to add 21 finance research tools to their existing harness with one `pip install` and one `claude mcp add`. The MCP server is the highest-leverage entry point.
- **Crypto traders missing structured analytical lenses** for derivatives microstructure: funding-rate carry, liquidation heatmaps, stablecoin supply / flow, on-chain valuation (MVRV / NVT / SOPR). The 6 crypto-domain skill files are useful as standalone reading even if you never run their agent.
- **A-share / HK-equity researchers**: this is one of the few open-source Western-distributed agent stacks with first-class Tushare, AKShare, and Futu data-source coverage. If A-shares are in your investable universe, the value goes up materially.
- **Backtest-first researchers** who want a multi-agent committee to debate a strategy before they commit code.

## Who should skip

- **Anyone needing live execution.** Vibe-Trading does not place orders. If you came looking for "bot that trades," you want Freqtrade, Jesse, or Hummingbot.
- **Anyone with a strict "LLM never on the trade-execution path" guardrail.** The marquee feature is "LLM writes strategy code that the agent then runs." If your discipline forbids that, the codegen + auto-run loop is a liability, not a feature. (You can still use the MCP server in read-only mode and ignore the codegen surface.)
- **Solo operators allergic to dependency weight.** LangChain + LangGraph + FastAPI + React 19 + ~40 transitive deps is a lot for a personal-account research stack.
- **Equity-only traders outside A-share / HK.** The CEX / CCXT path is fine, but for US / EU equities you have lighter, more focused alternatives (OpenBB, raw yfinance + pandas).

## Comparison: agentic finance research stacks (May 2026)

| Stack | Live exec | LLM scope | Deps weight | Default LLM | License | Maturity | Sweet spot |
|---|---|---|---|---|---|---|---|
| **Vibe-Trading** | ✗ | research + codegen | heavy (LangChain+FastAPI+React) | DeepSeek v3.2 | MIT | 1 month | Multi-agent crypto / A-share research |
| `virattt/ai-hedge-fund` | ✗ | committee voting | light (Python only) | configurable | MIT | 18 months | Risk module + committee debate patterns |
| OpenBB | ✗ | bring-your-own | medium | bring-your-own | AGPL-3.0 | 5 years | Macro / TradFi research SDK |
| FinceptTerminal | ✗ | terminal UI | medium | optional | MIT | 6 months | Curses-style terminal for screening |
| nautilus_trader (research) | ✓ (separate path) | none native | heavy | none | LGPL-3.0 | 4 years | Institutional backtest + live |
| Freqtrade | ✓ | none | medium | none | GPL-3.0 | 6+ years | CEX crypto bot framework |

The takeaway: Vibe-Trading is the most aggressively LLM-native of the research-only stacks, with the heaviest dependency footprint and the youngest codebase. The adoption call should be driven by whether you value the multi-agent presets enough to carry the weight, or whether the MCP-server-only path covers your real need.

## Comparison: Vibe-Trading vs ai-hedge-fund (closest peer)

| Dimension | Vibe-Trading | ai-hedge-fund |
|---|---|---|
| Agent framework | LangGraph (DAG swarm) | hand-rolled committee voting |
| Multi-agent design | 29 named presets | role-tagged "analyst" agents |
| Skill / tool format | 72 SKILL.md files | inlined Python tools |
| Risk module | none surfaced | explicit advisory layer |
| Memory | FTS5 cross-session | none |
| Frontend | React 19 streaming | none |
| MCP server | ✓ published | ✗ |
| Data source breadth | 6 (heavy CN / HK lean) | configurable, narrower |
| LLM-on-execution-path | yes (codegen + auto-run) | yes (signal-only, no auto-trade) |

Use Vibe-Trading if: you want the MCP server as a free leverage hit, or you want pre-built crypto-derivatives lenses. Use ai-hedge-fund if: you want a clean risk-advisory pattern and a committee-voting harness with light deps.

## Onboarding path (if you BOOKMARK and revisit)

If you choose to engage at all, do **only** the MCP-server path:

1. Create a scoped venv. Don't pollute your global Python:
   ```bash
   uv venv /tmp/vt-mcp-test --python 3.11
   source /tmp/vt-mcp-test/bin/activate
   uv pip install vibe-trading-ai
   ```

2. Configure `.env` for a non-DeepSeek default. Their `agent/.env.example` defaults to DeepSeek; if you want Anthropic or OpenAI, set the matching `LANGCHAIN_PROVIDER` and `LANGCHAIN_MODEL_NAME` keys.

3. Add as MCP server to Claude Code (or your MCP-aware client):
   ```bash
   claude mcp add vibe-trading -- uvx --from vibe-trading-ai vibe-trading-mcp
   ```

4. Sandbox the workspace dir. Several MCP tools accept paths (`write_file`, `read_file`, `read_document`); restrict the workspace to a scratch directory, not your repos.

5. Try three queries that map to real research gaps:
   - "Pull the current 8h funding rate for BTC and ETH on OKX. Annualize."
   - "Liquidation heatmap thresholds for the last week of BTC moves."
   - "Stablecoin mint / burn events in the last 7 days. Net flow direction."

   If two of three save a chart-pull step you would have done by hand, keep the MCP server. If one or zero, drop it.

6. **Do not** run the auto-codegen path (`/code <run_id>`, `/pine <run_id>`) against any strategy you would actually deploy. Treat generated artifacts as drafts at best.

## Gotchas

- **MCP file I / O is not strictly sandboxed in their code**. The README says "sandboxed to workspace" but path validation is not visible in the server source. If you wire the MCP server, scope the workspace dir aggressively at the OS level.
- **Default LLM is DeepSeek** (Chinese provider). Fine technically; surprising as a default for finance research where users may not realize their queries route to a foreign provider. Swap before first use.
- **A-share / HK bias**. 4 of 6 data sources lean Chinese / Hong Kong. If you trade neither, you're paying the dependency cost without reaping the data-source coverage benefit. Lean on the crypto-only subset (OKX, CCXT, yfinance for HK / US).
- **No benchmarks, no academic paper**. The README markets feature counts ("29 presets", "72 skills", "7 backtest engines") but presents zero quantitative validation. Treat the marketing as marketing.
- **Self-evolving skills mechanic**. Their docs claim "agents create, patch, and delete workflows autonomously." This is a risk vector if you wire the MCP server with write access to anything that matters. Disable or sandbox.
- **1-month-old codebase**. API churn risk. Don't hard-code against their function signatures; treat every integration as throwaway scaffolding for the next 90 days.
- **LangChain dependency**. LangChain + LangGraph is heavy; if you already run a different agent framework (Hermes, deepagents, hand-rolled), running both is dependency-hell territory.

## Reddit / community consensus

As of early May 2026:

- **r/algotrading**: silence. Vibe-Trading does not yet appear in the standard threads on AI trading agents (which still default to ai-hedge-fund and various forks).
- **r/CryptoCurrency / r/cryptoquant**: minor mentions tied to the launch wave; no analytical takes yet.
- **GitHub awesome-* lists**: not yet aggregated into the major curated lists. Expected to land within the next month given star velocity.
- **Chinese tech Twitter / Weibo** (where the launch traction concentrated): broadly positive, with the usual "free 13-LLM-provider support" framing dominating.

This is consistent with a fresh academic-launch project: noise > signal in early weeks, with substance verdicts arriving 30 to 60 days post-launch as users actually try to integrate.

## Re-evaluate when

- **30 days from now**: did the commit cadence persist? Did the contributor list grow past 15? Did any v1.0 release ship? If yes, upgrade verdict; if velocity has died, downgrade.
- **The MCP server adds order-execution tools** (today: zero exist). That would change the safety profile and force re-evaluation under whatever LLM-on-execution-path discipline you hold.
- **Independent benchmark publication** (currently absent). A paper or third-party comparison would shift Q3 (credibility) up.
- **You add A-share / HK to your investable universe**. The data-source bias becomes value, not noise.
- **Skill format formalization**. Today the SKILL.md schema is undocumented. If they publish a skill-author spec, the borrowability of individual skills goes up materially.

## Sources

- [HKUDS/Vibe-Trading repo](https://github.com/HKUDS/Vibe-Trading)
- [PyPI: vibe-trading-ai](https://pypi.org/project/vibe-trading-ai/)
- [README.md (English)](https://github.com/HKUDS/Vibe-Trading/blob/main/README.md)
- [pyproject.toml dependency list](https://github.com/HKUDS/Vibe-Trading/blob/main/agent/pyproject.toml)
- [agent/SKILL.md (skill system overview)](https://github.com/HKUDS/Vibe-Trading/blob/main/agent/SKILL.md)
- [agent/mcp_server.py (MCP tool surface)](https://github.com/HKUDS/Vibe-Trading/blob/main/agent/mcp_server.py)
- [agent/cli.py (CLI subcommands)](https://github.com/HKUDS/Vibe-Trading/blob/main/agent/cli.py)
- [agent/src/skills/crypto-derivatives/SKILL.md (sample skill)](https://github.com/HKUDS/Vibe-Trading/blob/main/agent/src/skills/crypto-derivatives/SKILL.md)
- [HKUDS lab (other repos for credibility check)](https://github.com/HKUDS)
- [Tool evaluation 5-question rubric](../ai-tooling/tool-evaluation-5-question-rubric.md)
- [AI dev stack 8-layer model](../ai-tooling/ai-dev-stack-8-layer-model-with-tool-evaluations-march-2026.md)

#finance-tooling #evaluation #ai-agent #multi-agent #crypto #backtesting #mcp #langgraph #hkuds

## Related

- [[geckoterminal-evaluation]]: adjacent slot (DEX OHLCV data); 14/15 ADOPT, no overlap with Vibe-Trading's surface
- [[openbb-evaluation]]: adjacent slot (research SDK, not agent); 11/15 PILOT
- [[fincept-terminal-evaluation]]: adjacent slot (terminal UI); 10/15 SKIP
- [[claudekit-evaluation-and-unique-features]]: agent-tooling category peer; 10/15 BOOKMARK
- [[hermes-agent-comprehensive-briefing-april-2026]]: different agent runtime category (self-hosted, generic); useful contrast on harness design
- [[deepagents-vs-openclaw-vs-hermes-category-positioning]]: agent runtime / library distinction; Vibe-Trading is the "domain skin" on top of LangGraph
- [[tool-evaluation-5-question-rubric]]: scoring source of truth
