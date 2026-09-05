---
title: "Apple Wallet passes need a Pass Type ID certificate and do not exist on macOS"
date: 2026-09-05
captured: 2026-09-05T09:40:00Z
tags: [ios, apple-wallet, pkpass]
source: "Claude Code session"
aliases: [pkpass will not add, wallet pass signing, pass type id certificate, no wallet app on mac, self-issued wallet pass]
status: refined
---

**A `.pkpass` installs on an iPhone only when it is signed by an Apple Pass Type ID certificate issued to a paid developer account, and macOS has no Wallet app at all, so a pass can be neither added nor tested on a Mac.**

An airline or venue pass "just works" because the issuer signed it; the signature (a PKCS#7 `signature` file inside the zip, over `manifest.json`) is what Wallet validates, and an unsigned or self-signed bundle is rejected with no useful error. The same applies to passes opened from Mail, Safari, AirDrop or Files.

## What self-issuing costs

| Requirement | Detail |
|---|---|
| Apple Developer Program | paid membership |
| Pass Type ID | created in the developer portal, one per pass kind |
| Pass Type ID certificate | issued against a CSR, used to sign every pass |
| WWDR intermediate | included in the signature chain |
| Signing step | `openssl smime -sign` or PassKit tooling over `manifest.json` |

Even then a self-issued pass is a reference card: it shows text, a barcode image and colors, but nothing scans it into any system unless a reader on the other side expects that barcode. A "membership pass" for a household or a small team is a picture in a wallet, not a credential.

## Alternatives that skip the certificate

- A plain image or PDF in Files or Photos, for something only a human reads.
- A URL or QR code that opens a web page, for something a phone scans.
- A third-party pass service (they sign with their certificate), when the pass must live in Wallet and the volume justifies the fee.
