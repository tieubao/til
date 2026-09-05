---
title: "Apple Vision OCR as an interpreted Swift CLI, no build step"
date: 2026-09-05
captured: 2026-09-05T09:40:00Z
tags: [macos, swift, ocr, vision]
source: "Claude Code session"
aliases: [swift script ocr, VNRecognizeTextRequest command line, run swift file without compiling, vietnamese ocr macos]
status: refined
---

**A `.swift` file that imports Vision runs directly with `swift file.swift args...`, so macOS ships an accurate multilingual OCR command line with nothing to install: no Xcode project, no compile, no Homebrew.**

```swift
import Vision
import AppKit

let args = Array(CommandLine.arguments.dropFirst())
for path in args {
    guard let img = NSImage(contentsOfFile: path),
          let cg = img.cgImage(forProposedRect: nil, context: nil, hints: nil) else { continue }
    let request = VNRecognizeTextRequest { req, _ in
        for obs in req.results as? [VNRecognizedTextObservation] ?? [] {
            if let top = obs.topCandidates(1).first { print("\(path)\t\(top.string)") }
        }
    }
    request.recognitionLevel = .accurate
    request.recognitionLanguages = ["en-US", "vi-VN"]
    try? VNImageRequestHandler(cgImage: cg, options: [:]).perform([request])
}
```

```bash
swift ocr-frames.swift frames/*.png > ocr.txt
```

About one second per 2x-scaled phone screenshot at `.accurate`; `.fast` is several times quicker and good enough for large UI text. `recognitionLanguages` takes BCP-47 tags and the Vietnamese model handles diacritics correctly, which Tesseract's default traineddata does not.

## Why interpreted

The script is the tool: edit, rerun, no toolchain state. It fits pipelines such as `ffmpeg -vf fps=2` frame extraction followed by a text parser, where the OCR step is one line in a shell script. The first run is slow (the Swift interpreter type-checks the file) and the frameworks load per process, so batch many images per invocation instead of calling it once per file.

Caveats: `swift file.swift` needs the Xcode command line tools; Vision is macOS 10.15+ and its language list is per OS version (`VNRecognizeTextRequest.supportedRecognitionLanguages()` prints it).
