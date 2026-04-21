---
title: "four days of Go"
date: 2017-07-31
captured: 2017-07-31T17:04:21Z
tags: [golang, language-review]
source: "GitHub issue tieubao/til#312 + http://www.evanmiller.org/four-days-of-go.html"
aliases: []
status: refined
---

## Context

Evan Miller, coming from C/Erlang background, spent four days building Hecate (a hex editor) in Go. This is his candid evaluation of the language's strengths and pain points as experienced by a newcomer.

**Source:** [Four Days of Go](http://www.evanmiller.org/four-days-of-go.html)

**Attachment:** [Four Days of Go.pdf](https://github.com/tieubao/til/files/1188272/Four.Days.of.Go.pdf)

## Practical strengths

- **Fast compilation.** Go's build speed and self-contained executables provide significant productivity gains over C/C++.
- **Familiar for C programmers.** Value/pointer semantics and syntax feel natural to C developers.
- **Good library ecosystem.** The termbox library demonstrated Go's suitability for terminal UI development.

## Criticisms

**Syntax inconsistencies.** The `:=` operator introduces ambiguity. It should dominate `=` in all contexts, but instead the two coexist with rules that feel arbitrary and error-prone.

**Missing features:**
- No ternary operator (deliberately excluded by the Go team)
- No polymorphic `Math` functions (float64 only)
- Compiler forbids unused variables and imports, which creates friction during exploratory programming

**Concurrency concerns.** Goroutines lack robust error handling in concurrent environments. Panics can corrupt shared memory state without proper recovery mechanisms.

## The strictness trade-off

Miller argues Go's design reflects a preference for enforcing coding discipline through the compiler rather than through team processes. This explains mandatory formatting (`gofmt`), forced variable/import cleanup, and resistance to language flexibility.

For exploratory programming and rapid prototyping, Go's strictness creates friction. For production systems prioritizing consistency across large teams, the enforced discipline delivers measurable value.

## Takeaways

- Go trades creative flexibility for team-scale consistency
- The language is opinionated by design, not by accident
- Coming from C, Go is approachable; coming from dynamic languages, expect friction
- The unused variable/import restriction is the most commonly cited annoyance by newcomers

## Related
- [[zen-of-go]] - the philosophy behind Go's design choices
- [[go-proverbs]] - Go's guiding principles that explain the strictness Miller critiques
