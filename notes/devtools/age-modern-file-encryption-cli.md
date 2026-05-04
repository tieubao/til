---
title: "age, a modern file-encryption CLI"
date: 2026-04-22
captured: 2026-04-22T00:00:00Z
tags: ["age", "encryption", "cli", "security", "sops", "devtools"]
source: "Claude Code chat"
aliases: ["age encryption tool", "age homebrew"]
status: refined
---

## Context

`age` showed up unexplained in a `brew list`. It is often installed as a dependency of `sops`, `chezmoi`, or a dotfiles bootstrap, so most people end up with it without ever reading its man page. Worth knowing because the moment you need to put a secret into a git repo (or send one to a teammate), `age` is the tool with the least friction.

## What it is

`age` is a modern file-encryption CLI by Filippo Valsorda (ex-Go security lead). Think "small, opinionated replacement for `gpg -c` and `gpg -e`, for files only."

- Public-key crypto: X25519 + ChaCha20-Poly1305
- Keys look like `age1qyqszq...` (public) and `AGE-SECRET-KEY-1...` (private)
- SSH keys (`ed25519`, `rsa`) work as identities too, which is why it slots nicely onto machines that already have an SSH key
- No keyring, no subkeys, no revocation, no web of trust, no signing. Encryption only. Use `minisign` or `ssh-keygen -Y` for signatures.

## The shape

```
   plaintext file                    ciphertext (.age)
        |                                   |
        |   age -r <recipient-pubkey> ----->|
        |                                   |
        |<-- age -d -i <identity-keyfile> --|
```

## What people actually use it for

| Use case | Why age fits |
|---|---|
| Secrets at rest in a git repo | `sops` has a native age backend; standard in Flux/ArgoCD |
| Encrypted backups | Single binary, scriptable, no keyring state to corrupt |
| Sending one file to one person | `age -r age1abc... file > file.age`, done |
| Personal `gpg -c` replacement | Passphrase mode: `age -p file > file.age` |
| Encrypting state files for CI | Short-lived workflow identities, no PGP ceremony |

## Minimum workflow

```bash
brew install age
age-keygen -o ~/.age/key.txt            # public key printed to stderr
age -r age1abc... -o secret.age secret
age -d -i ~/.age/key.txt secret.age > secret
```

## Why pick age over GPG

GPG's problems are well-documented: ancient UX, sprawling spec, global keyring state, bad defaults, hard to automate. `age` throws all of that out and keeps only the 20% that covers "encrypt this file for that person." If you need PGP-compatible workflows (email, Debian package signing), you still need GPG. For everything else, `age` is the right default.

## How to spot when this applies

You need to put a secret into a place that should not see it in plaintext (git repo, backup archive, Slack DM, USB drive), and the recipient already has either an SSH key or can paste a one-line public key back to you. That is the whole domain of `age`.

## Related

- [[age-and-1password-complementary-encryption-tiers]] - architectural pattern that runs age alongside 1Password as a two-tier encryption stack
- [[saas-cto-security-checklist]] - secret management is one of the checklist items age operationalizes at the file level
- [[xdg-base-directory-specification]] - where the age key file should live (`$XDG_CONFIG_HOME/age/keys.txt`) if you care about dotfile hygiene
- [[chezmoi-source-vs-target-two-layer-mental-model]] - chezmoi's `encrypted_` prefix calls age (or gpg) under the hood; the dotfile-management daily-driver use case
