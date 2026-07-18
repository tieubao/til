---
title: "age and 1password as complementary encryption tiers, and the key-separation principle"
date: 2026-04-24
captured: 2026-04-24T12:24:54.958Z
tags: ["age", "1password", "encryption", "ssh", "backup", "security", "architecture", "key-separation"]
source: "Claude Code session on dotfiles S-38 SSH backup design"
---
## Context

I was building an offline escape hatch for my SSH keys (S-38 in the
dotfiles repo). 1Password is already my primary secret store and holds
the canonical copy of every SSH key I use, so the first question was
why I'd need a second layer at all. The answer is simple: if I ever
lose access to my 1Password account (lockout, legal hold, account
compromise, family-emergency access loss), every SSH key in that vault
goes with it. The escape hatch exists for exactly that scenario, and
it must not depend on 1Password being reachable.

The backup bundle contains SSH private keys in plaintext, so it has to
be encrypted. I reached for `age` because it's already in my
`home/dot_Brewfile.tmpl` and because `chezmoi` can consume age
identities for its `encrypted_*` files. Reusing one identity for two
use cases was cleaner than maintaining two parallel crypto setups.

## Problem

Where does age fit in a stack where 1Password already holds every
sensitive secret? And once an age key exists, how do you keep the
encryption from becoming ceremonial?

The second question is the interesting one. It has a crisp answer that
I want to remember: **if the encryption key and the encrypted payload
ever share a failure domain, the encryption is theater, not security.**

## Discovery

The investigation started with a dead end. My first attempt at the SSH
backup flow tried to import existing private keys into 1Password via
the `op` CLI:

```fish
op item create --category "SSH Key" --vault Private \
    --title "SSH - id_rsa" "private key=$privkey"
```

1P CLI 2.x rejects this with:

```
creating items through piped input is not supported for SSH Key items
```

Every variant (assignment syntax, template file, piped JSON) hits the
same block. Agilebits does this on purpose: SSH Key imports have to go
through the desktop app so the user is the one handling the private
key bytes, not a subprocess. The only CLI-automatable path for SSH Key
items is `--ssh-generate-key`, which creates a brand new key. Fine for
provisioning, useless for adopting existing keys.

That forced the architecture inversion. Instead of trying to push SSH
keys into 1P via CLI, I export them from 1P (which is CLI-automatable)
and encrypt the bundle with age. The backup direction is now
1P → filesystem, not filesystem → 1P.

Three things fell into place:

1. **age fits where 1P can't reach.** 1P is the source of truth when
   it's available; age handles the scenario where it isn't. They're
   complementary, not redundant.
2. **One age identity covers multiple use cases.** `~/.config/chezmoi/key.txt`
   is the same key `chezmoi` uses for `encrypted_*` files. No reason
   to generate a second one for SSH backups. Writing the code to
   prefer this path over silently generating a new identity was worth
   the extra branch; silent generation would leave two keys to track.
3. **The key-separation principle snaps into focus.** If I put the
   age key and the `.age` bundle in the same location, anyone who
   gets that location gets both. The bundle being encrypted stops
   mattering. This applies to iCloud, USB drives, email, any shared
   store. The bundle can live anywhere; the key must live somewhere
   else.

## Solution

The actual S-38 design:

```
    ┌─────────────────────────┐
    │  1Password vault         │  ◀── canonical SSH keys live here
    │   - GitHub SSH Key        │     plus a copy of the age key as
    │   - trading_vps SSH Key   │     a Document, so the key can be
    │   - id_rsa SSH Key        │     recovered on a new machine
    │   - chezmoi age key       │
    │     (Document + notes)    │
    └──────────┬──────────────┘
               │
               │ op item list + op item get
               ▼
    ┌─────────────────────────┐
    │  plaintext bundle        │  ◀── exists only as a 0600 mktemp
    │  (ephemeral, mktemp)     │     for the duration of encryption
    └──────────┬──────────────┘
               │
               │ age --encrypt -r <recipient>
               ▼
    ┌─────────────────────────┐
    │  ssh-keys-YYYY-MM-DD.age │  ◀── safe to put anywhere encrypted-
    │  (age-encrypted blob)    │     payload can tolerate being seen
    └─────────────────────────┘
```

Commands at the boundaries:

```fish
# Generate age identity once
age-keygen -o ~/.config/chezmoi/key.txt
chmod 600 ~/.config/chezmoi/key.txt

# Copy to 1P as a Document (so a new Mac can get the key back)
op document create ~/.config/chezmoi/key.txt \
    --title "chezmoi age key (dotfiles backup)" --vault Private

# Create the encrypted bundle
dotfiles ssh backup --destination ~/Documents/Claude/dotfiles-backup
# writes <dest>/ssh-keys-2026-04-23.age (bundle+README stays on iCloud)

# Restore on a new machine
age --decrypt -i ~/.config/chezmoi/key.txt \
    ~/Documents/Claude/dotfiles-backup/ssh-keys-2026-04-23.age \
    > /tmp/ssh-bundle.txt
# Manually paste each private-key block into a new 1P SSH Key item
# (op CLI cannot import SSH keys; this step is permanently manual)
rm -P /tmp/ssh-bundle.txt
```

Three storage tiers, each deliberate:

| Location | What's there | Why |
|---|---|---|
| `~/.config/chezmoi/key.txt` | age private key | Working copy; decrypts chezmoi encrypted files; required locally |
| 1P Document "chezmoi age key" | age private key | Recovery for a new Mac when 1P is available |
| `~/Documents/Claude/dotfiles-backup/` (iCloud) | `.age` bundle + README | Offline escape hatch; encrypted so iCloud is an acceptable host |

The age key deliberately does not live in iCloud. Putting it there
would place lock and key on the same service, and Apple account
compromise would decrypt everything in one step.

## Key Takeaway

age and 1Password solve different parts of the same problem. 1P gives
you availability, sync, and a nice UI. age gives you portability,
offline durability, and independence from any single provider. Running
them side by side gives a two-tier backup that only fails if both tiers
fail simultaneously, which is a far narrower threat than either alone.

The rule that makes the pattern work is general and worth carrying
into other designs: **an encryption boundary is only real if the key
and the ciphertext have different failure modes.** Co-locating them,
even for convenience, collapses the boundary silently. It still
*feels* encrypted, which is the most dangerous state to be in.

Corollary for future-me: when I add another `encrypted_*` file to the
dotfiles repo, I already have an age identity at
`~/.config/chezmoi/key.txt`. Reuse it. Don't silently generate a
second one.

## Related

- [[age-modern-file-encryption-cli]] - reference card for the age CLI itself
- [[saas-cto-security-checklist]] - secret-management hygiene at the org level
- [[xdg-base-directory-specification]] - where the age key file belongs on disk
- [[chezmoi-source-vs-target-two-layer-mental-model]] - chezmoi is the daily consumer of the age identity stewarded by this two-tier pattern
- [[secret-resolution-for-pi-agent-providers-via-1password-op-read]] - the `!op read` indirection that keeps pi agent provider keys out of plaintext config files, a concrete downstream use of the credential tier
- [[1password-backup-pattern-for-apple-dev-certs]] - same key-separation principle applied to a codesigning cert: passphrase and `.p12` archive split into two 1Password items so neither alone is useful