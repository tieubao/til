---
title: "a self-updating script runs the old bodies for one tick"
date: 2026-09-17
captured: 2026-09-17T00:00:00Z
tags: [shell, bash, devtools, debugging]
source: "Claude Code session"
aliases: ["script updates itself but old code runs", "self updating bash script", "update applied but behaviour unchanged"]
status: refined
---

A script that rewrites its own file mid-run keeps executing the version the interpreter already parsed. The new code on disk takes effect on the next invocation, not this one.

```bash
#!/bin/bash
say() { echo "OLD"; }
sed -i '' 's/OLD/NEW/' "$0"   # the file on disk now says NEW
say                            # prints OLD
```

That run prints `OLD` while `grep` on the same file shows `NEW`. Every piece of evidence about the file agrees the update landed, and the behaviour of the run disagrees, which is what makes it expensive to diagnose.

The trap is the self-check written in the same tick. An updater that patches itself and then verifies its own new behaviour will report failure on a successful update, or worse, pass because the old body happened to produce the same output. Neither result means anything.

Two rules:

- An updater updates and exits. Verification belongs to a separate invocation.
- Never edit a script's bytes in place while it runs. Write the new version to a temp file and rename it over the original, so the running process keeps a coherent file and the next run gets the new one atomically.

## Key takeaway

Code that is already parsed does not change when its file does. Split update from verify across two runs.
