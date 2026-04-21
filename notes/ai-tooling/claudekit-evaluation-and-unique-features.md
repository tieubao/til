---
title: "ClaudeKit evaluation and unique features"
date: 2026-03-26
captured: 2026-03-26T23:11:13.581Z
tags: ["ai-tooling", "claude-code", "claudekit", "skill-pack"]
source: "Claude iOS session - SDD research session 3"
aliases: []
status: refined
---
## ClaudeKit (by mrgoonie)

Paid skill pack (free OSS subset) for Claude Code. 50+ slash commands covering full SDLC. VividKit is the documentation site with Vietnamese translation. Claims 4,000+ users in 109 countries.

Install free OSS: `/plugin marketplace add mrgoonie/claudekit-skills`

## Key commands

- `/ck:plan` -- spec generation with flags: `--hard` (complex multi-phase), `--parallel`, `--two` (2-phase), `validate` (interview gate), `red-team` (adversarial reviewers)
- `/ck:cook` -- all-in-one: research, plan, implement, test, review. Flags: `--interactive` (default), `--fast`, `--parallel`, `--auto`
- `/ck:scout` -- edge case detection across affected files, data flows, error paths
- `/ck:bootstrap` -- full project scaffolding from description
- `/ck:security-scan` -- OWASP top 10, hardcoded secrets, dependency vulnerabilities

## Unique pieces (not found in gstack or SDD frameworks)

1. `/ck:plan validate` -- interview-style validation gate. Decisions auto-propagate to implementation phase files.
2. `/ck:plan red-team` -- spawns 4 adversarial reviewers: Security, Failure Mode, Assumption Destroyer, Scope Critic. Auto-scales 2-4 lenses based on plan complexity.
3. `/ck:bootstrap` -- full project scaffolding: tech stack selection, architecture, UI/UX, implementation, docs.

## Evaluation (scored 10/15 = BOOKMARK)

Heavy overlap with gstack + SDD frameworks. ClaudeKit is the pre-assembled version of the stack you'd otherwise build from separate tools. Vietnamese docs are a real advantage for Dwarves team adoption. But: 50 commands is cognitive overload when you haven't tried 1 tool yet. Try GSD first; ClaudeKit is the upgrade path if GSD feels incomplete.

#ai-tooling #claude-code #claudekit #skill-pack

## Related

- [[claudekit-deep-dive-session-recovery-red-team-and-gaps]] - the follow-up deep dive on session recovery and specific gaps
- [[ai-dev-stack-8-layer-model-with-tool-evaluations-march-2026]] - positions ClaudeKit at L4 (methodology/workflow) alongside gstack and SDD frameworks
- [[tool-evaluation-5-question-rubric]] - the 5-question rubric that produced the 10/15 BOOKMARK score
- [[commands-vs-hooks-vs-skills-decision-framework]] - the decision framework for when to use commands vs skills, relevant to ClaudeKit's 50+ slash commands