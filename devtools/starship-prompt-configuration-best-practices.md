---
title: "Starship prompt configuration best practices"
date: 2026-04-03
captured: 2026-04-03T18:38:14.880Z
tags: ["starship", "fish", "terminal", "dotfiles", "catppuccin"]
source: "Claude web session on dotfiles planning"
---
## Starship config best practices

### Start with a preset, customize from there

Don't build from scratch. Pick a preset that matches your terminal theme, then tweak:

```bash
starship preset catppuccin-powerline -o ~/.config/starship.toml
```

Available official presets (built into CLI): `nerd-font-symbols`, `pure-preset`, `pastel-powerline`, `tokyo-night`, `gruvbox-rainbow`, `catppuccin-powerline`, `jetpack`, `plain-text-symbols`, `bracketed-segments`, `no-runtime-versions`, `no-empty-icons`.

### Theme and icons are independent

The palette section only defines color names. The `symbol` field in each module defines the icon. You can have catppuccin colors with zero Nerd Font icons by setting `symbol = ""` or `symbol = "text "` on each module.

```toml
[git_branch]
symbol = ""         # no icon, just the branch name

[character]
success_symbol = "[ツ](mauve)"    # custom cursor
error_symbol = "[ツ](red)"
```

### $fill trick for single-line right-alignment

`right_format` only works on the cursor line in two-line prompts. For single-line prompts with right-aligned content, use the `$fill` module instead:

```toml
format = """$directory$git_branch$git_status$character$fill$time"""

[fill]
symbol = " "
```

This pushes everything after `$fill` to the right edge. One line, path+git on the left, time on the right. Learned from amanhimself.dev's Starship config post.

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

Best practice: if you use `mise` for version management, disable all language modules entirely. Mise handles versions; the prompt doesn't need to show them.

### Disable aggressively

Starship has 60+ modules. Most people need 5-8. Disable everything you don't actively use. In our dotfiles config, only 6 modules are active: directory, git_branch, git_status, git_state, cmd_duration, character, fill, time.

### Preset comparison (ranked by visual weight)

**Minimal (no Nerd Font needed):**
- `pure-preset`: two-line, blue dir, gray branch, purple cursor. Quietest.
- `plain-text-symbols`: default layout, text icons instead of glyphs.
- `bracketed-segments`: each segment in [brackets]. Structured but clean.
- monochrome (community): all gray tones, zero color.

**Medium:**
- `jetpack`: future default. Shows "on"/"via" connectors. $ cursor.
- `no-runtime-versions`: default minus language versions. Good for mise users.

**Heavy (needs Nerd Font):**
- `pastel-powerline`: colored pill segments, pastel palette.
- `tokyo-night`: muted blue-gray segments. Calmest powerline.
- `gruvbox-rainbow`: warm retro (orange/yellow/green). Distinctive.
- `catppuccin-powerline`: full catppuccin rainbow. Most colorful.

### Fish integration

Put at the END of config.fish (after all other config):

```fish
starship init fish | source
```

### Debugging

```bash
starship explain       # what each module shows
starship timings       # performance per module
starship print-config  # dump effective config
STARSHIP_LOG=debug starship prompt  # verbose logs
```

### Final config for dwarvesf/dotfiles

- Single-line prompt (not two-line)
- Pure-preset base layout (minimal, no connector words)
- Catppuccin Mocha palette
- `ツ` cursor in mauve (red on error, green in vim mode)
- `$fill` for right-aligned time
- Zero Nerd Font icons
- ~35 modules explicitly disabled
- Only active: directory, git_branch, git_status, git_state, cmd_duration, character, fill, time
- scan_timeout=30, command_timeout=500