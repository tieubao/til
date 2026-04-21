---
title: "no primitives - model domain concepts with types"
date: 2021-04-05
captured: "2021-04-05T06:13:42Z"
tags: [software-engineering, design, domain-modeling, code-smell]
source: "GitHub issue tieubao/til#544"
aliases: [primitive-obsession]
status: refined
---

## Context

A lesson on the well-known code smell "Primitive Obsession" and why creating domain types instead of using raw primitives leads to better, safer code.

**Source:** [Lessons re-learned: No Primitives](https://gtramontina.com/posts/lessons-re-learned-1-no-primitives.html)

## The problem

It is tempting to use primitives (strings, numbers) instead of creating new types. When we do this, we miss an opportunity to model a domain concept and raise the abstraction level. We end up writing validation routines scattered everywhere, and if we have to remember to validate, we are bound to forget.

## Domain type examples

- **Age** - not just any number. Represents time alive, must be positive, may need to be in years, months, or weeks depending on the domain.
- **Email** - not just any string. Needs a specific format, possibly with a display name attached.
- **Money** - not just a number. Requires a Currency (itself not just a string). Math operations can only be performed on moneys of the same currency. Amount may be captured in cents.

These compose further: an Account model can be raised to SourceAccount and DestinationAccount for bank transfers.

## Eliminating parameter order bugs

With primitives:

```
save(productID string, name string)
// interface: save(string, string)
```

An honest mistake is calling it with arguments reversed. With domain types:

```
save(id ProductID, name ProductName)
// interface: save(ProductID, ProductName)
```

Now you do not need to depend on knowing internal variable names or parameter order. This is especially valuable in compiled languages or languages with type hints.

## How to spot this

Whenever you find yourself writing validation routines for the same concept in multiple places, you have likely missed a domain type. Wrap the primitive in a type that enforces its constraints at construction time.

## Related
