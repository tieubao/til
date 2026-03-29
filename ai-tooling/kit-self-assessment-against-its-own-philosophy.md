---
title: "Kit self-assessment against its own philosophy"
date: 2026-03-29
captured: 2026-03-29T07:50:23.739Z
tags: ["dwarves-kit", "evaluation", "philosophy", "autoResearch"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
---
## Context

dwarves-kit is growing (30 files, heading to 35). Risk: kit drifts from its own design philosophy as features accumulate. Need a mechanism to detect drift before it becomes bloat.

## Decision

Build a `/kit-health` command that checks the kit against its PHILOSOPHY.md principles. This is a meta-evaluation: "does the kit still follow its own rules?" Not an AutoResearch loop (which optimizes individual components).

## What it checks

| Check | Principle it validates | How to verify |
|-------|----------------------|---------------|
| File count <= 35 | Hard limit | `find` + count |
| All hooks < 500ms | Performance budget | `time` each hook with sample JSON input |
| Every command has 1-sentence description | Complexity check | Parse YAML frontmatter `description` field |
| Every pattern has source citation | "Synthesize, don't originate" | Grep README.md credits section |
| No compiled binaries | "Bash over binaries" | `find` for .exe, .bin, compiled artifacts |
| All 8 stack layers addressed | Coverage | Check against layer checklist |
| Hook logs show recent activity | Hooks are actually firing | Check log file modification dates |
| settings.json is valid JSON | Basic sanity | `jq . settings.json` |

Output: a health report with pass/fail per check and an overall score.

## Why not AutoResearch (Karpathy loop) yet

AutoResearch optimizes individual components (hook patterns, command prompts) against a scoring function. It needs a corpus of real usage data to evaluate against. With 0 users and 0 session transcripts, there's nothing to score.

The path to AutoResearch:
1. Ship v1 with logging on all hooks (building the corpus passively)
2. Use for 4+ weeks, accumulate 50+ stop-responses and 10+ reviewed PRs
3. Build the three-file contract: program.md (frozen goal) + modifiable component + eval.sh (scoring)
4. Run the loop: propose change, score against corpus, keep if improved, revert if not

For anti-rationalization specifically: program.md = "detect rationalization with <5% false positives." The patterns array in anti-rationalization.sh is the modifiable component. eval.sh runs the patterns against the labeled corpus and reports FP/FN rates.

## Consequences

- Kit drift is caught early (monthly /kit-health run during /retro)
- AutoResearch is deferred but designed -- the eval framework is ready when the data exists
- The log files from all hooks are the future training data, not just debug output