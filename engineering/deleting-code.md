---
title: "deleting code"
date: 2016-04-14
captured: 2016-04-14T10:52:26Z
tags: [code-quality, refactoring, maintenance]
source: "GitHub issue tieubao/til#208 + http://nedbatchelder.com/text/deleting-code.html"
aliases: []
status: refined
---

## Context

Ned Batchelder's argument for permanently deleting unused code instead of disabling it through comments or conditionals. The core principle: version control is your safety net, not commented-out code.

**Source:** [Deleting Code](http://nedbatchelder.com/text/deleting-code.html)

## The problem with disabled code

Leaving disabled code in the codebase creates noise and uncertainty. Future developers encounter unanswered questions:

- Why was the original approach abandoned?
- Will we revert to it?
- How will we decide?

These questions slow down everyone who reads the code without providing useful context.

### Commented-out code

Creates ambiguity without explanation. Forces developers to guess intent. If you must keep it, add a comment explaining why it is temporarily preserved and when removal might occur.

### Conditional compilation

Equally problematic. Disabled code may be invisible to IDEs. Using undefined symbols as feature flags is weak documentation that hides dead code behind indirection.

### Uncalled methods

The decision depends on context. Framework code with public APIs may warrant keeping methods that are not currently called internally. Application code should be removed if nothing calls it.

## Rules for deletion

1. **Delete permanently** - use source control as your safety net, not commented code
2. **If keeping code, explain it** - add comments stating why it is temporarily preserved and when removal should happen
3. **Mark temporary removals** - use distinctive markers (e.g., `//-`) to locate accidentally disabled code before check-in
4. **Clean up thoroughly** - remove empty conditionals and dead variables when deleting code blocks
5. **Leave pointers when useful** - small comments referencing commit history can aid future searches without cluttering the codebase

## The bottom line

Source control eliminates the fear of losing code. Embrace deletion to maintain code clarity and reduce cognitive burden on your team. Dead code is not free; it costs attention every time someone reads past it.

## Related

- [[write-code-easy-to-delete]] - the philosophy of writing code designed to be disposable
- [[code-for-readability]] - reducing cognitive load through clean code practices
