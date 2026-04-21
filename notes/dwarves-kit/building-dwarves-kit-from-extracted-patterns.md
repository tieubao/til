---
title: "Building dwarves-kit from extracted patterns"
date: 2026-03-29
captured: 2026-03-29T16:06:38.299Z
tags: ["dwarves-kit", "patterns", "synthesis"]
source: "Migrated from ai-tooling/ (March 2026)"
aliases: []
status: refined
---
# Building dwarves-kit from extracted patterns

Process of building the kit by cherry-picking battle-tested patterns from 6+ repos: GSD (atomicity, spec format), gstack (/review paranoid tone, /think forcing questions), Trail of Bits (anti-rationalization, safety hooks, statusline), OMC (HUD, slop-cleaner, notepad), CCGS (path-scoped rules, CDP), Smart Ralph (fail-fix-re-verify). Every component traces to a source. "Synthesize, don't originate" principle in action.

## Related

- [[claude-code-ecosystem-repo-evaluations-for-kit-building]] - the scored evaluations that fed this extraction process
- [[dwarves-kit-design-philosophy-and-architecture]] - the 7 principles that governed what to extract and what to skip
- [[dwarves-kit-v1-2-claudekit-patterns-adopted]] - a later round of extraction specifically from ClaudeKit