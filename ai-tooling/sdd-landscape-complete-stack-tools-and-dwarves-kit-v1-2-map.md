---
title: "SDD landscape - complete stack tools and dwarves-kit v1.2 map"
date: 2026-03-29
captured: 2026-03-29T15:57:31.284Z
tags: ["dwarves-kit", "sdd", "landscape", "claude-code", "multi-agent", "stack-model"]
source: "Claude.ai session consolidating SDD landscape (March 2026)"
---
# SDD landscape: complete stack, tools, and dwarves-kit v1.2 map

## The 8-layer stack model

```
L5:   Orchestration / workspace     Nimbalyst (BM), Intent (BM)
L4:   Methodology / workflow        dwarves-kit v1.2 (BUILT), GSD v1 (ADOPT patterns),
                                    GSD v2 Pi SDK (BM), Spec Kit (BM), OpenSpec (BM),
                                    ClaudeKit (BM), gstack (BM patterns)
L3.5: Context - codebase intel      codebase-memory-mcp (ADOPT)
L3.5: Context - external docs       Context Hub (ADOPT), Context7 (ADOPT), llms.txt
L3.5: Context - project config      CLAUDE.md (BUILT), .claude/rules/ (BUILT), skills (BUILT)
L3.5: Context - live data           MCP servers: Notion, Google, Capacities (ADOPT)
L3:   Coding agent                  Claude Code (ADOPT), 8 custom subagents (BUILT)
L2.5: Agent workspace / session     tmux (ADOPT), Agent Teams (BM, experimental)
L2:   IDE / editor                  VS Code (ADOPT)
L1:   Terminal                      tmux (ADOPT), Ghostty (BM)

Separate axis: AutoResearch / Karpathy loop (BM), /eval-tool rubric (BUILT)
```

Verdicts: BUILT = in dwarves-kit, ADOPT = use alongside kit, BM = bookmark (extract patterns only), SKIP = evaluated and rejected.

## Repos evaluated with scores

| Repo | Stars | Score | Verdict | What we extracted |
|------|-------|-------|---------|-------------------|
| GSD v1 | 31k | 13/15 | ADOPT patterns | Aggressive atomicity, 4 parallel researchers, wave execution, fresh-context-per-task |
| Trail of Bits | config | 14/15 | ADOPT hooks | Anti-rationalization, rm-rf blocker, push-to-main blocker, statusline |
| gstack (Garry Tan) | 23k | 11/15 | BM | /review paranoid tone, /think forcing questions, /qa browser testing |
| OMC | 9.7k | 10/15 | BM | HUD/statusline, slop-cleaner (Stop hook), notepad compaction pattern |
| CCGS (Game Studios) | 7k | 8/15 | BM | Path-scoped rules, CDP (Question > Options > Decision > Draft > Approval) |
| ClaudeKit | - | 10/15 | BM | Vietnamese docs, /ck:plan validate, /ck:plan red-team, /ck:bootstrap |
| Smart Ralph | - | 11/15 | BM | Fail-fix-re-verify retry loop |
| Spec Kit | 82k | - | BM | Full pipeline structure (future upgrade from GSD if needed) |
| OpenSpec | 33.8k | - | BM | Brownfield spec gen (reads existing code first) |
| BMAD | 42k | - | SKIP | Enterprise, overkill for solo/small team |
| GSD v2 | 1.6k | - | BM | Pi SDK: programmatic context control, crash recovery, stuck detection |
| codebase-memory-mcp | - | 14/15 | ADOPT | AST graph, 66 langs, 120x token reduction, single binary |
| Context Hub | - | 12/15 | ADOPT | Curated API docs with agent annotations |
| Context7 | - | 12/15 | ADOPT | 9k+ library docs via MCP |

## dwarves-kit v1.2 inventory

### 12 commands
/start, /think, /spec, /spec-validate, /execute, /next, /review, /review-team, /docs, /ship, /retro, /kit-health

### 8 agents
| Agent | Tools | Model | Dispatched by |
|-------|-------|-------|--------------|
| task-verifier | Read, Grep, Glob, test runners | sonnet | /execute |
| fix-agent | Read, Write, Edit, test runners | sonnet | /execute |
| security-auditor | Read, Grep, Glob, npm audit, go vet | sonnet | /review-team |
| reviewer (3 lenses) | Read, Grep, Glob, test runners | sonnet | /review-team |
| research-stack | Read, Grep, Glob | haiku | /spec |
| research-features | Read, Grep, Glob, git log | sonnet | /spec |
| research-architecture | Read, Grep, Glob, git log | sonnet | /spec |
| research-pitfalls | Read, Grep, Glob, git log | sonnet | /spec |

### 11 hooks
SessionStart: context-readiness (v2, with command suggestions)
PreToolUse(Bash): safety-gate (rm-rf, push-to-main, force-push)
PreToolUse(Write): spec-drift-guard
PostToolUse(Write|Edit): auto-format
PostToolUse(compact): post-compact-reinject
PreCompact: pre-compact-backup
Stop: anti-rationalization (5 patterns), slop-cleaner (nudge)
Notification: desktop alert
PermissionRequest: auto-approve reads
StatusLine: model, branch, context %, cost

### Key protocols
- Collaborative Design Protocol (CDP): Question > Options > Recommendation > Decision > Record
- Three decision modes: lead (human picks), coder (orchestrator picks), autonomous (proceed + log)
- Verification pipeline: worker > task-verifier > fix-agent retry (max 2) > escalate

## 6-phase workflow

1. **Think**: /start (detect state) > /think (6 forcing questions) > decision brief
2. **Spec**: /spec (4 research agents for brownfield, inline fallback if agents not installed) > /spec-validate (4 adversarial reviewers)
3. **Build**: /execute (worker > CDP plan > task-verifier > fix-agent retry) or /next (manual). Hooks enforce: safety-gate, spec-drift, auto-format, anti-rat, slop-cleaner
4. **Review**: /review (single-pass) or /review-team (3 parallel lenses: security, architecture, test-coverage)
5. **Ship**: /docs > /ship (commit + PR). safety-gate blocks push to main
6. **Reflect**: /retro (what worked, what hurt) > /kit-health (self-assessment)

Always-on hooks: context-readiness, pre/post-compact, permission-auto-approve, notification, statusline

## Open decisions (resolved)

1. **Execution engine**: Stay native (Claude Code Task tool). Revisit GSD v2 Pi SDK if crash recovery becomes a real pain after 3+ /execute runs.
2. **Graph context**: ADOPT codebase-memory-mcp on any project with 50+ source files. Skip vexp.
3. **Agent layer**: Complete for v1.2. No docs-writer yet (not needed). Future: parallel execution via Agent Teams (Phase 3, pending real usage data).
4. **SDD level**: Currently spec-first (level 1). Spec-anchored (level 2) is the natural next step once specs are kept alive post-implementation.

## What's NOT built (and why)

- Phase 3 parallel execution (Agent Teams): needs real usage data first
- Auto-eval / Karpathy loop: needs 50+ real session transcripts
- Custom execution runtime: use native Task tool, don't rebuild GSD v2
- Persistent cross-session agent memory: pre/post-compact hooks are sufficient
- docs-writer agent: /docs in main session is fine for small updates