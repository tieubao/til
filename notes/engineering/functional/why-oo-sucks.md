---
title: "why OO sucks - Joe Armstrong's critique"
date: 2019-04-24
captured: "2019-04-24T07:51:29Z"
tags: [oop, paradigm, functional-programming, erlang]
source: "GitHub issue tieubao/til#425"
aliases: []
status: refined
---

## Context

Joe Armstrong, creator of Erlang, wrote a well-known critique of object-oriented programming. His objections stem from the fundamental design choices OOP makes about binding data and functions together.

**Source:** [Why OO Sucks](http://www.cs.otago.ac.nz/staffpriv/ok/Joe-Hates-OO.htm)

## The four objections

**Objection 1: Data structures and functions should not be bound together.** Functions do things (imperative, sequential). Data structures just are (declarative). Functions are understood as black boxes transforming inputs to outputs. Since they are completely different types of things, it is fundamentally incorrect to lock them in the same cage.

**Objection 2: Everything has to be an object.** In a non-OO language, "time" is an instance of a data type with clear definitions. In OO, it must be an object with associated methods. These definitions do not belong to any particular object; they are ubiquitous and should be manipulable by any function in the system.

**Objection 3: Data type definitions are spread out all over the place.** In Erlang or C, you can define all data types in a single include file or data dictionary. In an OOPL, data type definitions are scattered across objects. Lisp programmers learned long ago: it is better to have a small number of ubiquitous data types and many small functions than many data types with few functions.

**Objection 4: Objects have private state.** State is the root of all evil. OOPLs chose to "hide the state from the programmer," which is the worst option. Pure declarative languages carry global state into and out of all functions, using mechanisms like monads or DCGs to hide it when convenient but providing full access when needed.

## Why OO became popular

1. It was thought to be easy to learn.
2. It was thought to make code reuse easier.
3. It was hyped.
4. It created a new software industry.

Armstrong sees no evidence for reasons 1 and 2. Reasons 3 and 4 are the real driving force: if a technology is bad enough to create a new industry to solve problems of its own making, it is good for the people who want to make money.

## Key takeaway

The Lisp principle applies broadly: prefer a small number of universal data structures with many functions over many specialized types with few functions. Separating data from behavior leads to simpler, more composable systems.

## Related
