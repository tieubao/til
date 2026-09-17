---
title: "matrix context is empty in a job-level if"
date: 2026-09-17
captured: 2026-09-17T00:00:00Z
tags: [ci, github-actions, workflow]
source: "Claude Code session"
aliases: ["github actions job level if matrix", "if matrix.os never matches", "conditional matrix leg"]
status: refined
---

A job-level `if:` is evaluated before the matrix is expanded, so the `matrix` context does not exist there. The expression still parses and still evaluates, to false, every time. The leg is never skipped and never reported as skipped: it simply runs, which reads as the condition having no effect rather than as a broken condition.

```yaml
jobs:
  build:
    # WRONG: matrix is not in scope here. Always false, so nothing is filtered.
    if: matrix.os != 'windows'
    strategy:
      matrix:
        os: [linux, windows]
```

To run a leg conditionally, select the matrix itself instead of filtering after expansion:

```yaml
strategy:
  matrix:
    os: >-
      ${{ github.event_name == 'pull_request'
          && fromJSON('["linux"]')
          || fromJSON('["linux","windows"]') }}
```

`fromJSON` is what makes this work: the expression has to produce a real array, not a string that looks like one.

`actionlint` catches the original mistake. A workflow change that adds a job-level condition is worth one `actionlint` run, because the runtime gives no signal at all.

## Key takeaway

A context that is out of scope in a GitHub Actions expression does not raise; it evaluates empty. Any comparison against it is silently false, so a filter written in the wrong scope fails open.

## Related

- [[checks-that-report-success-while-verifying-nothing]] - same family, a check that reports a clean result while verifying nothing
