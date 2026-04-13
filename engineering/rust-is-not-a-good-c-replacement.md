---
title: "Rust is not a good C replacement"
date: 2019-03-25
captured: "2019-03-25T15:38:38Z"
tags: [rust, c, programming-languages, systems-programming]
source: "GitHub issue tieubao/til#413"
aliases: []
status: refined
---

## Context

Drew DeVault argues that Rust is a C++ replacement, not a C replacement. His framing: "Go is the result of C programmers designing a new language, and Rust is the result of C++ programmers designing a new language." The values of good C++ programmers are incompatible with the values of good C programmers.

**Source:** [Rust is not a good C replacement](https://drewdevault.com/2019/03/25/Rust-is-not-a-good-C-replacement.html)

## The complexity argument

Both Rust and C++ are "kitchen sink" languages that solve problems by adding more language features. C solves problems by writing more C code.

Estimated new features per year:
- C: 0.73
- Go: 2
- C++: 11.3
- Rust: 15

A Rust program written last year already looks outdated. A C program written ten years ago has good odds of being fine. Systems programmers want things that work, not shiny features.

## Specific problems with Rust as a C replacement

**Portability.** C is the most portable programming language. A new CPU architecture barely exists until it has a C compiler, which then unlocks a vast software ecosystem.

**No spec.** Without a specification, nothing keeps rustc honest. Any behavior could change tomorrow. There is no way to know if something is a feature or a bug until your code breaks.

**Single implementation.** C has many competing compilers that stress-test the spec and pin down corner cases. Rust has essentially one compiler.

**No stable ABI.** C has the System-V ABI agreed upon across platforms. Rust has no stable internal ABI, so everything must compile and link in one go on the same compiler version.

**Cargo is mandatory.** Rust's compiler flags are not stable, and attempts to integrate with other build systems have been met with hostility. Systems programmers spend a lot of time integrating things.

**Concurrency is overrated.** Serial programs have X problems; parallel programs have XY problems. A program using poll effectively is simpler and has orders of magnitude fewer bugs. "Fearless concurrency" allows you to fearlessly employ bad software design 9 times out of 10.

## The counter-position on safety

DeVault acknowledges Rust is safer but argues the tradeoffs are not worth it. Rewriting an entire program from scratch always introduces more bugs than maintaining the existing C program would.

## Key takeaway

C's eventual replacement will be simpler, not more complex. Go has made a substantial dent in C's problem space by specializing on certain classes of programs and addressing them with the simplest solution possible. The kitchen sink approach does not work for systems programming.

## Related
