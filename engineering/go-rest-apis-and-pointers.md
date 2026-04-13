---
title: "Go, REST APIs, and pointers"
date: 2016-04-20
captured: 2016-04-20T07:09:11Z
tags: [golang, rest-api, json, pointers]
source: "GitHub issue tieubao/til#214 + https://willnorris.com/2014/05/go-rest-apis-and-pointers"
aliases: []
status: refined
---

## Context

When building Go API clients that support partial updates (PATCH requests), the standard value types create ambiguity between "field not set" and "field intentionally set to zero value." Will Norris explains how pointer types solve this.

**Source:** [Go, REST APIs, and Pointers](https://willnorris.com/2014/05/go-rest-apis-and-pointers)

## The problem

Go types initialize to zero values (empty strings, false, 0). With `omitempty` JSON tags, two conflicting scenarios arise:

**Without omitempty**: a struct like `Repository{Description: "new desc"}` marshals all unset fields to zero values, potentially causing unintended changes (e.g., accidentally making a private repo public).

**With omitempty**: attempting to deliberately clear a field by setting it to `""` results in the field being omitted entirely, making it impossible to send intentional zero values.

## The solution: pointer fields

Using pointer types resolves the ambiguity because the zero value for any pointer is `nil`:

```go
type Repository struct {
    Description *string `json:"description,omitempty"`
}
```

This distinguishes between:
- **Unset fields**: `nil` (omitted from JSON)
- **Intentional zero values**: `""`, `false`, `0` (included in JSON)

## Trade-offs

| Benefit | Cost |
|---------|------|
| Properly implements PATCH semantics | Additional memory allocation overhead |
| Enables setting fields to zero values | Verbose developer experience (pointer creation) |
| Clear distinction between unset and empty | Clients must perform nil checks to prevent panics |

## When to use this

This pattern matters primarily for APIs supporting partial updates. If your API only does full-object PUT requests, pointer fields add complexity without benefit. Evaluate whether proper partial-update semantics justify the additional overhead for your use case.

## Related
