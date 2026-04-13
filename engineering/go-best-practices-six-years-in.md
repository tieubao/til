---
title: "Go best practices, six years in"
date: 2016-05-04
captured: 2016-05-04T18:39:54Z
tags: [golang, best-practices, architecture]
source: "GitHub issue tieubao/til#225 + https://peter.bourgon.org/go-best-practices-2016/"
aliases: []
status: refined
---

## Context

Peter Bourgon's QCon London 2016 talk revisits his 2014 GopherCon best practices, examining which advice endured and which evolved. The central thesis: "make dependencies explicit" ties together configuration, testing, program design, and maintainability.

**Source:** [Go Best Practices, Six Years In](https://peter.bourgon.org/go-best-practices-2016/)

## Repository structure

Mature Go projects converge on a consistent layout:

```
pkg/  - libraries and reusable code
cmd/  - individual binary applications
```

This keeps artifacts go-gettable while providing space for non-Go assets (Docker configs, CI/CD, UI code).

## Explicit dependencies

The core principle is making dependencies visible. Hidden dependencies, especially package-global loggers and clients, create maintenance burdens:

```go
// Bad: hidden dependency
func (f *foo) process() {
    log.Printf("bar: %v", result)
}

// Good: explicit dependency
func (f *foo) process() {
    f.Logger.Printf("bar: %v", result)
}
```

Provide sensible defaults within constructors (nil checks inside `newFoo`, not at callsites). Make the zero value useful in config objects.

## Configuration

- Flag definitions belong exclusively in `func main`
- Environment variables are acceptable only when also exposed as flags
- Configuration should be discoverable and documented, never implicit

## Testing philosophy

Go's standard testing approach is preferred over DSL-based frameworks. "Testing in Go is just programming." Key strategies:

- Use many small interfaces to model dependencies
- Tests only need to test the thing being tested
- Table-driven tests provide clarity without complexity

## Dependency management

For binaries, use vendoring tools to lock dependencies. For libraries, never vendor dependencies (the consuming binary should control versions).

## Logging and instrumentation

Log only actionable information. Minimize log levels (info and debug are typically sufficient). Use structured logging. Where logging is expensive, instrumentation is cheap - apply the USE method (utilization, saturation, errors) for resources and RED method (request rate, error rate, duration) for endpoints.

## Build and deploy

Prefer `go install` over `go build` to leverage artifact caching. Use `GOOS` and `GOARCH` for cross-compilation. For containers, follow "FROM scratch" when possible.

## Related
