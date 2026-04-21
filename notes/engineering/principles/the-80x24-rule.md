---
title: "the 80x24 rule for code formatting"
date: 2019-11-17
captured: 2019-11-17T05:30:49Z
tags: [code-style, readability, best-practices]
source: "GitHub issue tieubao/til#462 + https://blog.ploeh.dk/2019/11/04/the-80-24-rule/"
aliases: []
status: refined
---

## Context

Mark Seemann argues that writing small, constrained code blocks is the single most impactful programming practice. When asked for one piece of advice, he says: "Write small blocks of code."

**Source:** [The 80/24 Rule - Mark Seemann](https://blog.ploeh.dk/2019/11/04/the-80-24-rule/)

## The rule

- Maximum **80 characters per line** (industry standard width)
- Maximum **24 lines per method** (fits one VT100 terminal screen)
- These limits are deliberately arbitrary but grounded in cognitive science: humans can track approximately seven things simultaneously

Languages like Haskell or F# should use even stricter limits due to their density.

## Why it works

Small methods reduce mental overhead by limiting dependencies to understand, keeping cyclomatic complexity low, and fitting entire logic within working memory. Constraints nudge you toward better design by forcing delegation through well-named method calls rather than inline logic.

## Practical benefits

| Scenario | Impact |
|----------|--------|
| Code review (side-by-side diff) | Half-width display readability |
| Small devices / tablets | Better accessibility |
| Visual impairment | Reduces strain |
| Mob programming | Shared screen limitations |
| Remote presentations | Screen sharing compatibility |

## Actionable takeaways

1. Use editor guideline extensions to enforce width limits visually
2. Delegate complexity through well-named method calls rather than inline logic
3. Format strategically to respect width limits without sacrificing clarity
4. View constraints as features, not restrictions - they force better decomposition

## Related

- [[code-for-readability]] - broader principles on writing readable code
- [[rob-pike-five-rules-of-programming]] - related simplicity principles
