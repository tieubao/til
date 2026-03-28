---
title: "AI dev stack - 8-layer model with tool evaluations (March 2026)"
date: 2026-03-28
captured: 2026-03-28T16:03:17.349Z
tags: ["sdd", "ai-tooling", "claude-code", "stack-model", "evaluation-framework"]
source: "Claude.ai session on SDD research + tool evaluation, March 28 2026"
---
# AI Dev Stack - 8-Layer Model (March 2026)

## The stack

Built across two sessions of SDD research. The original 4-layer model (terminal, IDE, agent, methodology) expanded to 8 layers after discovering missing middle layers.

```
L5:   Orchestration / workspace     Nimbalyst, Intent (Augment)
L4:   Methodology / workflow        GSD, Spec Kit, OpenSpec, gstack, ClaudeKit, Ralph
L3.5: Context - codebase intel      codebase-memory-mcp, vexp (AST graph, token savings)
L3.5: Context - external docs       Context Hub, Context7, llms.txt standard
L3.5: Context - project config      CLAUDE.md, skill files, ToB hooks
L3.5: Context - live data           MCP servers (Notion, Google, Capacities)
L3:   Coding agent                  Claude Code
L2.5: Agent workspace / session     tmux (current), Agent Teams (experimental)
L2:   IDE / editor                  VS Code
L1:   Terminal                      tmux, Ghostty (consider)

Separate axis: AutoResearch (Karpathy loop) - not a layer, an optimization pattern
```

## Three new layers discovered

**L5: Orchestration** - manages multiple agent sessions in parallel. Nothing coordinates 3+ Claude Code sessions running simultaneously. Nimbalyst adds visual kanban + git worktree isolation. Intent adds living spec + multi-agent coordination.

**L3.5 split: codebase intelligence vs external docs** - originally one flat layer, actually two distinct problems. External API docs (Context Hub, Context7) vs your own code's structure (codebase-memory-mcp parses AST into SQLite graph, 40-95% token savings).

**L2.5: Agent workspace / session manager** - tmux gives split panes but zero task awareness. No visibility into "task A is 70% done, task B is blocked." Claude Code Agent Teams targets this layer experimentally.

## SDD frameworks compared

| Framework | Coverage | Best for | Stars |
|-----------|----------|----------|-------|
| Spec Kit | Full pipeline (spec + execution) | Greenfield, structured | 82k |
| GSD | Spec only, you code | Solo dev, zero ceremony | 3k |
| OpenSpec | Spec for existing code | Brownfield | 33.8k |
| BMAD | Full + agile simulation | Enterprise teams 5+ | 42k |
| Ralph Wiggum | Execution only | Autonomous code-test-fix | - |
| Smart Ralph | Spec + execution bundled | Spec pipeline + Ralph | - |

Key insight: SDD frameworks split into **spec generators** (write the plan) and **execution engines** (run the plan). Some do both.

## Three levels of SDD (Fowler/Boeckeler)

1. **Spec-first**: Write spec, then code. Most tools stop here.
2. **Spec-anchored**: Keep spec alive after coding for evolution.
3. **Spec-as-source**: Spec IS the source, code is generated. Only Tessl attempts this.

## Context layer tools

- **Context Hub** (Andrew Ng): Curated API docs + agent annotations that persist across sessions + feedback loop to doc maintainers
- **Context7** (Upstash): 9k+ library docs via MCP. Broader coverage, no annotation system
- **codebase-memory-mcp**: AST graph via tree-sitter into SQLite. 14 MCP tools. Queries that consumed 8-12k tokens now need 200-2k
- **vexp**: Dependency graph + passive session memory. Local-first, no cloud
- **llms.txt**: Standard for websites to provide LLM-friendly content maps. Supply side that Context Hub/Context7 build on

## Role-based tools (gstack, ClaudeKit)

**gstack** (Garry Tan, YC CEO): 15 slash commands for Claude Code. Sprint-shaped: Think > Plan > Build > Review > Test > Ship > Reflect. Unique: persistent headless Chromium browser for QA, CEO-level product review questions. 23k+ stars. Controversy: critics say "it's just a bunch of prompts." Value is in the specific judgment encoded from 20 years of startup evaluation.

**ClaudeKit/VividKit** (mrgoonie): 50+ commands, full SDLC coverage. Vietnamese docs (good for Dwarves adoption). Paid Engineer Kit + free OSS skills. Unique pieces: /ck:plan validate (interview-style spec gate with auto-propagation), /ck:plan red-team (4 adversarial review lenses), /ck:bootstrap (full project scaffold). Verdict: BOOKMARK. Too comprehensive to adopt before trying simpler tools.

**Trail of Bits claude-code-config**: Not a workflow tool. Agent hardening kit. Sandboxing, hooks, anti-rationalization Stop hook, statusline. Cherry-pick: anti-rationalization gate, rm-rf blocker, push-to-main blocker.

## AutoResearch (Karpathy loop)

Autonomous optimization loop for anything with a measurable metric. Three-file contract:
- `program.md` - human goals (frozen)
- `train.py` / `skill.md` - the thing being optimized (agent-modifiable)
- `eval.py` - scoring function (frozen, agent cannot touch)

Loop: agent reads all > proposes change > modifies code > runs eval > if score improved, git commit (new baseline); if not, git revert. ~100 experiments overnight.

Key insight: the pattern works on anything you can score. Applied to: ML model training, prompt optimization, SEO, marketing copy. For Dwarves: could optimize skill files, prompt templates, document generators.

## Tool evaluation framework (5-question rubric)

Built as a reusable `/eval-tool` slash command skill:

1. **Layer**: Where does it sit in the stack?
2. **Replace or complement**: What does it touch that I already have?
3. **Credibility**: Who made it and why?
4. **Adoption cost**: How long to try for real?
5. **Kill question**: What specific failure in my last 3 projects would this have prevented?

Scoring: 12-15 = ADOPT, 8-11 = BOOKMARK, 5-7 = SKIP. Rule: never adopt more than 1 tool per week.

## Integrated workflow (6 phases)

1. **Think**: Challenge the idea (gstack /office-hours or Claude chat pushback)
2. **Spec**: Generate the plan (GSD for lightweight, Spec Kit for complex, OpenSpec for brownfield)
3. **Context**: Arm the agent (Context Hub for APIs, Context7 for libraries, CLAUDE.md for conventions)
4. **Build**: Execute against spec (contractor + Claude Code, optionally Ralph for autonomous loop)
5. **Review**: Catch what humans miss (gstack /review for security, /qa for browser testing)
6. **Ship + reflect**: Deploy, capture learnings to Capacities, /retro for periodic reviews

## Concrete next actions

1. Install GSD, use on one real Dwarves task this week (5 min)
2. Cherry-pick Trail of Bits hooks: anti-rationalization, rm-rf blocker, push-to-main blocker (10 min)
3. Try codebase-memory-mcp on largest Dwarves project with 100+ files (10 min)
4. Everything else: bookmark, revisit in 30 days using /eval-tool skill

## Session artifacts produced

- `eval-tool/SKILL.md` - automated tool evaluation slash command
- `ai-stack-complete.jsx` - interactive 8-layer stack map (React, light theme, iOS-friendly)
- `claudekit-eval.jsx` - ClaudeKit evaluation card
- `ai-stack-light.jsx` - combined stack + ClaudeKit eval with tabs