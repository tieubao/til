---
title: "chezmoi source vs target two-layer mental model"
date: 2026-04-29
captured: 2026-04-29T04:19:53.985Z
tags: ["chezmoi", "dotfiles", "cli", "macos"]
source: "Claude Code session, dfoundation repo"
---
## Context

I started using chezmoi for dotfiles. It mostly worked by following copy-paste recipes, but I never understood what `chezmoi add`, `chezmoi apply`, `chezmoi diff` actually meant in relation to each other. When I asked Claude what "add this file to chezmoi source" meant, the answer made the whole tool click. Capturing it.

## The two layers

Chezmoi keeps your dotfiles in two separate places on disk:

| Layer | Path on macOS | What lives here |
|---|---|---|
| Source | `~/.local/share/chezmoi/` | Your dotfiles repo. Files use prefixed names to encode metadata (mode, templating, encryption). This is what gets git-committed. |
| Target | `~` (home directory) | The actual files programs read. This is what zsh, ssh, git etc see. |

The source is never read by programs directly. Programs read the target. The source's job is to be the canonical, version-controlled description of what the target should look like.

## How files map between layers

```
SOURCE                                  TARGET
~/.local/share/chezmoi/                 ~
  dot_zshrc                  -------->    .zshrc
  dot_gitconfig.tmpl         -------->    .gitconfig    (templated)
  dot_ssh/
    config.tmpl              -------->    .ssh/config   (templated)
    config.d/
      private_mini           -------->    .ssh/config.d/mini   (mode 0600)
```

Source filenames carry metadata via prefixes:

| Prefix | Meaning |
|---|---|
| `dot_` | Becomes `.` in target |
| `private_` | Apply with mode 0600 |
| `executable_` | Apply with mode 0755 |
| `encrypted_` | Decrypt with age or gpg before writing |
| `.tmpl` suffix | Run through Go templating with per-machine values |

Filesystems do not carry mode or templating in a portable way, so chezmoi encodes them in the filename. Ugly but reliable across OSes.

## The four core verbs

| Command | Direction | Use when |
|---|---|---|
| `chezmoi add <file>` | target to source | You created or modified a live dotfile and want to bring it under chezmoi management |
| `chezmoi re-add <file>` | target to source | You edited a file directly in `~` instead of in the source, and want to sync that back |
| `chezmoi apply` | source to target | You pulled a source change (or edited the source) and want it propagated to live `~` |
| `chezmoi diff` | comparison | What would `apply` do? Empty output means source and target are in sync |

The mental model that finally stuck: **source is the spec, target is the build artifact**. Editing the spec, then applying, is the canonical flow. Editing the artifact directly works but you must `re-add` to write it back to the spec.

## How portability actually works

The chezmoi source directory is itself a git repository. Cross-machine sync goes:

```
Machine A                Machine B (new)
  ~/                       ~/  (empty)
  ^                        ^
  |                        |
  | chezmoi apply          | chezmoi init <repo-url>
  |                        | chezmoi apply
  |                        |
~/.local/share/chezmoi     ~/.local/share/chezmoi
  (git repo)                  (git repo, freshly cloned)
       \                          /
        \                        /
         \                      /
          ─── github.com/<you>/dotfiles ───
                  (the remote)
```

Without a remote and commits, `chezmoi add` only protects you on the same machine. The portability you actually want kicks in when you commit + push the source repo.

## Common gotcha: chezmoi add needs a follow-up

`chezmoi add ~/.ssh/config.d/foo` puts the file into the source directory but does NOT commit it to git. You still have to:

```
chezmoi cd
git add dot_ssh/config.d/private_foo
git commit -m "add foo ssh config"
git push
exit
```

Many people (me included, until I investigated) assume `chezmoi add` is a single-step "track this file forever" command. It is not. It is more like `cp file source/file`. The git layer is a separate concern.

## How to spot when this matters

You are working on or thinking about chezmoi if:

1. You edited a file in `~` and ran `chezmoi diff` and saw a delta you did not expect.
2. You set up chezmoi months ago, never committed, and a new machine cannot find your dotfiles.
3. You are wondering whether to track a file with `private_` prefix or with chezmoi's age encryption.
4. Something in `~` feels managed but `chezmoi managed` does not list it. Means the source does not have it. Run `chezmoi add` to bring it under management.

## Key takeaway

Chezmoi is two layers and four verbs. Source holds the spec with prefixed filenames; target is what programs read; `add` and `re-add` flow target to source; `apply` flows source to target; `diff` shows the gap. Cross-machine portability is a separate problem solved by treating the source as a git repo with a remote. Once you internalize the layer split, the tool stops being magic.

#chezmoi #dotfiles #cli #macos