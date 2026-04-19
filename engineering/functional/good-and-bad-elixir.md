---
title: "good and bad Elixir"
date: 2021-06-14
captured: 2026-04-13T00:00:00Z
tags: [elixir, better-dev, code-quality]
source: "GitHub issue tieubao/til#556 + https://keathley.io/blog/good-and-bad-elixir.html"
aliases: []
status: refined
---

## Context

Chris Keathley presents patterns that improve Elixir code quality by prioritizing reusability, explicit error handling, and intentional function design. The post is opinionated and practical, focusing on what real codebases get wrong.

**Source:** [Good and bad Elixir](https://keathley.io/blog/good-and-bad-elixir.html)

## Patterns to avoid

### Piping side effects

Piping results into functions that perform side effects spreads error handling across multiple call sites, creating tightly coupled code. Instead, use `case` or `with` statements to handle errors explicitly in the calling function where sufficient context exists.

### Over-using `with` for error differentiation

Using `with...else` to handle multiple distinct error types couples error logic to the function and obscures control flow. Better: create a unified error type and use `case` statements when specific error handling matters.

### Piping into case statements

This reduces readability and obscures intermediate steps. Assign intermediate results to variables before pattern matching.

### Hiding higher-order functions

Wrapping `Enum.map` or `Enum.reduce` in named functions limits reusability and couples functions to specific call sites. Instead, expose transformation logic directly through pipelines with explicit higher-order function calls.

### Using `Map.get/2` over access syntax

`Map.get/2` locks implementation to a specific data structure, requiring extensive refactoring when types change. Bracket notation (`opts[:foo]`) provides flexibility across data structure types.

## Positive practices

- **Express expectations clearly:** use specific guard clauses like `is_binary(req)` rather than `not is_nil(req)` to document requirements
- **Raise exceptions strategically:** raise for unrecoverable errors rather than returning error tuples when callers cannot remediate
- **Better test assertions:** use `for` loops instead of `Enum.all?` assertions to get detailed failure diagnostics per element

## Core philosophy

Push error handling responsibility to functions with sufficient context to respond appropriately. Write functions that are reusable across call sites. Be explicit about what you expect and what can go wrong.

## Related

- [[code-for-readability]] - readability as the primary quality metric
- [[programming-practices-principles]] - general principles that apply across languages
