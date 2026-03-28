---
title: "Redundant API Calls Pattern - Check Before Library Already Checks"
date: 2026-03-28
captured: 2026-03-28T05:14:57.921Z
tags: ["architecture", "api-design", "performance", "anti-pattern"]
source: "Claude Code session - reviewing push_skill tool in github-mcp-worker"
---
## The Pattern

A wrapper function pre-checks a condition (e.g., "does this file exist?"), then calls a library function that internally performs the same check. Result: double the API calls for no benefit.

## Concrete Example

In `push_skill`, the tool calls `getExistingFile()` to check if a file exists and get its content, then calls `createOrUpdateFile()` which internally does another GET to fetch the SHA. For N files, that's 2N unnecessary API requests.

```
push_skill flow (before fix):
  getExistingFile(path)     → GET /contents/path  (check #1)
  createOrUpdateFile(path)  → GET /contents/path  (check #2, redundant)
                            → PUT /contents/path  (actual write)
```

## How to Spot

1. A function that calls an API to check state before calling another function
2. The called function also checks the same state internally
3. Often happens when the library function was designed to be self-contained

## Fix Options

- **Expose a lower-level API**: Add a variant that accepts an optional SHA, skipping the internal check when the caller already has it
- **Remove the wrapper check**: If the only reason was to detect "add vs update" for the commit message, find a cheaper way (e.g., try the PUT and check the response)
- **Cache the result**: If the wrapper check returns useful data (like content for diff), pass the SHA downstream to skip the redundant fetch