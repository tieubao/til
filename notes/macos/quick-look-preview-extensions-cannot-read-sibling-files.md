---
title: "A Quick Look preview extension can read the previewed file and nothing beside it"
date: 2026-09-09
captured: 2026-09-09T01:30:00Z
tags: [macos, quick-look, app-sandbox, csp, appex]
aliases: [QLPreviewProvider sandbox, deny file-read-data quicklook, quick look broken image, QLPreviewReply data URI]
source: "Claude Code session"
status: refined
---

**Quick Look hands a preview extension a sandbox extension for the previewed file alone. A sibling file in the same directory is denied, so a markdown preview that renders `![](fig.png)` draws a broken image and no error.** The denial is only visible in the kernel log:

```
Sandbox: MarkdownPreviewQL(75079) deny(1) file-read-data /Users/me/notes/fig.png
```

Nothing surfaces in the extension itself. `Data(contentsOf:)` throws a generic read failure, the HTML keeps its unresolved `src`, and the panel renders the placeholder every browser draws for an image it could not load.

Two fixes exist and they are different in kind. Inline the bytes as a `data:` URI, which needs the read to succeed first, or grant the read. The grant is a temporary-exception entitlement, read-only and scoped to the home directory:

```xml
<key>com.apple.security.temporary-exception.files.home-relative-path.read-only</key>
<array><string>/</string></array>
```

## The second half of the trap

A data-based preview (`QLPreviewReply(dataOfContentType: .html, ...)`) is handed raw HTML with no base URL, so a relative `src` has nothing to resolve against even when the read is allowed. Inlining as `data:` is the way out. But a hardening CSP of `default-src 'none'` blocks `data:` images too, and the page then fails exactly like the sandbox denial did: a broken placeholder, no error, no log line.

Stacked, the two produce a convincing false conclusion. An earlier probe here tested three channels (a relative path, an absolute path, and a `data:` URI as the control) and every one drew a placeholder, which read as "the panel does not render images at all". The control was itself blocked by the CSP that the same session had just added, and the sandbox test that cleared the sandbox ran in a plain binary rather than inside the appex. Both controls tested something other than the shipping path.

The working combination is all three: the entitlement, the inlining, and `img-src data:` in the policy.

```
default-src 'none'; style-src 'unsafe-inline'; img-src data:
```

A remote `src` stays blocked under that policy, which is worth keeping: previewing an untrusted document should never reach the network.

## What to check first

When a sandboxed extension fails silently, read the kernel's sandbox log before forming any theory:

```
log show --last 15m --info --predicate 'eventMessage CONTAINS "deny"'
```

One line there named a cause that three rebuilds had missed. The general form: a control that does not run inside the real sandbox, under the real policy, proves nothing about the real failure.
