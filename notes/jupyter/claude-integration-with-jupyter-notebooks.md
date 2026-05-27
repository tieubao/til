---
title: "Claude integration with Jupyter notebooks"
date: 2026-05-18
captured: 2026-05-18T01:39:36.829Z
tags: ["jupyter", "claude-code", "mcp"]
source: "Claude.ai chat"
---
## When to use

You want Claude or Claude Code to help with notebook work: refactoring, exploration, debugging, conversion to scripts. There are three real ways to wire them together in 2026, each best for different things.

![Three ways to use Claude with Jupyter](https://assets.han-ws.workers.dev/i/2026/05/jupyter-claude-integration.svg)

## Option A: Claude Code with the built-in `NotebookEdit` tool

Claude Code has a native `NotebookEdit` tool that edits, inserts, or deletes cells in `.ipynb` files. Just run `claude` in a repo with notebooks.

Anthropic's own teams recommend running Claude Code and a Jupyter notebook open side-by-side in VS Code. Claude edits the source file; you switch to the notebook and run cells.

**What it does well:**
- Edits notebook source on disk
- Reads cells, including outputs (text and images)
- Refactors, adds markdown narrative
- Converts notebook to script via nbconvert workflow

**What it doesn't do:**
- Execute cells. It edits the file; you still hit Shift+Enter yourself.
- If a cell fails, Claude doesn't know unless you tell it or re-run and let it read the new output.

This is the simplest, lowest-overhead path. Right default for cleanup/refactor work.

## Option B: JupyterLab extensions (jupyter-ai or notebook-intelligence)

### jupyter-ai (official Jupyter project)

Gives you a chat sidebar (Jupyternaut) and `%%ai` magic commands. Example:

```python
%%ai anthropic-chat:claude-opus-4-7
Refactor this cell to use polars instead of pandas.
```

Commands:
- `/generate`: creates a whole notebook from a prompt
- `/learn`: builds a local FAISS vector index from your files for RAG-style querying

### notebook-intelligence (NBI, third-party, more capable)

Explicitly integrates with Claude Code. In Claude mode, NBI uses the Claude Code CLI for the chat panel, and Claude models (via the Anthropic API) for inline chat and auto-complete. This brings Claude Code's tools, skills, MCP servers, and custom commands into JupyterLab.

It even shows your Claude Code session history per-project: the chat sidebar has a history icon next to the gear, which lists Claude Code sessions recorded for the current working directory.

```bash
pip install notebook-intelligence
jupyter lab
```

For someone already using Claude Code with skills/MCP, NBI is the more natural fit than jupyter-ai.

## Option C: Jupyter MCP server (the agentic loop)

This is the most powerful option, and it fixes the biggest weakness of Option A. Datalayer publishes an open-source Jupyter MCP Server. Once registered with Claude Code, it exposes the notebook file AND the running Jupyter kernel to Claude.

The workflow becomes truly agentic:

> Claude creates the notebook, runs it top-to-bottom, hits import errors, runs `%pip install` for missing packages, re-runs the notebook, fixes the next error, and so on without human intervention.

### Setup

```bash
# In your notebook project directory
claude mcp add jupyter -- uvx jupyter-mcp-server@latest

# Verify
claude mcp list
# jupyter: uvx jupyter-mcp-server@latest - ✓ Connected
```

### CLAUDE.md template for notebook projects

Drop this in the repo root. Claude Code reads it automatically.

```markdown
## Rules for Claude when working with Jupyter notebooks

### Tool preference
- Use the Jupyter MCP for all `.ipynb` operations: read, edit, insert, delete, execute.

### Outputs
- Never print secrets, API keys, tokens, or passwords into cell output.
- Large outputs consume tokens and fill up your context window. Prefer summaries (`.head()`, `.shape`) over dumping full DataFrames.

### Execution
- When installing packages, use `%pip install` inside the notebook (not `!pip install`) so packages install into the running kernel.
- Execute cells to verify they work. Do not assume the code is correct.
- If a cell errors, read the actual traceback before attempting a fix. Do not guess.

### State and reproducibility
- Jupyter kernels are stateful. A notebook that runs top-to-bottom after "Restart & Run All" is the only notebook that works. Verify this before declaring a task done.

### Data safety
- Do not modify or delete raw data files. Write derived data to a separate path.
```

## Tradeoffs

| Option | Best for | Weakness |
|--------|----------|----------|
| A: NotebookEdit | Quick edits, refactoring, narrative pass | Can't execute cells |
| B: jupyter-ai / NBI | Want chat next to code in Lab UI | Adds extension surface area |
| C: Jupyter MCP server | Full agentic notebook work | Setup overhead, kernel can fight Claude on state |

## Recommendation for a Claude Code + homelab setup

Use **Option C** (Jupyter MCP server + `CLAUDE.md`) as the power-user default. It composes cleanly with existing Claude Code skills and MCP infrastructure.

Use **Option A** for one-off cleanup ("clean up this notebook before I commit") where the overhead of starting a server isn't worth it.

Skip Option B unless you specifically want chat-in-the-Lab-UI as a working mode. NBI is the right pick within B since it can route through Claude Code anyway.

## Key takeaway

The capability hierarchy is: edit-only (A) → chat-in-UI (B) → full kernel control (C). Option C is what turns Claude into a real notebook agent that can hit errors, install packages, re-run, and self-correct. The `CLAUDE.md` rules are not optional. Without them Claude will make stateful mistakes that look fine in the file but break on Restart & Run All.

## Related

- [[jupyter-architecture-kernel-server-frontend]] - the three processes each integration option plugs into
- [[jupyter-usage-patterns-and-friction-points]] - the friction points Claude can solve (refactor, narrative) or amplify (hidden state)
- [[notebook-landscape-2026-jupyter-alternatives-and-competitors]] - Marimo's plain-`.py` format is dramatically easier for Claude than `.ipynb` JSON
- [[managing-claude-codes-agent-view-background-sessions]] - companion workflow note for managing the Claude side of the loop
- [[commands-vs-hooks-vs-skills-decision-framework]] - how to wire Claude Code into a project, applied to notebook contexts