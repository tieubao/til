---
title: "Go 2 error handling draft design"
date: 2018-12-06
captured: 2018-12-06T14:19:40Z
tags: [golang, error-handling, language-design]
source: "GitHub issue tieubao/til#397 + https://go.googlesource.com/proposal/+/master/design/go2draft-error-handling.md"
aliases: []
status: refined
---

## Context

This is the official Go 2 draft proposal for reducing error-handling boilerplate while preserving Go's philosophy that "errors are values." The proposal introduces `check` and `handle` keywords. Note: this proposal was ultimately not accepted in this form, but it documents important design thinking about Go's error handling evolution.

**Source:** [Go 2 Draft: Error Handling](https://go.googlesource.com/proposal/+/master/design/go2draft-error-handling.md)

## The problem

Go developers repeatedly write `if err != nil { return err }`, which accounts for a significant portion of Go code. The boilerplate obscures the actual logic and creates opportunities for variable shadowing with repeated `err` declarations.

## Proposed design

**Check** replaces the `if err != nil` pattern:

```go
// Before
val, err := strconv.Atoi(s)
if err != nil {
    return err
}

// After
val := check strconv.Atoi(s)
```

**Handle** defines error processing in a lexical scope:

```go
handle err {
    return fmt.Errorf("parsing config: %v", err)
}
```

Handlers execute in reverse lexical order until one issues a `return`. A default handler exists in all functions returning `error`.

## Design trade-offs

**Advantages:**
- Reduced boilerplate without hiding error handling
- Better error wrapping via reusable handlers
- Explicit control flow (unlike exceptions, checks are clearly marked)
- Prevents variable shadowing from repeated `err` declarations

**Concerns:**
- Introduces context-dependent control-flow jumps (similar to `break`/`continue`)
- Adds conceptual complexity compared to the current pattern
- The handler chain execution order requires understanding

## What happened instead

The Go team ultimately went with simpler additions: `fmt.Errorf` with `%w` for wrapping (Go 1.13) and `errors.Is`/`errors.As` for unwrapping. The community consensus was that the `check`/`handle` proposal added too much complexity for the benefit gained.

## Takeaways

- Go's error handling verbosity is a deliberate trade-off for explicitness
- The `%w` verb and `errors.Is`/`errors.As` (Go 1.13+) address the wrapping problem without new syntax
- Language proposals often reveal more about design philosophy than the specific syntax proposed

## Related
- [[effective-error-handling-in-go]] - current best practices for Go error handling
- [[error-handling-in-upspin]] - real-world error handling design in a Go project
