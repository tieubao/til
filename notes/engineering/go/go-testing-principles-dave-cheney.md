---
title: "Go testing principles by Dave Cheney"
date: 2019-05-04
captured: 2019-05-04T03:09:35Z
tags: [golang, testing]
source: "GitHub issue tieubao/til#429 + https://dave.cheney.net/paste/gopherchina-2019-testing-talk.pdf"
aliases: []
status: refined
---

## Context

Dave Cheney's talk at GopherChina 2019 on testing in Go. The talk covers principles and practices for writing effective tests in Go, drawn from Cheney's extensive experience in the Go community.

**Attachment:** [gopherchina-2019-testing-talk.pdf](https://github.com/tieubao/til/files/3143985/gopherchina-2019-testing-talk.pdf)

## Key principles

**Tests should be table-driven.** Go's testing style favors table-driven tests where inputs and expected outputs are defined in a slice of structs, then iterated. This makes adding new test cases trivial and keeps test logic DRY.

**Test behavior, not implementation.** Tests should verify what a function does, not how it does it internally. This makes tests resilient to refactoring.

**Use `testing.T.Helper()` for test utilities.** Mark helper functions so that test failure messages point to the caller, not the helper.

**Subtests with `t.Run()` for organization.** Named subtests provide clear failure messages and allow running individual cases with `-run` flag.

**Keep test files alongside source.** Go convention places `_test.go` files in the same package, giving tests access to unexported identifiers when needed. Use `_test` package suffix for black-box testing.

**Avoid test fixtures when possible.** Prefer constructing test data inline. When fixtures are necessary, use `testdata/` directory which the Go toolchain ignores during builds.

**Tests as documentation.** Well-written tests serve as executable examples of how the API should be used. The `Example` test functions in Go render directly in godoc.

## Takeaways

- Write tests that a new team member can read and understand the API from
- Table-driven tests scale better than individual test functions
- `t.Helper()`, `t.Run()`, and `t.Parallel()` are the workhorses of Go testing
- Prefer testing the public API surface over internal implementation details

## Related
- [[zen-of-go]] - Dave Cheney's broader Go philosophy
- [[go-best-practices-six-years-in]] - testing as part of overall Go best practices
