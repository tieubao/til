---
title: "a closer look at Go's type system"
date: 2018-12-06
captured: 2018-12-06T18:35:38Z
tags: [golang, type-system]
source: "GitHub issue tieubao/til#398 + https://medium.com/@ankur_anand/a-closer-look-at-go-golang-type-system-3058a51d1615"
aliases: []
status: refined
---

## Context

Go's type system is simpler than most languages but has specific rules around named types, unnamed types, underlying types, and assignability that trip up developers. This article by Ankur Anand breaks down these mechanics.

**Source:** [A Closer Look at Go (golang) Type System](https://medium.com/@ankur_anand/a-closer-look-at-go-golang-type-system-3058a51d1615)

## Type classification

**Named types** include predeclared types (`int`, `string`, `bool`, etc.) and user-defined types created via `type` declarations. A named type is always distinct from any other type.

**Unnamed types** are composite types defined by type literals: arrays, structs, pointers, functions, interfaces, slices, maps, channels. For example, `[]string` and `map[string]int` are unnamed.

## Underlying types

Every type has an underlying type:

- Predeclared types and type literals have themselves as underlying type
- For user-defined types, the chain resolves to the first unnamed type encountered

```go
type MyInt int       // underlying type: int
type YourInt MyInt   // underlying type: int (not MyInt)
```

## Assignability rules

Go uses **name equivalence** for defined types and **structural equivalence** for unnamed types. The key rule: values can be assigned between variables when both share the same underlying type AND at least one is not a named type.

```go
type Celsius float64
type Fahrenheit float64

var c Celsius = 100
var f Fahrenheit = c  // ERROR: different named types
```

Both have `float64` as underlying type, but both are named, so assignment fails. Explicit conversion is required: `Fahrenheit(c)`.

## Struct conversion

Struct conversions require identical underlying types, meaning field types must match exactly. Wrapping a field in a new named type breaks conversion eligibility, even if the underlying types are the same.

## Takeaways

- Use named types to create type-safe abstractions around primitives (e.g., `type UserID int64`)
- Go requires explicit conversion, never implicit coercion
- Understand the "at least one unnamed" rule for assignability
- Named types prevent accidental mixing of semantically different values

## Related
- [[typed-nils-in-go]] - interface type mechanics and nil behavior
- [[debating-type-systems]] - broader context on type system design choices
