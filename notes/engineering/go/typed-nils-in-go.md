---
title: "typed nils in Go"
date: 2017-08-11
captured: 2017-08-11T06:03:15Z
tags: [golang, interfaces, nil, gotchas]
source: "GitHub issue tieubao/til#320 + https://dave.cheney.net/2017/08/09/typed-nils-in-go-2"
aliases: []
status: refined
---

## Context

Dave Cheney documents one of Go's most common gotchas: typed nils causing unexpected behavior when nil concrete values are stored in interface variables. Every Go programmer discovers this the hard way.

**Source:** [Typed nils in Go 2](https://dave.cheney.net/2017/08/09/typed-nils-in-go-2)

## The problem

An interface value in Go holds two components: a type slot and a data slot. An interface is only equal to `nil` when both slots are nil. When a nil pointer of a concrete type is assigned to an interface, the type slot is filled, making the interface non-nil.

```go
var p *bytes.Buffer = nil
var r io.Reader = p
fmt.Println(r == nil) // false!
```

The interface `r` contains `(*bytes.Buffer, nil)`, which is not `(nil, nil)`, so the nil check fails. This violates programmer intuition.

## Where this bites

The most common scenario is returning a concrete nil pointer where an interface is expected:

```go
func getReader() io.Reader {
    var buf *bytes.Buffer
    // ... some logic that might not initialize buf
    return buf // returns non-nil interface with nil pointer inside
}

r := getReader()
if r != nil {
    r.Read(data) // PANIC: nil pointer dereference
}
```

The nil check passes because the interface is non-nil, but the underlying pointer is nil, causing a panic on method call.

## The fix

Always return the interface type's zero value directly:

```go
func getReader() io.Reader {
    var buf *bytes.Buffer
    if buf == nil {
        return nil // return bare nil, not a typed nil
    }
    return buf
}
```

Or avoid returning concrete types through interface returns entirely.

## Cheney's Go 2 proposal

Cheney argued that Go 2 should change interface-to-nil comparison semantics so that an interface holding a nil concrete value would compare equal to nil. This would align with programmer intuition and reduce a class of subtle bugs. The proposal reflects the tension between logical consistency and practical usability in language design.

## Takeaways

- An interface is nil only when both type and value are nil
- Never return a concrete nil pointer where an interface is expected
- When a function returns an interface, use bare `return nil` for the nil case
- This is a frequent source of production bugs in Go, especially in error-returning functions

## Related
- [[understanding-nil-in-go]] - broader coverage of nil behavior in Go
- [[go-type-system-closer-look]] - type system mechanics that explain why typed nils exist
