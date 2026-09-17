---
title: "a scripted click carries no user activation"
date: 2026-09-17
captured: 2026-09-17T00:00:00Z
tags: [browser-automation, cdp, debugging]
source: "Claude Code session"
aliases: ["element.click popup blocked", "CDP window.open blocked", "Input.dispatchMouseEvent user activation"]
status: refined
---

`element.click()` issued from `Runtime.evaluate` over the DevTools Protocol dispatches a real click event, so most handlers run and the automation looks correct. What it does not carry is transient user activation. Any part of the handler gated on activation fails: `window.open`, entering fullscreen, reading the clipboard, playing audio.

The most confusing case is a form submit that opens a second window. The submit fires, the handler runs, the popup is blocked, and the page reports whatever its own error path says. That message is usually about the destination, not about the click, so the trail leads away from the automation entirely.

The fix is to synthesize the click at the input layer instead of the DOM layer:

```
Runtime.evaluate  -> el.click()               -> event fires, no activation -> popup blocked
Input.dispatchMouseEvent (at the coordinates) -> event fires, activation    -> popup opens
```

Resolve the element's box first (`DOM.getBoxModel`, or `getBoundingClientRect` via evaluate), scroll it into view, then dispatch `mousePressed` and `mouseReleased` at its centre with `button: "left"` and `clickCount: 1`.

This is the same rule that bites on menus and overlays that only open on a trusted event. The tell is always the same shape: the handler clearly ran, and exactly the activation-gated part of it did nothing.

## Key takeaway

A DOM-level click is an event, not an interaction. When anything in the handler needs user activation, dispatch at the input layer with coordinates.
