---
title: "Prompt improvement as a learning technique"
date: 2026-03-29
captured: 2026-03-29T07:49:57.303Z
tags: ["prompting", "learning", "workflow", "meta"]
source: "Claude.ai session: dwarves-kit design (March 29, 2026)"
aliases: []
status: refined
---
Throughout a long design session, every vague question was sharpened into a structured prompt before execution. The improved version was shown so the user could learn the technique. After 8+ rounds, a clear pattern emerged.

The core move: take a vague request, identify what's wrong with it, restructure it into something that forces a specific, useful output.

## The pattern

1. User asks something vague ("what are rooms for improvement?")
2. Identify why it's vague: no deliverable specified, no constraints, no scope boundary
3. Restructure: add a concrete output format, constraints, and success criteria
4. Show both versions side by side so the user sees what changed
5. Execute the improved version

## Common failure modes in prompts (observed this session)

| Vague pattern | Why it fails | Fix |
|---------------|-------------|-----|
| "Have a conclusion" | "Conclusion" could mean 10 things | Specify: "produce a mapping visual + priority list + file structure" |
| "Any other suggestions?" | Produces a wish list, not a design doc | Reframe: "what's the evolution strategy?" with a process, not a list |
| "How does it compare?" | Repeats prior analysis work | Reframe: "what's the differentiation thesis? One paragraph." |
| "Rooms for improvement" | Open-ended, no filter for what matters | Reframe: "design boundaries -- the NO list" |
| "Should we build auto evaluation?" | Unclear what "evaluation" means | Split: "what gets evaluated? what's the criteria? what's the three-file contract? is it worth building now?" |

## The meta-insight

The prompt improvement step isn't just about getting better answers. It's a thinking tool. The act of sharpening a vague question into a structured one forces you to articulate what you actually need. Half the time, the improved prompt reveals that the original question was asking the wrong thing entirely.

Skipping this step is appropriate for operational tasks where the outcome is fixed regardless of phrasing (file operations, memory management, Notion updates). Knowledge questions, design decisions, and research always benefit from the improvement pass.

## Related

- [[why-knowledge-notes-need-context-not-just-facts]] - the same principle applies: vague prompts lack context just as knowledge notes without context lack value
- [[dwarves-kit-design-philosophy-and-architecture]] - the design session where this prompt improvement pattern was observed
- [[tool-evaluation-5-question-rubric]] - another structured thinking framework that turns vague assessment into actionable decisions