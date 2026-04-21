---
title: "writing good commit messages"
date: 2017-11-17
captured: 2017-11-17T16:58:02Z
tags: ["git", "commit-messages", "best-practices"]
source: "GitHub issue tieubao/til#334"
aliases: []
status: refined
---

## Context

Guidelines from the Erlang/OTP project wiki on writing good commit messages, applicable to any project using Git.

**Source:** [Writing good commit messages](https://github.com/erlang/otp/wiki/Writing-good-commit-messages)

## Why it matters

Good commit messages serve three purposes:

1. Speed up the reviewing process
2. Help write good release notes
3. Help future maintainers (it could be you, five years from now) understand why a change was made

## Structure

```
Short (50 chars or less) summary of changes

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. The blank line separating the summary from the body
is critical; tools like rebase can get confused if you run the two
together.

Further paragraphs come after blank lines.

  - Bullet points are okay, too
```

## Do

- Write the summary in the imperative mode: "Fix", "Add", "Change" - not "Fixed", "Added", "Changed"
- Always leave the second line blank
- Line-break the commit message (for readability in `gitk`)

## Don't

- Don't end the summary line with a period; it is a title

## Tips

If it seems difficult to summarize what your commit does, it may include several logical changes or bug fixes. Split them into separate commits using `git add -p`.

## Related
