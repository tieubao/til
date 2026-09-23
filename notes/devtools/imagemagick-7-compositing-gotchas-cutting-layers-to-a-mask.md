---
title: "imagemagick 7 compositing gotchas: cutting layers to a mask"
date: 2026-09-23
captured: 2026-09-23T00:00:00Z
tags: [imagemagick, cli, image-processing, compositing, devtools]
source: "Claude Code session"
aliases: ["magick flatten drops color after CopyOpacity", "imagemagick overlay composite wrong operand order", "magick rotate then crop off-center", "awk txt: parsing y coordinate bug", "cut a color to a mask with CopyOpacity"]
status: refined
---

Batch-generating colored logo remixes from one white-on-black mask in ImageMagick 7 hit five distinct compositing bugs, each verified against a tiny synthetic mask and checked with `-format '%[fx:mean]'` or a pixel readout. Four gotchas plus one non-repro, recorded below.

## `-flatten` inherits whatever `-compose` was last set to

Symptom: a layer correctly cut to the mask with `-compose CopyOpacity`, then flattened onto a canvas, comes out near-blank instead of colored.

Cause: `-flatten` (also `-layers flatten` and `-mosaic`) composites using the CURRENT `-compose` setting, not `Over`. If the compose mode is still `CopyOpacity` from an earlier step, flatten copies the top layer's alpha onto the canvas and keeps the canvas's own color, discarding the color layer entirely.

```bash
# broken: -compose CopyOpacity leaks out of the parens into -flatten
magick canvas.png \( color.png mask.png -alpha off -compose CopyOpacity -composite \) \
  -flatten broken.png
# center pixel: graya(255,1)  <- near-transparent, no red

# fixed: reset compose to Over before flatten
magick canvas.png \( color.png mask.png -alpha off -compose CopyOpacity -composite \) \
  -compose Over -flatten fixed.png
# center pixel: srgb(255,0,0)  <- correct
```

## `A B -compose Overlay -composite` puts B onto A, and Overlay drops alpha

Symptom: overlaying a full-canvas opaque grayscale texture onto a mostly transparent color layer returns the texture everywhere, not just inside the masked shape.

Cause: the composite operand order puts the second image onto the first, and `Overlay` (like most blend-mode composes) produces an opaque result, wiping the destination's alpha channel. A layer that was transparent outside the mask becomes fully opaque texture outside the mask too.

```bash
# broken: texture overwrites the whole canvas, alpha channel gone
magick cut.png texture.png -compose Overlay -composite broken.png
# corner pixel: srgb(127,127,127), alpha_mean=0 (no alpha channel at all)

# fixed: re-apply the mask after the overlay
magick broken.png mask.png -alpha off -compose CopyOpacity -composite fixed.png
# corner pixel: srgba(0,0,0,0), alpha_mean restored to the mask's own coverage
```

## `-rotate` leaves a virtual canvas offset that a later `-gravity`/`-crop` reads against

Symptom: `-rotate` then `-gravity center -crop WxH+0+0` crops off-center, sometimes cropping the subject out entirely.

Cause: rotating a non-square-multiple angle expands the canvas and records a page offset (e.g. `276x276+38+38`) instead of resetting it. A following `-gravity center -crop` computes the center against that stale virtual canvas, not the visible content, so the crop window lands in the wrong place.

```bash
# broken: crop misses the content
magick layer.png -background none -rotate 30 -gravity center -crop 200x200+0+0 broken.png
# alpha_mean=0.0022  <- subject almost entirely cropped away

# fixed: +repage right after -rotate resets the page offset
magick layer.png -background none -rotate 30 +repage -gravity center -crop 200x200+0+0 fixed.png
# alpha_mean=0.0395  <- subject retained and centered
```

## Parsing `txt:` output with `awk -F'[(,)]'` needs `+0` on the y field

Symptom: a script that indexes pixels by row (`row[y] += ...`) using the y coordinate parsed from `magick img.png txt:-` comes back empty for every numeric lookup.

Cause: a `txt:` line looks like `0,0: (255,0,0,0)  #FF000000  srgba(...)`. Splitting on `[(,)]` leaves the y field as `"0: "`, a string carrying the trailing colon and space, not a clean number. Used as an array key without coercion, it never matches a numeric lookup like `row[100]`.

```bash
# broken: y is the string "100: ", not the number 100
magick img.png txt:- | awk -F'[(,)]' 'NR>1{y=$2; row[y]+=1} END{print row[100]+0}'
# => 0

# fixed: coerce to numeric with +0
magick img.png txt:- | awk -F'[(,)]' 'NR>1{y=$2+0; row[y]+=1} END{print row[100]+0}'
# => 51000
```

## Non-repro: blur before threshold does not reliably erase noise

Claimed gotcha: `-blur` then `-threshold 62%` on uniform random noise erases everything, because blurred noise clusters near 50% mean.

What actually reproduced: the clustering claim holds (`+noise Random` blurred with `-blur 0x3` measured mean 0.502, stdev 0.028, versus stdev 0.289 unblurred), and a threshold far from that mean does erase the image (mean 0.0005 after `-threshold 62%`). But this is just "a tight distribution collapses under a threshold far outside its range," true of any blur radius and any noise seed, not a fixed 62%-specific bug. For sparse distress speckle, threshold close to the blurred mean (near 50%) or skip the blur and threshold the raw noise instead, which keeps real speckle density (measured mean 0.385, stdev 0.487 unblurred vs. mean 0.0005 blurred-then-thresholded-far-off).

## Bonus: the base pattern for cutting a solid color to a mask

```bash
magick -size WxH xc:COLOR color.png
magick color.png mask.png -alpha off -compose CopyOpacity -composite out.png
```

`mask.png` supplies alpha from its grayscale value (white opaque, black transparent); `-alpha off` on the mask stops its own alpha channel, if any, from interfering with the copy.

## Key takeaway

`-compose` is session state, not an argument scoped to the next `-composite`. It survives across parentheses and into `-flatten`, and composite operand order plus blend-mode choice both change whether the result keeps an alpha channel at all. Reset `-compose Over` before any flatten, re-mask after a full-canvas blend, and `+repage` immediately after any operation that resizes the canvas.

## Related

- [[an-escaped-delimiter-is-still-the-delimiter-byte]] - another case where a hand-rolled positional parse over CLI output silently returns the wrong field
