---
title: "the antipattern scripting language"
date: 2021-03-28
captured: "2021-03-28T07:21:00Z"
tags: [software-engineering, patterns, scripting, best-practices]
source: "GitHub issue tieubao/til#540"
aliases: []
status: refined
---

## Context

Hillel Wayne's argument that antipatterns are contextual. What counts as bad practice in enterprise code can be perfectly reasonable in scripts and throwaway code. This reframes "best practices" as context-dependent rather than universal.

**Source:** [The Antipattern Scripting Language](https://buttondown.email/hillelwayne/archive/c006df5e-05f0-4e61-8838-c5af827d17f2)

## Why antipatterns exist

God objects, balls of mud, global variables are antipatterns because they make changes harder, bugs sneakier, and communication more difficult. But this assumes specific properties of the codebase:

- There is (or will be) lots of code
- Multiple people work on it
- It will be modified as needs change
- It will be maintained for a long time
- It should be robust to errors and sad paths
- The code itself is an artifact we care about (needs tests, logging, benchmarks)

## The opposite context: scripts

Some code has the opposite properties:

- Small, two or three files at most
- Written by one person for their own use
- Solves a very specific problem
- If you need something similar later, you start over
- Only the happy path matters
- The code is a means to an end, not an artifact worth maintaining

Examples: a one-off math calculation, an editor customization script, a database migration you run once, scraping webpages for a data dump, munging text, a custom build pipeline.

## The insight

In this scripting context, the problems caused by antipatterns are not actually problems. If antipatterns help you finish the script faster, they become good ideas. If you can keep the entire program in your head, using global variables is easier than dependency injection.

This means scripting follows different "best practice" rules than conventional engineering. It is a different paradigm, not a lesser one.

## How to spot this

Before applying "best practices" to a piece of code, ask: what context is this code in? Will it be maintained? Will others read it? If the answer is no, optimize for speed of completion, not maintainability.

## Related
