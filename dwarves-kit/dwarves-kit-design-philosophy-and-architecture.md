---
title: "dwarves-kit design philosophy and architecture"
date: 2026-03-29
captured: 2026-03-29T16:05:09.016Z
tags: ["claude-code", "dwarves-kit", "philosophy", "architecture"]
source: "Migrated from ai-tooling/ (March 2026)"
---
## Context

Building a Claude Code workflow kit (dwarves-kit) to cover the full SDLC for a solo tech lead + contractors. Needed principles that would govern what gets added, what gets rejected, and how the kit evolves without becoming bloatware.

## The 7 principles (each resolves a real tradeoff)

**1. Guardrails over guidance.** A rule in CLAUDE.md is followed ~70% of the time. A hook with exit 2 is followed 100%. Safety-critical rules get hooks, not documentation. This is why "don't push to main" is a PreToolUse hook, not a CLAUDE.md line.

**2. Synthesize, don't originate.** Every pattern must trace to a proven source repo. No novel inventions in the kit. If you have a new idea, test it standalone for 3+ months before merging. This prevents the kit from becoming a personal experiment lab.

**3. One kit, whole cycle.** `.planning/SPEC.md` is the shared contract between /spec, /execute, /review, and /docs. Data flows through the cycle without re-entry or format translation. This is the core differentiator vs. using 3-4 separate tools.

**4. Shallow and wide beats deep and narrow.** Covering 7 phases at 70% depth is better than 2 phases at 100%. The biggest AI-assisted dev failures come from skipped phases (no spec, no review, no retro), not insufficient depth in any one phase.

**5. Bash over binaries.** Every hook is a readable shell script. A contractor reads any .sh file in 30 seconds. `bash -x hooks/safety-gate.sh` is the entire debugging workflow. One carve-out: HUD/statusline may use Node.js for per-turn JSON parsing speed.

**6. External tools are dependencies, not features.** Check for Context Hub/Context7/codebase-memory-mcp and warn if missing. Never rebuild their functionality. If chub breaks, remove the check; don't maintain a replacement.

**7. Detect, don't dictate.** Suggest the right action based on project state (SessionStart context injection). Never block workflow progression except for safety (rm-rf, push-to-main). Rigid phase gates annoy experienced coders who know when to skip a step.

## Hard limits

- 35 files maximum (currently 30, room for 5 more before something must be removed)
- 500ms per hook execution
- No compiled binaries
- No paid dependencies
- No LLM API calls in v1 hooks

## How each principle was tested

Every principle has an example of a decision it made AND a decision it would reject:

| Principle | Decision it made | Decision it rejects |
|-----------|-----------------|-------------------|
| Guardrails over guidance | Anti-rationalization is a Stop hook | "Add a CLAUDE.md rule saying always write tests first" |
| Synthesize, don't originate | /think is adapted from gstack's /office-hours | "I have a new review methodology nobody's tried" |
| One kit, whole cycle | .planning/SPEC.md flows unchanged into /execute | "Use GSD for planning and a separate tool for execution" |
| Shallow and wide | /execute uses native Task tool (not as deep as GSD v2's Pi SDK) | "Build a custom TypeScript runtime for execution" |
| Bash over binaries | All 9 hooks are bash + jq | "Rewrite hooks in Python for better JSON handling" |
| External dependencies | context-readiness checks if chub is installed | "Build our own API doc fetcher" |
| Detect, don't dictate | SessionStart injects project state as context | "Block /execute unless /spec-validate was run" |

## How we'll know a principle is wrong

If the same principle gets a carve-out more than once, it should be revisited entirely. "Bash over binaries" already has one carve-out (HUD). A second carve-out means the principle doesn't hold and should be rewritten, not bent again.