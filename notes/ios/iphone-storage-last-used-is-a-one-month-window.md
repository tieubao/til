---
title: "iPhone Storage \"Last Used\" is a one-month window and the only usage signal iOS exposes"
date: 2026-09-05
captured: 2026-09-05T09:40:00Z
tags: [ios, storage, ocr]
source: "Claude Code session"
aliases: [iphone storage last used, ios app last opened date, installation_proxy no usage data, which iphone apps are unused]
status: refined
---

**iOS exposes app usage in exactly one place, Settings > General > iPhone Storage, and only as a "Last Used" subtitle for apps opened within roughly the last month. Nothing in the device APIs carries it.**

Verified on iOS 26 with a full `ideviceinstaller list --xml` dump: the installation-proxy metadata has bundle ids, versions, sizes and dates for install and update, and no field for last launch. The Storage screen is rendered from usage data the phone keeps but does not export.

## Reading the absence

| Storage row shows | Meaning |
|---|---|
| `Last Used: <date>` | opened within about the last month (oldest value observed: 26 days back) |
| no subtitle | not opened in over a month |
| `Never Used` | not seen on iOS 26 at all in a list of 260 apps |

So the cull signal is the missing subtitle, not a date: every row without one is a candidate.

## Getting it at scale

The screen is a scrolling list with no export, so capture it as video and read it back:

1. Screen-record a slow scroll through iPhone Storage (260 apps took a few minutes).
2. Extract frames: `ffmpeg -i rec.mp4 -vf fps=2 frames/%04d.png`.
3. OCR the frames (Apple's Vision framework works from a Swift script, about one second per frame).
4. Join the OCR'd app names against `ideviceinstaller list --user` to get bundle ids, and treat "name present, no Last Used line" as unused.

The join is fuzzy (OCR mangles some names) but the false-negative direction is safe: an app you cannot match stays installed.
