---
title: "agentkernel plugin install defaults to CWD, not user-global; use --global to avoid leaking files into your repo"
date: 2026-05-04
captured: 2026-05-04T15:35:00+07:00
tags: ["agentkernel", "claude-code", "gotcha", "mcp"]
source: "agentkernel + Hermes brainstorm 2026-05-04"
aliases: []
status: refined
---

**`agentkernel plugin install <agent>` writes its plugin files to the current working directory by default, not to user-global config.** If you run it from inside a git repo, three new untracked files appear:

- `<repo>/.claude/skills/agentkernel/SKILL.md`
- `<repo>/.claude/commands/sandbox.md`
- `<repo>/.mcp.json`

Now you have the agentkernel skill registered ONLY for sessions in that repo. Other projects don't see it. And if your `.gitignore` doesn't cover those paths, you risk committing them.

The fix is a single flag:

```bash
agentkernel plugin install --global claude
```

This installs at the user level instead:

- `~/.claude/skills/agentkernel/SKILL.md`
- `~/.claude/commands/sandbox.md`
- `~/.mcp.json`

User-global means the agentkernel skill, the `/sandbox` slash command, and the agentkernel MCP server all become available in every Claude Code session, regardless of which project's CWD you launched from.

Cleanup if you accidentally went CWD-scoped first:

```bash
# move the project-scoped files out (use trash on macOS to avoid rm -rf)
trash <repo>/.mcp.json <repo>/.claude/skills/agentkernel <repo>/.claude/commands/sandbox.md
# then re-install at user level
agentkernel plugin install --global claude
```

`agentkernel plugin install --help` documents the `--global` flag, but the default behavior surprises people the first time. The convention in many CLI tools is the opposite - `npm install` is local by default and `-g` is the override; in agentkernel you also need `-g`/`--global`, but the surprise is that "plugin install" reads as a configuration action that intuitively belongs at user scope.

## Related

- [[agentkernel-broken-flags-on-apple-containers]] - once installed, you also need to know which agentkernel flags actually work on macOS
- [[opt-in-beats-all-in-for-coding-agent-sandboxing]] - context for why you would install the agentkernel claude plugin in the first place
