---
title: "Redundant API pre-checks in wrapper functions"
date: 2026-03-28
captured: 2026-03-28T05:38:33.151Z
tags: ["architecture", "api-design", "performance", "anti-pattern"]
source: "Claude Code session - github-mcp-worker project, reviewing push_skill tool"
aliases: []
status: refined
---
## Context

While reviewing the `push_skill` tool in the `github-mcp-worker` project (a Cloudflare Worker that commits files to GitHub via the Contents API), the code review revealed that every file push was making twice the expected number of API calls. The worker is rate-limited at 5,000 GitHub API requests/hour, so unnecessary calls matter, especially when pushing a skill with multiple extra files.

## The Problem

The `push_skill` tool calls a local `getExistingFile()` function to check if a file already exists (to detect "add vs update" for the commit message and to compare content for no-op detection). Then it calls `createOrUpdateFile()` from the shared GitHub library, which internally does the exact same GET request to fetch the file's SHA before writing.

```
push_skill flow (2 API calls per file, 1 redundant):
  getExistingFile(path)     → GET /contents/path  (check #1: exists? content?)
  createOrUpdateFile(path)  → GET /contents/path  (check #2: get SHA, redundant)
                            → PUT /contents/path  (actual write)
```

For a skill with SKILL.md + 3 extra files, that's 8 GETs where 4 would suffice.

## What I found

This is a common pattern when a **wrapper function pre-checks a condition that the library function already checks internally**. It happens because:

1. The library function (`createOrUpdateFile`) was designed to be self-contained; it handles both create and update cases by checking for an existing SHA.
2. The wrapper (`push_skill`) needs additional information from that same check (the file content, for diffing), so it does its own GET.
3. Nobody noticed the overlap because the two checks live in different files.

**Three fix options:**

1. **Expose an optional SHA parameter** on the library function. The wrapper passes the SHA it already fetched, and the library skips its internal check. Minimal API change, biggest efficiency win.

2. **Split the library into check + write**. Export `getFileSha()` separately from `writeFile()`. The wrapper composes them explicitly. More flexible but more surface area.

3. **Accept the redundancy**. If the rate limit headroom is large and the code clarity of self-contained functions matters more than 1 extra GET per file, do nothing. Valid choice for a single-user personal tool.

The right fix depends on scale. For a personal MCP worker doing < 50 pushes/day, option 3 is fine. For a shared service, option 1.

## How to spot this

When a function calls an API to check state (exists? version? content?) and then passes the result to a library function that starts by checking the same state, you have a redundant pre-check. Look for this pattern whenever a wrapper function adds logic around a self-contained library call.

## Related

- [[why-knowledge-notes-need-context-not-just-facts]] - this note was one of the two original notes that revealed the "shallow capture" problem, leading to the context-first capture system
- [[youtube-transcript-extraction-from-cloud-containers]] - another example of debugging a multi-layer system where understanding which layer handles what prevents redundant work