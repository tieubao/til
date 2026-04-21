---
title: "writing great documentation for open source"
date: 2017-02-11
captured: "2017-02-11T18:23:19Z"
tags: [documentation, open-source, writing, better-dev]
source: "GitHub issue tieubao/til#294"
aliases: []
status: refined
---

## Context

Andre Staltz (creator of Cycle.js and contributor to RxJS) answered a question about how to write great documentation for open source libraries. His advice distills documentation into a teaching problem.

**Source:** [staltz/ama#21](https://github.com/staltz/ama/issues/21)

## Key principles

**Start with empathy.** View the empty README from the eyes of another developer. Ask yourself "what would they expect to read here?" and fill in accordingly. Documentation is not about what you know; it is about what the reader needs.

**Linearize non-linear concepts.** Concepts in any library form a graph of interrelated ideas, but documents are linear. The hardest part is figuring out which concept to address first so that each subsequent topic builds on the previous one. Pretend the reader only needs to learn one new thing per step. Sometimes you have to hide a concept temporarily, almost "lying" to the reader that they are not missing anything, to keep the progression feeling natural.

**Write a table of contents first.** Draft section headings as questions the reader would ask: (1) how to install, (2) how to write a hello world, (3) how to build a counter app, (4) how to use an advanced feature. Then write "answers" to each heading. The content follows naturally.

**Use conversational style.** Breaking formal English rules is fine if it helps the message land. Starting a sentence with "But" or tossing in expressions like "Wait a second, wasn't foo supposed to be 123?" makes documentation feel like a conversation, not a lecture.

**Use plenty of examples.** Humans learn example-first, concepts-later. Children learn by imitating adults and peers. Many examples help readers see common patterns, which translate into the core concepts of the library.

## Takeaway

Great documentation is a teaching problem, not a writing problem. Empathy for the reader, a carefully linearized progression, and generous examples matter more than formal prose.

## Related
