---
title: "XDG Base Directory Specification"
date: 2026-04-02
captured: 2026-04-02T05:27:51.486Z
tags: ["xdg", "dotfiles", "macos", "linux", "standards", "chezmoi"]
source: "Claude.ai chat"
aliases: []
status: refined
---
## What is XDG

XDG stands for **X Desktop Group**, the original name of the organization now known as **freedesktop.org**. They published the XDG Base Directory Specification (first version August 2003, current version 0.8.0) to standardize where applications store user-specific files on Unix-like systems.

The "XDG" prefix survives in the environment variable names (`XDG_CONFIG_HOME`, `XDG_DATA_HOME`, etc.) even though the org rebranded decades ago.

## The directory structure

The spec defines 5 categories of user files, each mapped to a default directory:

| Category | Env variable | Default path | What goes here |
|----------|-------------|-------------|----------------|
| Configuration | `XDG_CONFIG_HOME` | `~/.config/` | App settings, preferences, keybindings. Analogous to `/etc` at system level. |
| Data | `XDG_DATA_HOME` | `~/.local/share/` | Portable user data: fonts, downloaded files, desktop entries. Analogous to `/usr/share`. |
| State | `XDG_STATE_HOME` | `~/.local/state/` | Machine-specific state: logs, command history, game saves, recently-used files. |
| Cache | `XDG_CACHE_HOME` | `~/.cache/` | Non-essential cached data. Safe to delete anytime. Analogous to `/var/cache`. |
| Runtime | `XDG_RUNTIME_DIR` | Set by pam_systemd | Sockets, named pipes. Must be owned by user with 0700 permissions. |

Additionally, `~/.local/bin/` is the conventional directory for user-specific executables.

Each app creates a subdirectory under these paths (e.g. `~/.config/ghostty/`, `~/.config/fish/`), keeping things organized per-application.

## Why it matters

**Before XDG:** Every app dumps dotfiles directly into `$HOME`. Run `ls -la ~` and you see 40-50+ hidden files and directories mixed together: `.bashrc`, `.vimrc`, `.gitconfig`, `.mozilla/`, `.docker/`, `.npm/`, `.ssh/`. Config, data, cache, and state all jumbled in one flat directory. No way to tell what's safe to delete, what to backup, what to sync across machines.

**After XDG:** Clean separation by purpose.

- Want to backup your configs? Copy `~/.config/`.
- Want to free disk space? Delete `~/.cache/`.
- Want to sync settings across machines? Sync `~/.config/` only, skip `~/.local/state/`.
- Want to wipe app data without losing config? Delete `~/.local/share/appname/` only.

## Adoption status (2026)

Modern tools follow XDG from day one:

- Ghostty: `~/.config/ghostty/config`
- Fish shell: `~/.config/fish/`
- Neovim: `~/.config/nvim/`
- Starship: `~/.config/starship.toml`
- chezmoi: `~/.local/share/chezmoi/` (data), `~/.config/chezmoi/` (config)

Legacy tools still use root-level dotfiles but some are migrating:

- Firefox: finally adopted XDG in version 147 (late 2025), closing a 21-year-old bug report from 2004
- Git: still uses `~/.gitconfig` by default but respects `XDG_CONFIG_HOME/git/config` as alternative
- SSH: still `~/.ssh/` with no XDG support

Tools that will likely never change: `.bashrc`, `.zshrc` (shell rc files are loaded by the shell binary itself, which predates XDG).

## Relevance to dotfiles management

When using chezmoi for dotfiles, XDG simplifies the mental model:

```
~/.local/share/chezmoi/           # chezmoi source (XDG_DATA_HOME)
  dot_config/                     # maps to ~/.config/
    ghostty/config                #   -> ~/.config/ghostty/config
    fish/config.fish              #   -> ~/.config/fish/config.fish
    starship.toml                 #   -> ~/.config/starship.toml
  dot_gitconfig                   # maps to ~/.gitconfig (legacy)
  private_dot_ssh/config.tmpl     # maps to ~/.ssh/config (legacy, templated)
```

chezmoi itself is XDG-compliant and handles both XDG-compliant apps (everything under `dot_config/`) and legacy dotfiles (root-level `dot_*` files) seamlessly.

The practical benefit: XDG-compliant tools cluster their configs under one directory, making it obvious what to track in your dotfiles repo vs what to ignore.

## Related

- [[starship-prompt-configuration-best-practices]] - Starship is a good example of an XDG-compliant tool, config lives at `~/.config/starship.toml`