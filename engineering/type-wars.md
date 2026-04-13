---
title: "type wars - the history of static vs dynamic typing"
date: 2016-05-02
captured: "2016-05-02T18:08:33Z"
tags: [type-systems, programming-languages, history, better-dev]
source: "GitHub issue tieubao/til#220"
aliases: []
status: refined
---

## Context

Uncle Bob (Robert C. Martin) traces the six-decade war between static and dynamic typing, from Frege's logic through Fortran, C, Pascal, C++, Smalltalk, Java, and into the modern era of Ruby, Python, Go, and Swift.

**Source:** [Uncle Bob - Type Wars](http://blog.cleancoder.com/uncle-bob/2016/05/01/TypeWars.html)

## The pendulum swings

**Origins.** Types predate computers. Frege's logical system (late 1800s) was found to allow ambiguous statements (Russell's paradox). Types were proposed as a fix, but Godel's incompleteness theorems dashed those hopes in 1931.

**Fortran (1960s).** Two types: fixed point (integer) and floating point. Variables starting with I-N were integers. No mixed-mode expressions. The distinction existed because of hardware.

**C wins over Pascal (late 1970s).** C had types but did not enforce them. Pascal enforced strong typing. Assembly programmers saw Pascal as a "language for babies." C won decisively.

**C++ reasserts types (mid 1980s).** After a decade of C's type ambivalence, debugging wrong-type function calls in large programs became painful. C++ brought strong static typing back.

**Smalltalk's dynamic typing.** Smalltalk was also strongly typed, but types were enforced at runtime, not compile time. The C++ community dismissed runtime type errors as equivalent to segfaults.

**IBM vs Sun (1990s).** Capers-Jones research showed Smalltalk programmers were 2-5x more productive than C++ programmers. IBM bet on Smalltalk for the internet; Sun bet on Java (C++ Lite). Sun won on the "type safety" argument. Smalltalk died as a mainstream language.

**The revenge.** Smalltalk programmers had solved the "missile problem" by inventing Test Driven Development. They infiltrated Java shops and taught TDD. Java programmers using TDD started asking: "Why am I satisfying type constraints when my unit tests already check everything?" Many jumped to Ruby and Python.

## Uncle Bob's prediction

TDD is the deciding factor. With 100% unit test coverage, static type checking becomes redundant. As TDD becomes accepted as a professional discipline, dynamic languages will become preferred. The Smalltalkers will eventually win.

## Takeaway

The static vs dynamic typing debate is not about correctness; it is about which safety net you trust. Types and tests solve overlapping problems. The industry keeps swinging between them.

## Related
