---
title: "working as a software developer"
date: 2019-10-17
captured: "2019-10-17T04:09:20Z"
tags: [software-engineering, career, practices]
source: "GitHub issue tieubao/til#460"
aliases: []
status: refined
---

## Context

Henrik Warne, a professional developer, gave a talk to first-year engineering students about the realities of professional software development versus university programming. The talk covers challenges of production software, management techniques, and paths to improvement.

**Source:** [Working as a Software Developer](https://henrikwarne.com/2012/12/12/working-as-a-software-developer/)

## Characteristics of production software

- **Programs are big.** Production codebases run into millions of lines. The sheer size complicates everything.
- **Software is never done.** Successful software grows continuously for years with multiple developers touching the same code.
- **Complexity from aggregation.** Individual features are simple, but their interactions create subtle bugs. Complexity comes from the aggregation of many simple parts.
- **Reading code matters more than writing it.** Before modifying a program, you need to understand it. A well-designed program is one that is relatively straightforward to modify.

## How to manage

- **Modularize.** Split software into subsystems and layers so smaller chunks can be dealt with at a time.
- **Iterate.** "A complex system that works is invariably found to have evolved from a simple system that worked." (John Gall)
- **Self-documenting code.** Good naming of classes, methods, and variables lets you understand what a program does just by reading them.
- **No duplication.** Code duplication causes problems when modifying the program later. Combine logic into one method.
- **Unit testing.** Ensures smallest parts work as expected and, as a side effect, forces better code structure.
- **Version control.** Track different working versions and know exactly what code is in each release.
- **Write for people first, computer second.** "You shouldn't need to figure out code. You should be able to read it." (Steve McConnell)
- **Plan for failure.** Build in logging and error handling from the start. Errors will happen.

## Becoming a better programmer

- **Program.** The best way to learn is to actually write code, not just read about it.
- **Learn a scripting language.** Being able to write quick scripts to automate tasks or filter log files is invaluable.
- **Learn an IDE and text editor well.** The goal is to go from thought to program as effectively as possible.
- **Read books.** Code Complete (McConnell) and The Pragmatic Programmer (Hunt, Thomas) are essential reading.

## Fun facts from practice

- HashMap and ArrayList are the two most-used Java data structures in real codebases.
- Around 30% of developer time is scheduled for bug-fixing (including investigation and testing).
- No UML in practice, but whiteboards are used constantly for discussing solutions.
- People interactions are significant even in pure coding roles.

## Related
