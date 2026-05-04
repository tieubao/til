---
title: "Opt-in beats all-in for coding-agent sandboxing on a developer laptop"
date: 2026-05-04
captured: 2026-05-04T16:25:00+07:00
tags: ["coding-agents", "sandboxing", "claude-code", "dev-experience", "security"]
source: "agentkernel + Hermes brainstorm 2026-05-04"
aliases: []
status: refined
---

**Wrapping every coding-agent invocation in a sandbox sounds safer but kills adoption within weeks. Per-trigger opt-in is the design that survives daily use.** This is the lesson from designing how to integrate `agentkernel` (microVM sandbox via Apple Containers) with Claude Code on a developer laptop in May 2026.

The naive impulse is "always sandbox; safer is better." On a daily-driver laptop, the host integrations Claude Code has - IDE plugins, browser automation MCPs, ssh-agent, 1Password biometric unlock, wrangler / op / gh creds - are exactly the things that don't work inside a sandbox. Wrapping every call by default means losing 70% of what makes the agent useful.

## The two variants laid out

| | Variant 1: All-in (every claude call sandboxed) | Variant 2: Opt-in (per-trigger sandbox) |
|---|---|---|
| Setup tax | 3-5 hours of bind-mount fiddling for ssh, gh, op, etc. before claude can do basic work | ~30 min once |
| Lost daily integrations | high (browser MCPs, IDE plugins, biometric op unlock) | none |
| Coverage | every claude session | only marked-risky sessions |
| Discipline required | low (always sandboxed) | high (remember to opt in) |
| Failure mode if drift | low (sandbox catches it) | medium (Han forgets to opt in for a risky task) |
| Cost-of-using-the-tool | painful, leads to "I'll just run claude on host this once" | low, sandboxing is the conscious choice you make for the risky case |

Variant 1 looks better on a security spec sheet. Variant 2 wins in practice because the daily friction of variant 1 leads to abandonment within weeks, and an abandoned security tool provides zero protection.

## Why setup tax kills variant 1

A Claude Code session in a known repo wants to read:

- `~/.ssh/` for git operations and SSH-based MCPs
- `~/.config/gh/` for `gh` auth
- `~/.config/op/` for 1Password CLI session (or biometric unlock via the desktop app)
- `~/.config/wrangler/` for Cloudflare deploys
- `~/.claude/` for skills, slash commands, memory
- `~/Library/Application Support/Claude/` for settings, MCP config
- The repo working tree (multiple repos)
- Sometimes the iCloud-synced `private/` folder for sensitive context

Every one of these needs an explicit bind-mount config in variant 1, AND mounting them in defeats much of the boundary you wanted (`~/.ssh/` is exactly the dir most worth keeping out of a sandbox). The first week is "why doesn't claude see X?" tickets you fix one at a time. By week three you've abandoned the wrapper because you "needed to ship" five times in a row.

## Why variant 2 works

The trigger criteria that justify opting in:

| Trigger | Why sandbox |
|---|---|
| Ingesting `_meta/inbox/` or external material | Untrusted content; could contain prompt-injection or instruct agent to run host commands |
| PR review where the agent will execute scripts from the PR | Code under review is untrusted by definition |
| Evaluating an unknown CLI / `npx` / `pip install` from a fresh source | Unknown package may hit network, modify shell config, install global state |
| Running tests on a fresh-cloned repo with unknown `agents.md` / `CLAUDE.md` content | Repo's agent instructions may direct dangerous behavior |
| Long autonomous loops where you step away mid-session | Reduce blast radius if attention drifts |
| Explicit `/sandbox <cmd>` invocation | Conscious opt-in for any case the above don't cover |

Default = host. Sandbox = the conscious choice you make for the risky case. The discipline cost is real but contained - it's a 5-second decision per session, not a 5-hour setup tax.

## The recovery layer

Variant 2's residual risk (you forget to opt in, or the agent has a bad day on host) is bounded by the recovery layer underneath: macOS Time Machine, off-host backups (restic to R2 or similar), GitHub branch protection. If a coding agent on host does something dumb, you restore from yesterday. If it force-pushes garbage, branch protection rejects.

That recovery layer is where the security spec goes when the prevention layer is opt-in. Variant 1 promises prevention-by-default; variant 2 promises prevention-when-you-trigger plus recovery-always. Operationally, variant 2's combined cost-of-using is dramatically lower, AND for the failure modes that actually occur (forgot to sandbox a routine task, agent did something silly on host), recovery handles it.

## Three failure modes I considered

1. **You forget to opt in for a genuinely risky task and the agent hits the host.** Recovery layer (Time Machine, branch protection) catches it. Reviewable in the post-incident retro; the trigger criteria gain a new entry if the failure was preventable.
2. **You opt in too aggressively (sandbox routine work) and lose productivity.** Trigger criteria get tightened. Easier to relax restrictions than to expand them.
3. **The sandbox tool itself is buggy.** This is real. agentkernel v0.16.0 + v0.18.1's Apple Containers backend has three documented isolation flags that silently no-op (per [[agentkernel-broken-flags-on-apple-containers]]). Variant 2 makes this less catastrophic - you weren't relying on those flags for routine work, only for the ones you opted into. Variant 1 would have built daily ops on top of broken flags.

## When variant 1 might still be right

Variant 1 makes more sense when:

- The user is genuinely untrusted (operator with their own agent that you don't control)
- The agent runs in a CI environment where host integrations don't exist anyway
- The work is already container-shaped (GitHub Actions runners, Docker-based dev environments)
- Compliance requires "always sandboxed" as an auditable property

Most laptop-based coding-agent use doesn't fit those. If yours does, variant 1's costs are absorbed by the structure of your setup. If yours doesn't, variant 2 is the design that survives.

## How to spot the failure mode early

Watch for these signals in the first month after deploying variant 1:

- Setup tickets accumulate ("claude can't see X")
- Users start sneaking around the sandbox ("just this once, on host")
- Velocity drops measurably on routine tasks
- The wrapper's documentation grows a "common workarounds" section that's actually "things that don't work"

If you see two of those signals, flip to variant 2 and document the trigger criteria. Sunk cost on the variant 1 setup is real but short - most of the bind-mount config translates directly into "what gets bind-mounted when the user opts in" for variant 2.

## The bigger lesson

For any always-on security control on a productivity tool, ask: "what does this break that the user relies on?" If the answer is "many things" and the user's recovery is "skip the tool for that case," you'll lose adherence within a month. Better to design opt-in triggers that are explicit enough that compliance is easy, and accept the residual risk via a recovery layer.

The cheapest security posture you don't follow is worth less than the medium security posture you actually use.

## Related

- [[agentkernel-broken-flags-on-apple-containers]] - concrete example of why "always sandbox" can build on broken assumptions
- [[threat-model-split-cross-tenant-isolation-vs-per-agent-damage-containment]] - variant 2 is a damage-containment design; understand the threat before picking the architecture
- [[macos-multi-user-cost-myth-gui-vs-service-users]] - adjacent design lesson: pick the boundary that fits the actual threat, not the most aggressive available
- [[apple-containers-overview-the-macos-native-microvm-runtime]] - the underlying tool variant 2 builds on, on macOS
