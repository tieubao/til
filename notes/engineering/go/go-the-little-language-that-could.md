---
title: "Go - the little language that could"
date: 2016-05-17
captured: 2016-05-17T17:34:35Z
tags: [golang]
source: "GitHub issue tieubao/til#228"
aliases: []
status: refined
---

## Context

Richard Eng's 2016 analysis of Go's meteoric rise in language rankings, exploring why simplicity and pragmatism drove adoption despite critics who wanted more language features.

**Source:** [The Little Language That Could](https://medium.com/@richardeng/the-little-language-that-could-61eaa62b5e0a)

## The rise in rankings

By 2016, Go had climbed to #15 on Redmonk, #13 on IEEE Spectrum, and 8th-9th on CodeEval and ad hoc indices - surpassing Swift, Scala, and far ahead of Haskell and Rust. The TIOBE index was an outlier due to a pathological decision to search for "Google Go programming" instead of "Go programming," which nobody actually searches for.

## Why simplicity wins

Go's success is not explained by Google's backing alone. Google has backed plenty of failed languages (AtScript died, Dart languished, GWT never took off). Programmers care about utility over branding.

Rob Pike's 2012 keynote articulated Go's philosophy:

- Go is about **software engineering**, not programming language research
- Syntax is the user interface of a language; Go was designed for **readability and clarity**
- The focus on simplicity and composability resulted in a productive, fun language

Dave Cheney at Gophercon India added the crucial insight: what makes Go successful is **what was left out** just as much as what was included. Critics demand new languages push language theory forward, but that is really a request to include their favorite features from older languages. Go deliberately resists this.

As Pike put it: "less is exponentially more."

## Historical precedent

Go is not the first language to pursue radical simplicity. Per Brinch Hansen's Edison language (1981) took a similar approach, deliberately eliminating reals, subrange types, variant records, files, pointers, goto statements, case statements, for statements, and more. Niklaus Wirth's Oberon followed the same philosophy a few years later.

## The pragmatism argument

Every language design involves tradeoffs. There will always be critics because no language can be perfect. The real question is whether programmers find the language useful. Go's adoption numbers answer that clearly.

The problems Google solves - large server software, clarity at scale, tooling-friendly syntax - are the same problems most large companies face. That alignment is a primary driver of Go's popularity.

The author's personal experience echoes this: picking up Go in 2014, he was "utterly astonished" by the speed of learning compared to C, C++, Java, Python, and Smalltalk. Go's clean syntax, efficient tooling, and fast compilation gave it a dynamic language feel while remaining statically typed.

## Related
