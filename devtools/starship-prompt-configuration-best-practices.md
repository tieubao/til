---
title: "Starship prompt configuration best practices"
date: 2026-04-03
captured: 2026-04-03T17:59:39.128Z
tags: ["starship", "fish", "terminal", "dotfiles", "catppuccin"]
source: "Claude web session on dotfiles planning"
---
## Starship config best practices

### Start with a preset, customize from there

Don't build from scratch. Pick a preset that matches your terminal theme, then tweak:

```bash
starship preset catppuccin-powerline -o ~/.config/starship.toml
```

Available presets: `nerd-font-symbols`, `pure-preset`, `pastel-powerline`, `tokyo-night`, `gruvbox-rainbow`, `catppuccin-powerline`, `jetpack`, `plain-text-symbols`.

### Theme and icons are independent

The palette section only defines color names. The `symbol` field in each module defines the icon. You can have catppuccin colors with zero Nerd Font icons by setting `symbol = ""` or `symbol = "text "` on each module.

To go fully icon-free: use `plain-text-symbols` preset as a base, then override colors with your catppuccin palette. Or just set symbols manually:

```toml
[git_branch]
symbol = ""         # no icon, just the branch name

[nodejs]
symbol = "node "    # plain text instead of nerd font glyph

[character]
success_symbol = "[>](bold green)"
error_symbol = "[>](bold red)"
```

### Performance tuning

Two critical timeout settings:

```toml
scan_timeout = 30        # ms, directory scanning
command_timeout = 500    # ms, external command execution
```

Use `starship timings` to find slow modules. Common offenders:

- **python**: runs `python --version`, can take 150ms+. Remove `$version` or disable.
- **nodejs**: runs `node --version`, slow on some systems. Same fix.
- **package**: reads package.json/Cargo.toml for version. Rarely useful in prompt.
- **git_status in huge repos**: git operations can timeout in repos >1GB.

Best practice: if you use `mise` for version management, remove `$version` from all language modules. Mise handles versions; showing them in the prompt is redundant and slow.

### Disable aggressively

Starship has 60+ modules. Most people need 5-8. Disable everything you don't actively use:

```toml
[package]
disabled = true    # slow, rarely useful

[aws]
disabled = true    # only if you switch profiles

[gcloud]
disabled = true

[battery]
disabled = true

[username]
disabled = true    # unless SSH

[hostname]
disabled = true    # unless SSH
```

### Two-line prompt

Info on top line, cursor on bottom. This prevents the cursor from jumping left/right as you cd between repos:

```toml
format = """
$directory $git_branch $git_status
$line_break
$character"""
```

### Right-side prompt

Fish supports `right_format` natively. Put low-priority info there:

```toml
right_format = """$time"""

[time]
disabled = false
format = "[$time]($style)"
time_format = "%H:%M"
```

### Fish integration

Put at the END of config.fish (after all other config):

```fish
starship init fish | source
```

### Debugging

```bash
starship explain     # what each module shows
starship timings     # performance per module
starship print-config  # dump effective config
STARSHIP_LOG=debug starship prompt  # verbose logs
```

### Key design decisions for the dwarvesf/dotfiles config

- Catppuccin Mocha palette (matches Ghostty theme)
- Zero Nerd Font icons (plain ASCII: `>`, `go`, `py`, `rs`, `node`)
- Two-line prompt: info on top, cursor on bottom
- Right side: time only
- Language modules show label only (no version, since mise manages versions)
- cmd_duration only when >500ms
- ~30 modules explicitly disabled for performance
- scan_timeout=30, command_timeout=500