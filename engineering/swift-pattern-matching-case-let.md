---
title: "pattern matching with case let in Swift"
date: 2019-02-08
captured: 2019-02-08T16:24:25Z
tags: [swift, pattern-matching, syntax]
source: "GitHub issue tieubao/til#409 + https://swiftwithmajid.com/2019/02/06/pattern-matching-with-case-let/"
aliases: []
status: refined
---

## Context

Swift's `case let` keyword enables powerful pattern matching across enums, optionals, and tuples. This provides elegant, readable syntax for destructuring and filtering complex data types.

**Source:** [Pattern Matching with case let](https://swiftwithmajid.com/2019/02/06/pattern-matching-with-case-let/)

## Enums with associated values

Extract associated values from enum cases with optional filtering:

```swift
case let .loaded(shows)                           // extract value
case let .loaded(shows) where shows.isEmpty       // extract + condition
```

## Optionals

Since optionals are enums (`Optional<T>`), pattern matching applies directly:

```swift
case let value?    // matches non-nil (equivalent to .some(value))
case .none         // handles nil
```

## Tuples

Pattern matching works with tuple grouping:

```swift
case ("admin", "admin")                                    // match specific values
case let (_, password) where password.count < 6            // wildcard + condition
```

## Control flow integration

`case let` integrates with multiple Swift constructs:

- **`if case let`** - conditional checks with extraction
- **`guard case let`** - early returns when pattern does not match
- **`for case let`** - loop filtering: `for case let .loaded(shows) in collection where condition`

## Key takeaway

Pattern matching with `case let` reduces boilerplate while improving code clarity. It replaces verbose `switch` statements and nested `if let` chains with concise, expressive syntax that works consistently across enums, optionals, and tuples.

## Related

- [[swifty-code]] - broader principles for idiomatic Swift
