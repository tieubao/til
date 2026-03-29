---
title: "Building a Claude Code workflow kit from extracted patterns"
date: 2026-03-29
captured: 2026-03-29T07:49:36.331Z
tags: ["claude-code", "dwarves-kit", "sdd", "workflow", "playbook"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
---
## When to use this

You want to build a custom Claude Code workflow kit instead of adopting someone else's wholesale. You've evaluated the ecosystem and decided that no single tool covers your full workflow, but several tools have excellent individual patterns worth extracting.

## The extraction process

### Step 1: Survey and score

Evaluate repos against a rubric (layer fit, credibility, adoption cost, pain match, timing). Score 1-3 per criterion, max 15. 12+ = ADOPT, 8-11 = BOOKMARK, 5-7 = SKIP. Never adopt more than 1 tool per week.

### Step 2: Classify each pattern by mechanism

For each pattern worth extracting, decide:

| If the pattern is... | Make it a... | Because... |
|---------------------|-------------|-----------|
| A workflow phase the user drives (plan, review, ship) | Command | Human judgment on when to invoke |
| A rule that must be enforced every time (block destructive commands) | Hook | Deterministic, no exceptions |
| A behavior Claude should adopt autonomously (fetch API docs) | Skill | Claude decides when relevant |
| A tool that exists separately and your kit just checks for | Dependency | Don't rebuild, just verify it's present |

### Step 3: Map to Claude Code's hook events

For each hook, identify: which event (PreToolUse, Stop, SessionStart, etc.), which matcher (Bash, Write|Edit, etc.), which exit code (0 = proceed, 2 = block), and what JSON output format.

### Step 4: Design the shared data contract

All commands and hooks must agree on one format and one directory. In our case: `.planning/SPEC.md` is the shared contract. `/spec` produces it, `/execute` reads it, `/review` checks against it, `/docs` updates it. One format eliminates translation overhead between phases.

### Step 5: Set hard limits before building

Without limits, the kit grows until it's unmaintainable:
- File count cap (ours: 35)
- Hook latency budget (ours: 500ms)
- No compiled binaries
- Every pattern must cite its source

## The spec lifecycle (learned the hard way)

A flat `.planning/SPEC.md` breaks after 10 features. Use per-feature specs:

```
.planning/
  active/
    feat-auth-api.md       <-- currently being built
  completed/
    feat-user-onboarding.md
```

`/spec` writes to `active/`. `/ship` moves completed specs to `completed/`. Git history handles versioning.

## Execution engine reality check

Our `/execute` command asks Claude to use the native Task tool for subagent dispatch. It works for 3-5 task specs on greenfield projects. Beyond that:
- Subagent context loading is imprecise (burns tokens on orientation)
- No crash recovery (session dies = restart from scratch)
- No real parallelism (Task tool is sequential in practice)
- Orchestrator context fills up after 8-10 task reports

For 10+ task specs, use GSD v2 as the execution engine or fall back to `/next` (manual, one task at a time).

## Kit maintenance

Two distinct problems:
- **Prompt sharpness** (AutoResearch after 4+ weeks of logged data): run the Karpathy loop on /review and anti-rationalization patterns
- **Ecosystem currency** (monthly manual check): read Claude Code changelog, check if new hook events are useful, verify no deprecated patterns