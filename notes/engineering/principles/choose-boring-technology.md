---
title: "Choose Boring Technology"
date: 2025-07-18
captured: 2026-04-13T00:00:00Z
tags: [better-dev, architecture, decision-making]
source: "GitHub issue tieubao/til#615 + https://boringtechnology.club/"
aliases: []
status: refined
---

## Context

Dan McKinley's influential talk arguing that organizations should prioritize mature, well-understood technologies over newer alternatives. The central metaphor: you have a finite number of "innovation tokens" and should spend them on business problems, not infrastructure.

**Source:** [Choose Boring Technology](https://boringtechnology.club/)
**Attachment:** [Choose Boring Technology.pdf](https://github.com/user-attachments/files/21316341/Choose.Boring.Technology.pdf)

## Innovation tokens

Early-stage organizations typically have about three innovation tokens to spend on creative or difficult technical endeavors. Spending these on infrastructure choices (a new database, a new language, a new queue) means fewer resources for core business objectives. Every novelty carries a hidden operational tax that compounds over time.

## Known vs. unknown risks

Technology risk falls into two categories:

- **Known unknowns** - identified risks you can test or plan for
- **Unknown unknowns** - unforeseen failure modes

Mature technologies accumulate known unknowns over years, making failure modes predictable. Newer technologies harbor more unknown unknowns, creating hidden operational dangers that surface at the worst times.

## The mastery curve

Every tool follows a pattern: initial difficulty, discovery of problems, then eventual mastery. Switching tools to escape early frustration prevents reaching the productive phase where pain feels manageable because you understand the system deeply.

The paradox: "You should probably be using the tool that you hate the most. You hate it because you know the most about it."

## The real cost of diversity

Adding redundant technologies (like Redis alongside Memcached) carries permanent operational overhead. The benefits dominate only in rare cases, while maintenance costs typically outstrip the convenience gained by tool diversity.

## Decision-making framework

1. **Ask first:** "How would we solve this without adding new technology?"
2. **Write it down:** document the awkward workarounds required with existing tools
3. **Compare honestly:** weigh operational burden against genuine benefit
4. **Require dialogue:** technology choices need organizational conversation, not individual decisions
5. **Prove in production:** prove new technology at minimal risk before full adoption
6. **Commit to replacement:** if you add a redundant tool, commit to retiring the old one

## How to spot this

When a team proposes adopting a new tool, ask: is this spending an innovation token on infrastructure instead of the product? If the answer is yes, the bar should be very high.

## Related

- [[lessons-learned-in-software-dev]] - pragmatic engineering wisdom
- [[data-drives-code-structure]] - letting the problem shape the solution, not the tool
