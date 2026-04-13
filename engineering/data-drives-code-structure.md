---
title: "data drives code structure"
date: 2025-01-15
captured: "2025-01-15T11:31:02Z"
tags: [software-engineering, structured-programming, design]
source: "GitHub issue tieubao/til#609"
aliases: []
status: refined
---

## Context

A Quora answer by Andrew Bromage on how programmers know what to code before writing any code. The answer distills a core principle of structured programming that many experienced developers internalize but rarely articulate clearly.

**Source:** [How do programmers know what to code before they write any code?](https://www.quora.com/How-do-programmers-know-what-to-code-before-they-write-any-code/answer/Andrew-Bromage)

## The principle

The structure of a software component follows from the structure of the data it has to deal with. This is the starting point: look at the data, and it will dictate what has to be done first.

Examples of how data shape dictates code shape:

- If your data looks like an **array**, the code that deals with it looks like a **loop**
- If your data looks like an **algebraic data type**, the code looks like **recursion**
- If your data looks like a **graph**, the code looks like **graph traversal**

This is not a secret at all. It is the main idea behind structured programming: data has structure, and code that deals with structured data is also structured.

## Practical nuance

A lot of modern programming is not purely algorithmic. In the real world, the first thing often written is a mock-up user interface so designers can iterate on it. But the job of any nontrivial program is ultimately to transform data into other data. For the kind of work that will outlast "no code" tools, this is the fundamental skill.

## How to spot this

When you are stuck figuring out where to start on a new feature or system, stop thinking about the code. Instead, look at the data: what shape does it have? What transformations need to happen? The code structure will follow naturally from the answers.

## Related
