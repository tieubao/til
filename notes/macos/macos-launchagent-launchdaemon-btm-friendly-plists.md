---
title: macOS LaunchAgent/LaunchDaemon authoring for a BTM-friendly identity
date: 2026-07-18
captured: 2026-07-18T09:59:52
tags: [macos, launchd, btm, code-signing, plist]
source: personal ops tooling conventions
aliases: ["BTM-friendly plists", "launchd plist authoring for BTM"]
status: refined
---

## Context

Companion to [[how-macos-code-signing-actually-works]]: that note explains why BTM (Background Task Manager, System Settings -> General -> Login Items & Extensions) needs a real Apple-issued cert to show a developer identity instead of "Unknown Developer". Getting the cert right is necessary but not sufficient. A launcher wrapped in `/bin/sh`, or named with a `.sh` extension, still reads wrong in BTM regardless of signing, because BTM's name and icon come from the plist's own fields, not from the signature.

## The Problem

Three common defaults, left alone, produce a LaunchAgent/LaunchDaemon that BTM displays badly even when the underlying script is legitimate and correctly signed:

- Wrapping the entry point in `/bin/sh` or `/bin/bash` as `ProgramArguments[0]` collapses every agent under one generic "sh" row, hiding the actual tool name.
- Naming the launcher script with a `.sh` extension renders it with the generic blank-page icon instead of the green-on-black executable badge.
- Using `#!/usr/bin/env bash` as the shebang ties TCC (Transparency, Consent, and Control) grants to whatever bash binary `env` resolves to at grant time, which can silently change later.

## What I Found

Three rules fix all three, and they compound: BTM reads the plist's own `ProgramArguments` for the name and icon, and the cert's identity chain (see the companion note) only supplies the developer label.

1. **`ProgramArguments[0]` is the launcher's own absolute path**, never `/bin/sh` or `/bin/bash` as argv[0]. The script carries its own shebang and is `chmod +x`.
   ```xml
   <key>ProgramArguments</key>
   <array>
     <string>/Users/you/.local/bin/myagent</string>
   </array>
   ```
   Not `<string>/bin/bash</string><string>/Users/you/.local/bin/myagent.sh</string>`.

2. **The top-level launcher has no `.sh` extension.** `.sh` files get the generic blank-page icon in BTM; bare-name executables get the exec badge. Helper libraries that the launcher `source`s can keep `.sh`; only the entry point `ProgramArguments[0]` points at needs the bare name.

3. **The shebang is `#!/bin/bash` (the Apple-signed system binary), never `#!/usr/bin/env bash`.** macOS's TCC keys every permission grant (Full Disk Access, folder access, Reminders, Photos, etc.) to the resolved interpreter binary's identity. `env bash` under a launchd `PATH` typically resolves to a Homebrew-installed bash whose path and ad-hoc signature change on every bash upgrade; each upgrade silently invalidates the agent's TCC grants and forces a re-grant round in System Settings. `/bin/bash` is one stable TCC identity for the life of the machine. Cost: Apple's bundled bash is version 3.2, so a launcher can't use `declare -A`, `mapfile`/`readarray`, or `${var,,}`; keep the launcher thin and push anything needing modern bash features into a script it execs.

## How to Spot This

BTM shows a generic "sh" name, a blank-page icon, or the agent keeps losing a permission grant after a bash upgrade. Check the raw `ProgramArguments` with `launchctl print system/<label> | grep -i program` for a LaunchDaemon or `launchctl print gui/<uid>/<label> | grep -i program` for a LaunchAgent, then confirm visually in System Settings -> General -> Login Items & Extensions: the row should show the script's own name with the exec icon.

## Related

- [[how-macos-code-signing-actually-works]] - the signature-identity half of the BTM label; this note covers the plist-authoring half
