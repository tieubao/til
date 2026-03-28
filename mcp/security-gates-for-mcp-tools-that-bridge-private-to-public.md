---
title: "Security gates for MCP tools that bridge private to public"
date: 2026-03-28
captured: 2026-03-28T04:06:22.021Z
tags: ["mcp", "security", "secret-scanning", "path-traversal", "claude-skills"]
source: "Claude Code session building push_skill tool for github-mcp-worker"
---
## The problem

When an MCP tool pushes content from a private environment (like Claude.ai with Notion connected) to a public GitHub repo, the tool itself is the last checkpoint before secrets go public. Client-side classification (e.g., a skill that labels content as SAFE/SENSITIVE) is helpful but not sufficient. The server must enforce.

## Three lessons from building `push_skill`

### 1. Server-side secret scanning is mandatory

Claude.ai skills often contain Notion database IDs, API tokens, and company-specific data because they were written assuming a private environment. When exporting to a public repo, the MCP worker must scan all content before committing. Hard block, no override flag.

### 2. Regex secret scanners need context anchoring

Bare hex patterns like `[0-9a-f]{32}` are useless as secret detectors. They match:
- Git SHAs in documentation
- Content hashes
- Hex color codes
- Random prose

Instead, match only when the hex string is assigned to an ID-like variable:

```
# Bad: too broad
/[0-9a-f]{32}/

# Good: context-anchored
/(?:database_id|page_id|spaceId)\s*[:=]\s*["']?[0-9a-f-]{32,36}["']?/i
```

The same principle applies to UUID patterns. A UUID in prose is probably fine. A UUID assigned to `space_id` is a leak.

### 3. Path traversal validation on LLM-provided file paths

Any MCP tool that accepts user-provided file paths needs traversal checks. Even in a single-user "trusted" setup, the caller is an LLM that can be prompt-injected or simply confused. A `files[].path` of `../../README.md` could overwrite files outside the intended directory.

Minimum validation:
- Strip leading slashes
- Reject any path containing `..`
- Reject absolute paths
- Constrain to the expected directory prefix

## Design pattern

```
Input validation (schema) → Secret scan (block) → Diff check (skip no-ops) → Commit
```

The secret scan sits between validation and the actual write. This way you get clean error messages for bad input (schema), security blocks for sensitive content (scan), and efficiency for unchanged content (diff).