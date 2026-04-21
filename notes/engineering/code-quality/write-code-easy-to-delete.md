---
title: "write code that is easy to delete, not easy to extend"
date: 2017-12-14
captured: 2017-12-14T10:23:52Z
tags: ["software-design", "maintainability", "code-quality", "architecture"]
source: "GitHub issue tieubao/til#343"
aliases: []
status: refined
---

## Context

A programming essay by tef on programmingisterrible.com arguing that we should optimize code for deletion rather than extension. The central metaphor: lines of code are "lines spent," not "lines produced."

**Source:** [Write code that is easy to delete, not easy to extend](https://programmingisterrible.com/post/139222674273/write-code-that-is-easy-to-delete-not-easy-to)

## Key insight

Every line of code has a maintenance cost. Instead of building reusable software, build disposable software. The more consumers of an API you have, the more code you must rewrite to introduce changes. Managing how code fits together is a significant problem that gets harder as a project ages.

## The seven steps

**Step 0: Don't write code.** The easiest code to delete is code you never wrote. A million-line monolith is more painful than a ten-thousand-line one.

**Step 1: Copy-paste code.** It is good to copy-paste a couple of times rather than making a library function, just to understand how the code will be used. Once you make something a shared API, you make it harder to change. It is simpler to delete code inside a function than to delete a function.

**Step 2: Don't copy-paste code.** When you have copied enough times, pull it into a utility function. Keep the hard-to-delete parts (library code, collections, logging) as far away as possible from the easy-to-delete parts.

**Step 3: Write more boilerplate.** Duplicate parts of code to avoid introducing dependencies. Libraries that require boilerplate (network protocols, wire formats) are hard to delete, so keep business logic out of them.

**Step 4: Don't write boilerplate.** Wrap your flexible library with one that has opinions. The `requests` library in Python wraps `urllib3` this way, separating concerns between common workflows and low-level control.

**Step 5: Write a big lump of code.** Business logic is a never-ending series of edge cases and hacks. Sometimes one big mistake is easier to deploy than 20 tightly coupled ones. It is quicker to write ten big balls of mud than to polish a single turd.

**Step 6: Break your code into pieces.** Instead of grouping by common functionality, isolate by what each part does NOT share. We build modules not for reuse but for changeability. Each hard problem should be handled by one module.

**Step 7: Keep writing code.** Feature flags decouple feature releases from deployments. Google Chrome found that the hardest part of a regular release cycle was merging long-lived feature branches. Feature flags solved this by letting larger changes merge incrementally.

## The principle

The strategies of layering, isolation, common interfaces, and composition are not about writing "good software." They are about building software that can change over time. Good code is not about getting it right the first time. Good code is legacy code that does not get in the way.

## Related
