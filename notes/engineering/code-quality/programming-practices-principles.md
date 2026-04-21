---
title: "programming practices - Unix philosophy and beyond"
date: 2020-12-05
captured: "2020-12-05T09:55:16Z"
tags: [software-engineering, principles, unix-philosophy, design]
source: "GitHub issue tieubao/til#524"
aliases: []
status: refined
---

## Context

A curated list of programming principles, many rooted in the Unix philosophy. These are timeless guidelines for writing software that is simple, composable, and maintainable.

## The principles

1. **Prototype before polishing.** Get it working before optimizing it.
2. **Separate policy from mechanism.** Separate interfaces from engines.
3. **Write simple modular parts** connected by clean interfaces.
4. **Design programs to be connected** to other programs.
5. **Write programs to write programs** when you can.
6. **Design for the future**, because it will be here sooner than you think.
7. **Least surprise in interfaces.** Always do the least surprising thing.
8. **Silence is golden.** When a program has nothing surprising to say, it should say nothing.
9. **Fail noisily and early.** When a program must fail, it should fail loudly and as soon as possible.
10. **Write big programs only when demonstrated** that nothing else will do.
11. **Consider solving without adding.** Think about how you would solve the immediate problem without adding anything new.

## The thread connecting them

These principles share a bias toward simplicity and composability. They assume that the most dangerous thing in software is unnecessary complexity, and that the best code is code you did not have to write. The emphasis on clean interfaces and modular parts enables the Unix pipe-style composition that lets small tools solve big problems.

## Related
