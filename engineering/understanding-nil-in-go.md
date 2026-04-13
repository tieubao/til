---
title: "understanding nil in Go"
date: 2016-09-19
captured: 2016-09-19T09:33:21Z
tags: [golang, nil, zero-values]
source: "GitHub issue tieubao/til#260 + https://speakerdeck.com/campoy/understanding-nil"
aliases: []
status: refined
---

## Context

Francesc Campoy's talk explores nil as a feature of Go rather than a bug. Nil is the zero value for six types (pointers, channels, functions, interfaces, maps, slices), and understanding how each behaves when nil unlocks cleaner, more idiomatic code.

**Source:** [Understanding Nil](https://speakerdeck.com/campoy/understanding-nil)

**Attachment:** [understanding_nil.pdf](https://github.com/tieubao/til/files/479651/understanding_nil__1_.pdf)

## Zero values by type

| Type       | Zero Value |
|------------|-----------|
| Boolean    | false     |
| Numbers    | 0         |
| String     | ""        |
| Pointers, Slices, Maps, Channels, Functions, Interfaces | nil |

## The interface gotcha

A nil pointer assigned to an interface variable does not equal nil:

```go
var p *Person           // nil of type *Person
var s fmt.Stringer = p  // Stringer (*Person, nil)
s == nil                // false!
```

The interface holds a concrete type even when the underlying pointer is nil. Rule: return interface types directly, not concrete error types stored in variables.

## Practical nil patterns

**Nil pointer receivers** - methods can be called on nil receivers, enabling clean recursive patterns like tree traversal where `nil` represents an empty subtree.

**Nil slices** - safe to range over (zero iterations), and `append()` works without initialization. Often fast enough without pre-allocation.

**Nil maps** - safe to read from (returns zero value), but writing to a nil map panics. Useful as read-only empty maps.

**Nil channels** - receiving and sending both block forever. This is powerful in select statements: set a channel to nil to disable that case dynamically.

**Nil functions** - useful for lazy initialization and default behavior patterns (`if logger == nil { logger = log.Printf }`).

## Key takeaway

Rob Pike's Go proverb applies directly: "Make the zero value useful." Design methods to handle nil receivers gracefully, use nil maps and slices freely for read-only operations, and leverage nil channels to disable select cases. Return interface types rather than concrete error types to avoid the nil interface gotcha.

## Related
