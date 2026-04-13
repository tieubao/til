---
title: "why Vim uses hjkl for navigation"
date: 2021-03-21
captured: "2021-03-21T17:25:21Z"
tags: [vim, history, ascii, hardware]
source: "GitHub issue tieubao/til#538"
aliases: []
status: refined
---

## Context

The history behind Vim's hjkl navigation keys goes deeper than "it keeps your fingers on the home row." It traces back through vi, the ADM-3A terminal, and ultimately the 1967 ASCII table.

**Source:** [There is always more history](https://www.hillelwayne.com/post/always-more-history/)

## Layer 1: Bill Joy and the ADM-3A

Bill Joy developed vi on the ADM-3A terminal, which did not have dedicated arrow keys. The ADM keyboard had arrow symbols printed on the h, j, k, l keys. Joy used the same mapping for vi, which became Vim.

## Layer 2: why the ADM used hjkl

The deeper question is why the ADM-3A put arrows on those particular keys. The answer lies in ASCII and control characters.

In the 1967 ASCII table, each character has 7 bits. The first 32 characters are control characters used for communication. Keyboards needed a way to input these while keeping the QWERTY layout, so they added a "control" key that zeroed the 6th and 7th highest bits of the pressed key.

- **Ctrl+H** (^H) = backspace (100 1000 becomes 000 1000)
- **Ctrl+J** (^J) = line feed (move down)

The ADM manual defined "backspace" as "move cursor left" without deleting the character. With ^H and ^J already being left and down, it was natural to assign ^K as up and ^L as right.

## The chain of causation

1967 ASCII table defined control characters -> ADM-3A mapped ^H/^J/^K/^L to cursor movement -> keyboard printed arrows on hjkl -> Bill Joy followed the convention for vi -> Vim inherited it.

## The takeaway

What appears to be an ergonomic design decision is actually a chain of historical accidents going back to the ASCII standard. "Home row efficiency" is a retroactive justification, not the original reason.

## Related
