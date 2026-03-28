---
title: "Why Knowledge Notes Need Context Not Just Facts"
date: 2026-03-28
captured: 2026-03-28T05:29:15.899Z
tags: ["pkm", "workflow", "knowledge-management"]
source: "Claude Code session - github-mcp-worker project, iterating on knowledge capture pipeline"
---
## Context

Building a personal knowledge management pipeline that captures learning moments from Claude sessions and pushes them to a GitHub repo (Obsidian vault). The pipeline had been working end-to-end: detect a learning moment, clean it, format it, push via MCP Worker. But the output quality was off.

## The Problem

The first two notes the capture system produced were technically correct but shallow. One documented that Claude Code hooks have different valid `decision` values per event type. The other documented a redundant API call pattern. Both were structured as flat bullet lists: here's the fact, here's a table, here's a code example.

Reading them back, they felt like Stack Overflow answers stripped of the question. They told you WHAT the answer was but not:
- What situation led to discovering it
- Why it mattered in that moment
- What the investigation looked like
- When you'd encounter it again

A note that says "Stop hooks use `approve`, not `allow`" is a fact. A note that says "I was configuring a Stop hook to inject learning-capture reminders, and it kept throwing validation errors because the decision field accepts different values per hook event type" is a story. The story is what makes future-you actually recognize the pattern when it shows up again in a different context.

## Discovery

The realization came from reviewing the notes immediately after pushing them. The pipeline's content type system had four types (Q&A, Definition, TIL, Article) and defaulted to TIL when ambiguous. TIL is the shallowest format: 2-3 sentences, one code example, no context required.

The problem wasn't that TIL exists as a type. Quick gotchas deserve quick notes. The problem was that the **default** was TIL, so everything got flattened to the minimum depth. Most learning moments from debugging sessions and architectural decisions deserve more: they have a situation that triggered them, an investigation that uncovered them, and a transferable principle that makes them useful later.

The fix was three changes to the capture pipeline:

1. **Default type changed from TIL to Atomic Note.** Atomic Notes require a Context section, a Problem section, a "What I Found" section, and a "How to Spot This" section. This forces every non-trivial note to explain the situation, not just the answer.

2. **Context section made mandatory for non-TIL notes.** This is the "why should I care" anchor. Without it, the note is a fact floating in space. With it, future-you knows when this note is relevant.

3. **Article format got a Discovery section.** For deeper investigations, the journey matters as much as the destination. What you tried first, what failed, what clue led forward. This teaches pattern recognition, not just pattern matching.

## Solution

Updated both the CLAUDE.md knowledge capture rules and the `knowledge-capture` SKILL.md with the new type system:

| Type | Depth | When to use | Context required? |
|------|-------|-------------|-------------------|
| TIL | < 200 words | Pure gotcha, no investigation needed | 1 sentence |
| Atomic Note | 200-500 words | **Default.** Problem investigated, insight found | Yes, 1-3 sentences |
| Article | 500+ words | Multi-concept discovery, debugging journey | Yes, full situation |
| Definition | 100-300 words | "What is X" reference card | Brief |

Then re-pushed both original notes in the new Atomic Note format. The hook schema note went from a bullet list with a table to a note that explains what we were building, what broke, and why the values differ per event type. The redundant API note went from a code pattern description to a note that explains the rate limit concern, shows the actual call flow, and offers three fix options with tradeoffs.

## Key Takeaway

The default depth of a capture system determines the quality of your knowledge base. If the default is shallow, everything trends shallow because "just push it quick" wins every time. Making the default medium-depth (Atomic Note with mandatory Context) means every note at minimum answers "why does this matter" and "when would I see this again." You can still drop to TIL for true gotchas, but you have to consciously choose shallow instead of accidentally landing there.