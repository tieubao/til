---
title: "Finding your unknowns: the bottleneck in agentic coding"
date: 2026-07-04
captured: 2026-07-04T05:30:00.000Z
tags: ["ai", "agents", "prompting", "claude-code", "workflow"]
source: "A Field Guide to Fable: Finding Your Unknowns - Thariq (Claude Code team, Anthropic), https://x.com/trq212/status/2073100352921215386"
status: refined
---

# Finding your unknowns: the bottleneck in agentic coding

## Context

Published the day Claude Fable 5 launched, by an engineer on the Claude Code team. The claim worth keeping: with current frontier models, output quality is no longer bottlenecked by model capability but by how fast the OPERATOR clarifies their own unknowns. The map (your prompt, skills, context) is not the territory (the codebase, the real constraints); the gap between them is where agents guess, and every guess is a place the work can go wrong.

![The unknowns 2x2](https://assets.han-ws.workers.dev/i/2026/07/unknowns-2x2.svg)

## The frame

Break any task down with the Rumsfeld 2x2. Each quadrant has a distinct technique; using the wrong one wastes a session:

| Quadrant | What it is | Technique |
|---|---|---|
| Known knowns | what your prompt states | say it well: goal, constraints, where you are in your thinking, your experience level |
| Known unknowns | questions you know are open | be INTERVIEWED, one question at a time, prioritized by "would the answer change the architecture?" |
| Unknown knowns | taste; you recognize it when you see it | prototype and react: throwaway mocks, N wildly different design directions, never specify what you cannot articulate |
| Unknown unknowns | what you have not considered at all | the BLINDSPOT PASS: "find my unknown unknowns so I can prompt you better" (the literal words work) |

Two calibration failures bracket the whole game: too specific and the agent follows you off a cliff when a pivot was right; too vague and it fills gaps with generic best practices that do not fit your task.

## Moves worth stealing

- **References beat descriptions.** When you cannot articulate what you want, point at source code that already implements the semantics, even in a different language. Code is a richer spec than prose.
- **Order plans by likelihood-to-change.** Ask for implementation plans that lead with the decisions you will actually tweak (data models, interfaces, UX flows) and bury the mechanical refactoring you would rubber-stamp anyway.
- **Keep an implementation-notes file with a Deviations section.** Planning never covers everything; when the agent hits an edge case mid-run, the default should be "pick the conservative option, log it under Deviations, keep going", so unknowns discovered in flight become data instead of silent drift.
- **Quiz before merge.** After a long session the agent has done more than you realize; a quiz built from the actual diff is a cheap check that YOU still understand the system you own.
- **Unknowns are found before, during, AND after implementation.** It is an iterative loop, not a planning phase; some unknowns only surface once real code exists, and some tell you to solve a different problem entirely.

The economic frame that ties it together: every explainer, brainstorm, interview, prototype, and reference is a cheap way to find out what you did not know, before it gets expensive to fix.

## Related

- [[scaling-the-harness-six-components]] - the system-level counterpart: unknowns are the operator-side gap; the harness is the machinery that surfaces or hides them
- [[multi-agent-coding-brain-rot-scan-design-externalized-state-clean-handoffs]] - externalized state and clean handoffs are how deviation-logging survives across sessions
