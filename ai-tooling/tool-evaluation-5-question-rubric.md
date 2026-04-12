---
title: "Tool evaluation 5-question rubric"
date: 2026-03-26
captured: 2026-03-26T23:10:18.606Z
tags: ["ai-tooling", "evaluation", "framework", "decision-making"]
source: "Claude iOS session - SDD research session 3"
aliases: []
status: refined
---
## The 5-question rubric

When a new AI dev tool drops, run these 5 questions in under 10 minutes:

**Q1: Layer** -- Where does it sit in the 8-layer stack? If you can't map it, it's either a new category or marketing fluff.

**Q2: Replace or complement?** -- If it replaces something: is the replacement 3x better? If it complements: does it plug a gap you actually feel? "Nice to have" = skip.

**Q3: Credibility** -- Security firm sharing their config > solo dev's weekend project. Company battle-testing internally > "awesome list" compilation. Trail of Bits, Anthropic, Karpathy carry weight. Random 18-star repo? Wait.

**Q4: Adoption cost** -- Under 30 min = try now. 1-2 hours = bookmark, try this weekend. Half-day+ = dedicated session. Multiply by contractor count if it requires project restructuring.

**Q5: The kill question** -- What specific failure in my last 3 projects would this have prevented? If you can't name one, bookmark and move on.

## Scoring

Each question scores 1-3. Total 5-15:
- 12-15: **ADOPT NOW** -- try this week
- 8-11: **BOOKMARK** -- revisit in 30 days
- 5-7: **SKIP** -- not worth the attention

## Anti-shiny-object rules

- Never adopt more than 1 tool per week
- If you haven't tried your current "next step" tool yet, no new evaluations
- Star count alone means nothing. 50k stars on a tool that solves a problem you don't have = skip
- If the repo is less than 2 weeks old with fewer than 500 stars, default to BOOKMARK

## Automated as a skill file

Built as `/eval-tool` Claude Code slash command that takes a GitHub URL, auto-classifies layer, checks overlap, scores rubric, outputs verdict.

#ai-tooling #evaluation #framework #decision-making

## Related

- [[ai-dev-stack-8-layer-model-with-tool-evaluations-march-2026]] - Q1 ("which layer?") references this stack model directly
- [[claudekit-evaluation-and-unique-features]] - a concrete application of this rubric (ClaudeKit scored 10/15 = BOOKMARK)
- [[llm-memory-benchmarks-and-evaluation-crisis]] - evaluation methodology problems in a different domain; the "kill question" approach avoids the gaming that plagues memory benchmarks