---
title: "Shell values spliced into python3 -c source are an injection"
date: 2026-09-05
captured: 2026-09-05T09:40:00Z
tags: [bash, python, security, injection]
source: "Claude Code session"
aliases: [python3 -c interpolation, bash variable in inline python, source config file executes it, KEY=value config parsing]
status: refined
---

**A bash script that builds Python source by string interpolation (`python3 -c "... glob.glob('$src_dir/*.md') ..."`) hands every shell value to the Python parser as code.** A path containing a single quote, a backslash or a newline breaks the string literal at best and rewrites the program at worst. The bug is silent for ordinary paths and appears the first time a directory name carries punctuation.

## The safe form

Pass values through the environment and read them inside Python; the shell never touches the Python source:

```bash
SRC_DIR="$src_dir" VI_DIR="$vi_dir" python3 -c '
import glob, os
files = glob.glob(os.path.join(os.environ["SRC_DIR"], "*.md"))
'
```

Single-quote the Python so bash leaves it alone, and let `os.environ` do the crossing. The same applies to `perl -e`, `node -e`, `ruby -e` and to `jq` (use `--arg name "$value"`, never `".key == \"$value\""`).

## The sibling bug: sourcing a config file

`source book.conf` to read `KEY=value` settings executes the file as bash, so a value like `title="$(rm -rf ~)"` runs. Parse it as text instead:

```bash
title=$(grep -E '^title=' book.conf | cut -d= -f2- | tr -d '"')
```

or read it with `while IFS='=' read -r key value`. A config file is data; anything that evaluates it has turned it into code.

## How to spot it

Any `-c "..."` or `-e "..."` in double quotes with a `$` inside, and any `source` / `.` of a file a user or a job can edit. Both are the same mistake: data reaching an interpreter through the code channel.
