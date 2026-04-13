---
title: "idiomatic Go"
date: 2016-09-28
captured: 2016-09-28T09:03:16Z
tags: [golang, style, conventions]
source: "GitHub issue tieubao/til#264 + https://dmitri.shuralyov.com/idiomatic-go"
aliases: []
status: refined
---

## Context

Dmitri Shuralyov's guide supplements Go's official style guide with nuanced conventions drawn from Go's standard library and core projects. These are the small decisions that make Go code feel native rather than translated from another language.

**Source:** [Idiomatic Go](https://dmitri.shuralyov.com/idiomatic-go)

## Naming conventions

| Category          | Pattern                                        | Rationale                              |
|-------------------|------------------------------------------------|----------------------------------------|
| Error variables   | `ErrSomething` (exported); `err` (local)       | Clarity and simplicity                 |
| Acronyms          | `OAuth`, `GitHub` (exported); `oauth` (unexported) | Lowercase all letters when unexported |
| Unused receivers  | `func (foo) method()`                          | Signals receiver fields are not accessed |
| Collections       | Singular forms: `example/`, `image/`           | Matches Go project folder structure    |

## Spelling and formatting

Use American spellings matching Go project conventions: "marshaling" (not "marshalling"), "canceling" (not "cancelling"). Single spaces separate sentences in comments.

- Human-readable comments: `// This is a comment` (with space after `//`)
- Compiler directives: `//go:generate` (no space) - the spacing signals purpose

## Mutex hat pattern

Position mutex fields directly before the variables they protect, eliminating the need for explicit comments:

```go
rateMu     sync.Mutex
rateLimits [categories]Rate  // implicitly protected by rateMu
```

## Small but significant choices

- Empty string checks: prefer `s == ""` over `len(s) == 0` for readability
- Flag package: omit `os.Exit(2)` in `flag.Usage` callbacks since the package handles termination
- Prioritize consistency with established Go conventions over personal preference

## Key takeaway

Idiomatic Go prioritizes consistency with the standard library and readability through deliberate style choices. When in doubt, look at how the Go project itself does it.

## Related
