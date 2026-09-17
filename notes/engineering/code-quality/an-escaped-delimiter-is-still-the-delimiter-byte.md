---
title: "an escaped delimiter is still the delimiter byte"
date: 2026-09-17
captured: 2026-09-17T00:00:00Z
tags: [shell, parsing, bash, code-quality]
source: "Claude Code session"
aliases: ["markdown table escaped pipe bash", "IFS read phantom fields", "backslash pipe splits anyway"]
status: refined
---

A markdown table cell can contain `\|` to render a literal pipe. The escape is a rendering convention, not a transformation: the bytes on disk are a backslash followed by a pipe. Anything that splits the row on the pipe byte still splits there.

```bash
# row: | id | a \| b | status |
IFS='|' read -r _ id value status _ <<< "$row"
# value is "a \", status is " b ", and every later field is shifted by one
```

The damage is not a parse error. Every field after the escaped cell shifts by one position, so a field read by index returns a neighbouring cell's contents. Downstream that reads as the feature not applying, or as a row being in the wrong state, rather than as a parse bug, which is why it survives review.

Two ways out, in order of preference:

1. Do not read fields by position out of a hand-split line. Parse with something that knows the format's escaping rules.
2. If a shell split is genuinely the right tool, unescape or placeholder the escaped delimiter before splitting, and assert the field count afterwards so a shift fails loudly.

## Key takeaway

Escaping changes what a renderer displays, never what a byte-level splitter sees. A format whose delimiter can appear inside a field needs a parser, not `IFS`.

## Related

- [[checks-that-report-success-while-verifying-nothing]] - the general form: any delimiter expressible as content needs positional parsing
