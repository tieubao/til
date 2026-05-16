---
title: How macOS code signing actually works (and why BTM is the strict reader)
date: 2026-05-16
captured: 2026-05-16T07:50:00
tags: [macos, code-signing, security, btm, gatekeeper, launchd]
source: investigation
aliases: ["macos code signing", "code signing on macos", "btm unidentified developer"]
status: refined
---

## Context

A personal LaunchAgent showing "Item from unidentified developer" in BTM (Background Task Manager, System Settings → General → Login Items & Extensions) is cosmetic, not blocking. The agent still runs. But fixing it forces you to understand what code signing actually is, and the dead-end paths people commonly try (ad-hoc, self-signed cert, sudo bypass) all fail for instructive reasons. This note unpacks what the seal really contains, what macOS does with it at runtime, and why BTM is the strict reader that demands a real Apple-issued cert.

## What signing actually does

Codesigning is two things bundled together: a cryptographic seal on the file, plus a claim of identity about who sealed it. macOS uses both at different stages.

### The seal (the math)

`codesign` walks the file, splits it into pages, takes the SHA-256 hash of each page, and packs those hashes plus metadata (file path, version, flags, target architecture) into a structure called the CodeDirectory. The CodeDirectory itself is then hashed and signed with the private key of the signing cert; the result is a CMS signature blob.

Where the blobs live:
- **Mach-O binaries**: inside the file in a load command called `LC_CODE_SIGNATURE`. Survives `cp`, `rsync`, `tar`. Stripped if the binary is rebuilt.
- **Shell scripts and other non-Mach-O files**: in extended attributes `com.apple.cs.CodeDirectory`, `com.apple.cs.CodeRequirements`, `com.apple.cs.CodeSignature`. Survives `cp -p`, `rsync -aX`, `tar` (with `copyfile`). Does NOT survive `git`, `chezmoi apply`, or any editor that does atomic rename on save.

The seal gives integrity (any byte change invalidates the page hash) and authenticity (only someone with the private key could have produced the CMS signature). This is pure cryptography. No Apple involvement is required for the seal itself.

### The identity claim

The CMS blob carries the full cert chain, not just the leaf. For an Apple-issued developer cert, the chain looks like:

```
Apple Development: <Developer Name> (<10-char ID>)
        |
        | signed by
        v
Apple Worldwide Developer Relations Certification Authority
        |
        | signed by
        v
Apple Root CA          <- built into every Mac's trust store
```

The TeamIdentifier (a 10-character string like `XXXXXXXXXX`) is an OID extension Apple adds to the cert when issuing it. It is Apple's claim, not the developer's. A self-signed cert can put any string in any OID; macOS will not treat it as authoritative because the cert does not chain to Apple Root.

## What macOS does at runtime

```
                    trustd / syspolicyd
                    (Gatekeeper)        evaluates the cert chain
                                        on first run + policy events
                            |
            +---------------+----------------+
            |               |                |
            v               v                v
       Hash verify     Trust chain      TeamIdentifier
       (file matches   (chain to        extraction
       its manifest?)  Apple Root?)
            |               |                |
            v               v                v
       Integrity OK    Identity         Used by:
                       established      - sfltool / BTM display
                                        - launchd permission gates
                                        - sandbox attribution
                                        - "Allow apps from..." policy
```

For BTM specifically: `sfltool dumpbtm` reads the cert from the signed binary at agent-registration time, extracts the TeamIdentifier OID and the cert Subject's Organization field, and stores both in its database as `Team Identifier:` and `Developer Name:` for the row. The System Settings UI just reads from that database.

## Why the obvious workarounds fail

| Attempted approach | What ends up stored | What BTM extracts | BTM label |
|---|---|---|---|
| Unsigned | nothing | nothing | "Unknown Developer" |
| Ad-hoc (`codesign --sign -`) | hashes + CodeDirectory, no cert | TeamIdentifier = not set, Developer Name = (null) | "Unknown Developer" |
| Self-signed cert | hashes + CodeDirectory + cert chain rooted in your own self-generated root | TeamIdentifier from cert IF the chain validates | Still "Unknown Developer" because the chain does not reach Apple Root |
| Apple-issued cert (Apple Development, Apple Distribution, Developer ID Application) | hashes + CodeDirectory + Apple-rooted cert chain | TeamIdentifier extracted from the cert OID; Developer Name from Organization | Real label (the developer's org name) |

The cryptographic seal exists in all three signed variants. The identity claim is what differs, and BTM only cares about identity.

## Trust modifications are not bypassable from CLI on Sequoia

A common reflex when seeing the "Unknown Developer" label is to make a self-signed cert and trust it via `security add-trusted-cert`. On macOS Sequoia (15.x) this fails with:

```
SecTrustSettingsSetTrustSettings: The authorization was denied since no user interaction was possible.
```

The Security framework requires the SecurityAgent GUI prompt for any trust-store modification. None of the following bypass it:

- `sudo security add-trusted-cert` (admin domain).
- `security trust-settings-import`.
- `sudo security authorizationdb write system.privilege.admin allow` (the trust API uses a different authorization right, not `system.privilege.admin`).
- Direct write to `~/Library/Keychains/TrustSettings.plist` (Sequoia moved the authoritative trust store away from the legacy plist).

If you cannot run a GUI session on the target machine, you cannot trust a self-signed cert there from CLI. Either get a real Apple-issued cert, or sign on a machine with GUI access and transport the resulting xattrs.

## Transport rules for shell-script signatures

For Mach-O binaries, the signature lives inside the file and survives anything that preserves bytes. For shell scripts, the signature lives in xattrs, and not every transport preserves them:

| Transport | Preserves codesign xattrs? |
|---|---|
| `cp -p` | No |
| Apple's bundled `rsync` (2.6.9) with `--extended-attributes` | Buggy on file-to-file copies |
| Homebrew `rsync` 3.x with `-aX` | Yes |
| `tar` over SSH (macOS bsdtar with `copyfile`) | Yes |
| `git` | No (xattrs are not in the index) |
| `chezmoi apply` | No (treats source files as templates, regenerates target) |
| Editors with atomic-rename-on-save | No (new inode has no xattrs) |

If you sign on machine A and copy to machine B, prefer `tar -czf - <file> | ssh <host> "tar -xzf - -C <dir>"`. Use modern rsync only when you have it installed on both ends.

## BTM cache and re-read

BTM caches the signing identity at agent-registration time, not at runtime. After re-signing an already-registered file, BTM keeps the old "Unknown Developer" label until forced to re-evaluate. The three things that force re-read:

1. Reboot (heavy).
2. `launchctl bootout system/<label>` followed by `launchctl bootstrap system <plist>` (and `touch <plist>` to make sure BTM sees the file as changed).
3. `sfltool resetbtm` (very heavy; re-prompts the user about every login item).

Option 2 is the right one. After bootout/bootstrap, the BTM row picks up the new TeamIdentifier on the next dump.

## What signing does NOT do

- Grant new permissions. The script still needs sudo, TCC consents, or entitlements to actually do work.
- Protect against the signer themselves. The cert holder can re-sign anything.
- Stop a user from overriding Gatekeeper via right-click Open.
- Make a binary "safe". It only makes the binary "attributed".

Signing's value is label hygiene plus tamper detection. If the bytes change without re-signing, macOS refuses to load the binary on next bootstrap. That is genuinely useful as a defense-in-depth measure, but it is not authorization.

## Common gotchas

1. **Apple Development vs Developer ID Application**. Apple Development certs are intended for development (1-year validity, tied to enrolled devices via provisioning profile). Developer ID Application certs are intended for distribution outside the App Store (5-year validity, no device list). Both produce real TeamIdentifier labels in BTM. For personal automation that only runs on registered devices, either works; for binaries you ship to others, you need Developer ID.

2. **The TeamIdentifier reflects the Apple Developer account's org, not the developer's intent**. If you enrolled in the Apple Developer Program under a company name, all your personal automation will show as signed by that company. The fix is a separate personal Developer Program enrollment.

3. **Re-signing on every edit**. Any change to the source bytes (an editor save, a `chezmoi apply`, a `git checkout`) strips the seal. Long-lived setups need an automated re-sign step (a chezmoi `run_after_apply_*.sh` hook, a `make sign` target, a CI step).

4. **Yearly cert renewal**. Apple Development certs expire after 1 year. When the cert expires, existing signatures remain on disk but `codesign --verify` will mark them as invalid. Renew via Xcode → Settings → Accounts → Manage Certificates and re-sign all affected files.

## Related

- [[macos-launchagent-launchdaemon-btm-friendly-plists]] - companion: how to author plists so BTM shows the right name + icon
- [[1password-backup-pattern-for-apple-dev-certs]] - companion: backing up the .p12 + passphrase to 1P for cross-machine recovery
