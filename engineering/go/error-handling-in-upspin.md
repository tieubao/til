---
title: "error handling in Upspin"
date: 2018-01-21
captured: 2018-01-21T11:34:09Z
tags: [golang, error-handling, design]
source: "GitHub issue tieubao/til#348 + https://commandcenter.blogspot.com/2017/12/error-handling-in-upspin.html"
aliases: []
status: refined
---

## Context

Rob Pike and Andrew Gerrand describe how the Upspin project (a distributed storage system) designed a custom error handling approach in Go. Rather than using the standard `error` interface with string messages, they built a structured `errors.Error` type that carries rich context through the call stack and across network boundaries.

**Source:** [Error handling in Upspin](https://commandcenter.blogspot.com/2017/12/error-handling-in-upspin.html)

**Attachment:** [Error handling in Upspin.pdf](https://github.com/tieubao/til/files/1649712/Error.handling.in.Upspin.pdf)

## The custom Error type

The `errors.Error` struct has five optional fields:

| Field  | Purpose                                      |
|--------|----------------------------------------------|
| `Path` | The resource path affected                   |
| `User` | The user involved in the operation           |
| `Op`   | The operation name (method or function)      |
| `Kind` | Error classification (Permission, IO, NotExist, etc.) |
| `Err`  | The underlying error (supports nesting)      |

## Key design decisions

**Type-based constructor.** The `errors.E()` function accepts arguments in any order and dispatches by type. This makes error construction concise and readable without positional boilerplate.

**Operational tracing, not stack traces.** Errors record the sequence of operations (system components) involved, not execution call stacks. In a distributed system, knowing "client > server > storage" is more useful than a goroutine stack trace.

**Network-aware marshaling.** `MarshalError` and `UnmarshalError` preserve the full error structure across RPC boundaries. Clients can programmatically inspect server-side error context.

**Template matching for tests.** The `Match()` function tests errors against templates, ignoring irrelevant fields. This prevents brittle tests that break when error messages change.

**Deduplication on nesting.** When wrapping errors, duplicate fields (like Path) are automatically stripped from inner errors to keep messages clean.

## Principles

- Errors are for users, not just for programmers. Design messages with end-user understanding as the primary concern.
- Custom errors fit custom needs. Generic error packages cannot address system-specific requirements.
- Use distinct types (PathName vs UserName) to catch programming errors at compile time.
- Avoid stack traces in distributed systems. Operational context is more actionable.

## Related
