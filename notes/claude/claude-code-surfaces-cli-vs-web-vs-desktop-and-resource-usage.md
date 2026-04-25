---
title: "Claude Code surfaces: CLI vs web vs desktop and resource usage"
date: 2026-04-25
captured: 2026-04-25T02:46:56.669Z
tags: ["claude-code", "devtools", "performance"]
source: "Claude.ai chat"
---
## Overview

There are four ways to run Claude Code in 2026, not three. The question "web vs terminal vs app" conflates two separate things: where the agent *process* runs (local machine vs Anthropic cloud) and what *surface* you interact with (shell, browser, GUI, phone).

The four surfaces:

1. **Terminal CLI** (`claude` command) — Node.js process running locally, full access to filesystem, shell, git, and PATH. Installed via `npm i -g @anthropic-ai/claude-code`.
2. **Web** (claude.ai/code) — agent runs on Anthropic's cloud infrastructure in a sandboxed VM with a fresh git clone. Sessions persist when the laptop is off.
3. **Desktop app** (Code tab in Claude app) — Electron GUI wrapper around the same engine as the CLI. Can spawn three *kinds* of sessions: local, remote (cloud), or SSH. macOS and Windows only, no Linux.
4. **iOS / Dispatch** — monitor and spawn sessions from the phone. The agent itself runs in the cloud or on a paired desktop.

## Where the agent runs

![Claude Code three surfaces comparison](https://assets.han-ws.workers.dev/i/2026/04/claude-code-three-surfaces.svg)

| Surface | Agent runtime | Local access | Best for |
|---|---|---|---|
| CLI | Your machine | Full filesystem, shell, git, PATH | Scripting, automation, headless agent SDK, Bedrock/Vertex/Foundry, plugins/hooks, Linux |
| Web | Anthropic cloud VM | None (fresh git clone only) | Long refactors, overnight test runs, multi-repo migrations, work from phone |
| Desktop | Local / cloud / SSH | Depends on session kind | Visual diff review, live preview pane, parallel git worktrees, computer use on macOS |

Key consequence: the desktop app is really a *control plane* for all three execution environments, not a separate runtime. The CLI and desktop app share config files (`CLAUDE.md`, `~/.claude/settings.json`, `.mcp.json`), so you can run both simultaneously on the same project with separate session history.

## Resource usage

Numbers come from developers monitoring their own machines via GitHub issues in March-April 2026. Not official benchmarks, but consistent across multiple independent reports.

| Surface | Idle RAM | Idle CPU | Active/heavy | Energy impact |
|---|---|---|---|---|
| Terminal CLI | ~150-500 MB per session | <1% | 400-800 MB, 5-10% CPU | Minimal. 20 concurrent CLI sessions = ~7-9% CPU on a 6-core machine |
| Desktop app | ~800 MB - 1 GB (renderer) | 40-120% sustained (reported) | 1.0-1.3 GB, 10%+ CPU, spikes to 367% during streaming | High. Fans spin up, machine runs hot even when app is idle and backgrounded |
| Web | ~150-300 MB (browser tab) | Low (just rendering) | Same as any streaming web app | Minimal on user machine. Agent VM runs on Anthropic's cloud |

### Why the desktop app costs so much more

![Claude Code resource architecture](https://assets.han-ws.workers.dev/i/2026/04/claude-code-resource-architecture.svg)

The desktop app runs the same Claude Code engine as the CLI, but wraps it in an Electron shell (Chromium renderer + Node.js main + GPU helper). That shell is where the extra weight comes from. The visual diff view, preview pane, sidebar, and embedded browser cost CPU and RAM whether actively used or not.

### Known performance issues (as of April 2026)

- **Desktop renderer CPU**: averages 68% CPU with peaks to 367% across multiple cores, *even when the app is idle and backgrounded with no active conversations* ([issue #32010](https://github.com/anthropics/claude-code/issues/32010))
- **Windows memory explosion**: renderer accumulates DOM nodes during streaming, memory climbs linearly to ~900 MB then explodes to 1,050-1,290 MB, freezing the UI for 17 seconds to 2+ minutes while the Claude Code backend continues working fine ([issue #31666](https://github.com/anthropics/claude-code/issues/31666))
- **Linux UI lag**: Electron zygote process alone uses 20% CPU and 600 MB RAM on an i9 with 64 GB RAM; CLI on the same machine is instantaneous ([issue #48299](https://github.com/anthropics/claude-code/issues/48299))

## Decision matrix

| Task | Surface |
|---|---|
| Slash commands, subagent pipelines, spec-driven dev (dwarves-kit workflow) | CLI |
| Headless agent execution on VPS (Hermes stack) | CLI via Agent SDK |
| Reviewing complex PR with many files | Desktop |
| 2+ hour refactor across multiple repos | Web or Desktop's Remote env |
| Quick fix from phone | Web or Dispatch |
| Plugins, hooks, third-party providers (Bedrock/Vertex/Foundry) | CLI only |
| Battery-sensitive coffee shop coding | CLI |
| Long-running overnight tasks | Web |

## The brutal take

The desktop app has real performance issues in early 2026. Multiple independent developers have filed bug reports showing sustained high CPU even when idle, memory explosions during streaming, and UI freezes on Windows and Linux. Visual features are nice, but there's a significant tax.

Recommended pattern for a laptop-based workflow with VPS agents:

1. **CLI for 80% of the day.** Matches custom kit workflows, spec-driven dev, and VPS agents. Only surface that supports plugins and hooks.
2. **Web for long-running cloud tasks.** Frees up local CPU/battery, runs overnight without the laptop needing to stay on.
3. **Desktop only when visual diffs or live preview are specifically needed.** Not a good daily driver.

The "CLI + web for async" pattern uses less than 1 GB RAM total on the laptop. The "desktop app as main interface" pattern uses 2x that minimum, and battery life takes a noticeable hit.

## Sources

- [Claude Code desktop docs](https://code.claude.com/docs/en/desktop)
- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [GitHub issue #32010](https://github.com/anthropics/claude-code/issues/32010) — desktop renderer CPU analysis
- [GitHub issue #31666](https://github.com/anthropics/claude-code/issues/31666) — Windows memory explosion
- [GitHub issue #48299](https://github.com/anthropics/claude-code/issues/48299) — Linux UI lag