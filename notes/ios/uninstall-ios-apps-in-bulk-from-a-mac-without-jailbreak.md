---
title: "Uninstall iOS apps in bulk from a Mac without jailbreak"
date: 2026-09-05
captured: 2026-09-05T09:40:00Z
tags: [ios, macos, cli, libimobiledevice]
source: "Claude Code session"
aliases: [ideviceinstaller uninstall, remove iphone apps from terminal, libimobiledevice app list, iphone app cull]
status: refined
---

**`ideviceinstaller` (libimobiledevice) uninstalls apps over the cable through the same installation service iTunes once used: no jailbreak, no developer account, one "Trust This Computer" tap.**

## Setup

```bash
brew install ideviceinstaller      # pulls libimobiledevice
# plug the phone in, unlock it, tap Trust This Computer
idevicepair pair                   # once
```

## List and uninstall

```bash
ideviceinstaller list --user > apps.txt       # bundle id, version, name; the old -l flag is gone
ideviceinstaller uninstall com.example.app    # one app
```

For a cull, keep a file of bundle ids (one per line) and loop:

```bash
while IFS= read -r bundle; do
  [ -n "$bundle" ] || continue
  ideviceinstaller uninstall "$bundle" && echo "removed $bundle"
done < to-remove.txt
```

One pass removed 104 apps (262 down to 158) with zero failures, roughly a second each. The phone shows each icon vanish as it goes.

## Limits worth knowing

- `--user` lists user-installed apps only; system apps are not removable this way.
- The metadata it returns (`--xml`) has no usage or last-opened dates, so "which apps are unused" has to come from somewhere else (see the iPhone Storage "Last Used" note).
- The device must stay unlocked for the first command after pairing; later commands work with the screen off.
