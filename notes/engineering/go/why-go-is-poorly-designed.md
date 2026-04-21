---
title: "why Go is a poorly designed language"
date: 2015-10-29
captured: 2015-10-29T04:10:18Z
tags: [golang, language-design, criticism]
source: "GitHub issue tieubao/til#54 + https://medium.com/@tucnak/why-go-is-a-poorly-designed-language-1cc04e5daf2"
aliases: []
status: refined
---

## Context

Ian Byrd's critique identifies seven design flaws in Go. Despite harsh criticism, the author concludes he will continue using Go for its community and tooling, making this a useful catalog of known rough edges rather than a dismissal.

**Source:** [Why Go Is a Poorly Designed Language](https://medium.com/@tucnak/why-go-is-a-poorly-designed-language-1cc04e5daf2)

## The seven complaints

**1. Slice manipulations are cumbersome** - without generics (at the time of writing), creating reusable insert/delete operations on slices requires verbose, type-specific code.

**2. Nil interface paradox** - interfaces can report as non-nil when they hold a nil concrete value. A common pitfall that confuses developers expecting straightforward nil-checking.

**3. Variable shadowing** - the `:=` operator creates unexpected shadowing within nested scopes. Code compiles and passes linting but silently uses the wrong variable.

**4. Interface slice incompatibility** - you cannot pass `[]ConcreteType` as `[]InterfaceType`, even when the concrete type satisfies the interface. This suggests interfaces lack full first-class support.

**5. Range loop by-value semantics** - range loops copy values rather than providing references. Modifying the loop variable does not affect the original collection, which is not immediately obvious.

**6. Compiler rigidity** - strict enforcement of minor rules (unused imports cause compilation failure) conflicts with rapid iteration during development.

**7. Code generation via magic comments** - `go:generate` relies on special comments to trigger code generation. Comments should explain code, not generate it.

## The paradox

Despite cataloging these flaws, Byrd concludes: "I'll continue to use Go... I hate the language, but I love community, I love tooling." This tension is common among Go developers who accept the language's deliberate trade-offs while wishing for improvements in specific areas.

## What has changed since

Several of these complaints have been addressed: generics arrived in Go 1.18 (2022), and the community has developed patterns and linter rules for variable shadowing. The nil interface gotcha and range value semantics remain as-is.

## Related
