---
title: "Go best practices for production environments"
date: 2016-05-02
captured: 2016-05-02T06:48:34Z
tags: [golang]
source: "GitHub issue tieubao/til#219"
aliases: []
status: refined
---

## Context

Peter Bourgon's experience report from SoundCloud, originally a GopherCon 2014 talk (updated for 2016). SoundCloud ran Go in production for 2.5+ years across a service-oriented architecture with over a dozen gophers across half a dozen teams.

**Source:** [Go: Best Practices for Production Environments](https://peter.bourgon.org/go-in-production/)

## Development environment

Use a single global GOPATH (e.g., `$HOME`). Clone repos into their canonical paths within GOPATH and work there directly. Fighting this convention is not worth the hassle. Most SoundCloud engineers used vim or Sublime Text with GoSublime; nobody used an IDE.

## Repository structure

Keep things simple. Many production services are just half a dozen files in `package main`. Do not create structure until you demonstrably need it. For repos with multiple binaries (server, worker, janitor), put each in a separate `package main` subdirectory with shared code in its own package. Never include a `src/` directory.

## Formatting and style

Configure your editor to `go fmt` on save. Follow Google's Code Review Comments document, plus:

- Avoid named return parameters unless they significantly increase clarity
- Avoid `make` and `new` unless necessary or size is known in advance
- Use `struct{}` as a sentinel value (sets as `map[string]struct{}`, signal channels as `chan struct{}`)
- Break long function signatures one parameter per line

## Configuration

Use plain `package flag`. Define flags in `func main` to prevent reading them as globals, which forces strict dependency injection and makes testing easier. For 12-Factor apps, use a start script to convert environment variables to flags.

## Logging and telemetry

Use plain `package log`. Only log actionable information: serious errors needing human attention, or structured data for machine consumption. Everything else is telemetry. For telemetry, prefer **pull** (expvar, Prometheus) over **push** (Graphite, Statsd) - push metrics scale poorly because cost grows with infrastructure size.

## Testing

Use plain `package testing` with table-driven tests. No testing frameworks provided significant value. For integration tests, use build tags (`// +build integration`) with global flags for service addresses, run with `go test -tags=integration`.

## Validation pipeline

| When you do this | Run this |
|-----------------|----------|
| Save | `go fmt` or `goimports` |
| Build | `go vet`, `golint`, `go test` |
| Deploy | `go test -tags=integration` |

## Dependency management

For non-critical projects: `go get -d` and hope. For important projects: vendor dependencies. Ship binaries with a `_vendor` subdirectory and a blessed build that prepends it to GOPATH. Ship libraries with a `vendor` subdirectory and rewritten imports.

## Key takeaway

The most notable finding is how uninteresting the conclusions are. Lightweight, pure-stdlib conventions scale to large teams and diverse projects. You do not need custom error checking frameworks, testing libraries, or flag parsers just because your codebase grows. The standard idioms continue to function beautifully at scale. Go's greatest strength is structural simplicity - embrace it rather than circumventing it.

## Related
