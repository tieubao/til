---
title: 1Password backup pattern for Apple developer signing certs
date: 2026-07-18
captured: 2026-07-18T09:59:52
tags: [macos, code-signing, security, 1password, backup]
source: personal ops tooling investigation, genericized
aliases: ["backing up a codesigning p12 to 1password", "apple dev cert 1password backup"]
status: refined
---

## Context

Companion to [[how-macos-code-signing-actually-works]]. An Apple Development or Developer ID Application cert lives as a private key + certificate pair inside one Mac's keychain. Existing signatures stay valid even if that Mac dies, because verification checks the cert chain embedded in the signature itself, not the live private key. What is lost is the ability to sign anything new or re-sign anything edited under that identity, and the private key cannot be re-derived. The cert needs a backup that survives the machine without ever landing on disk as an unencrypted archive.

## The Problem

A codesigning identity is two secrets that must travel together: the `.p12` archive (private key + cert, itself passphrase-encrypted) and the passphrase that unlocks it. Storing the passphrase next to the file defeats the encryption; storing the `.p12` unencrypted anywhere is worse. The backup also has to survive years of not being touched, so it must be retrievable without remembering which machine or session created it.

## What I Found

Split into two 1Password items, generated and uploaded in one flow, tagged identically so they are always found together:

```bash
# generate a random passphrase and store it as a Password item
P12PW=$(op item create --category=password \
    --title="Codesigning Cert Passphrase - <identity-name>" \
    --vault=<vault> \
    --tags="codesign,backup" \
    --generate-password='letters,digits,32' \
    --format=json | jq -r '.fields[] | select(.id=="password") | .value')

# export the identity from the keychain, encrypted with that passphrase
TMPFILE=$(mktemp)
security export -k ~/Library/Keychains/login.keychain-db \
    -t identities -f pkcs12 -P "$P12PW" -o "$TMPFILE"

# upload the encrypted archive as a Document item
op document create "$TMPFILE" \
    --title="Codesigning Cert - <identity-name>" \
    --vault=<vault> --tags="codesign,backup"

# remove the local copy; it's passphrase-encrypted, and 1Password
# now holds the canonical copy. `rm` is honest here, a raw-device
# overwrite (`dd`/shred) buys nothing extra on APFS, which is
# copy-on-write and can leave the original blocks intact anyway
rm "$TMPFILE"
```

Restore on a fresh Mac:

```bash
op document get "Codesigning Cert - <identity-name>" \
    --vault=<vault> --out-file ~/cert.p12
PW=$(op item get "Codesigning Cert Passphrase - <identity-name>" \
    --vault=<vault> --fields password --reveal)
security import ~/cert.p12 -k ~/Library/Keychains/login.keychain-db \
    -T /usr/bin/codesign -T /usr/bin/security -P "$PW"
```

Two items instead of one matters because a 1Password Document item cannot itself hold a generated-and-revealed password field the way a Password item can. Tagging both identically (`codesign,backup`) means a single `op item list --tags=codesign` always surfaces the pair, even years later when the exact title wording is forgotten.

**Caveat**: `security export -t identities` exports every identity currently in the source keychain, not just the one you intend to back up. Verify the keychain holds only the target identity before exporting, or export from a temporary keychain that contains only that identity.

## How to Spot This

Any Apple Development, Apple Distribution, or Developer ID Application cert used for personal automation (LaunchAgents, notarized tools) needs this treatment: the cert is tied to one machine's keychain, and Apple's expiry cadence (yearly for Development, five years for Developer ID) means it will need re-signing and re-backup on a schedule. If the only copy of a signing cert exists on one Mac, that is the moment to run this pattern.

## Related

- [[how-macos-code-signing-actually-works]] - why a real Apple-issued cert (not self-signed) is required in the first place; this note covers keeping that cert recoverable
- [[age-and-1password-complementary-encryption-tiers]] - same key-separation shape (key and ciphertext must fail independently), applied to SSH keys via age instead of a codesigning cert via 1Password's own passphrase-encrypted archive format
