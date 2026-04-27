---
title: "macOS Input Method Kit (IMK) architecture and lifecycle"
date: 2026-04-27
captured: 2026-04-27T06:39:47.143Z
tags: ["macos", "input-methods", "architecture"]
source: "Claude.ai chat"
---
## Definition

**Input Method Kit (IMK)** is Apple's Objective-C framework (`/System/Library/Frameworks/InputMethodKit.framework`, introduced in macOS 10.5 Leopard, 2007) for building **input methods** on macOS. Examples: Pinyin, Japanese kana/kanji conversion, Vietnamese Telex/VNI, Korean Hangul, custom IMEs (emoji pickers, Vim-style modal input, dead-key transliterators). It is the supported successor to the old Carbon Text Services Manager (TSM) component APIs.

The single most important architectural fact: **an input method on macOS is a separate `.app` bundle that runs as its own process**, not a library linked into the host app. When the user selects an input source in the menu bar, macOS launches that input method process and routes keystrokes to it before they reach the focused app. This out-of-process model is the source of most of IMK's quirks.

## Architecture

![macOS input stack with IMK](https://assets.han-ws.workers.dev/i/2026/04/imk-macos-input-stack.svg)

The stack from hardware to client app:

1. **Hardware → IOKit HID → WindowServer** delivers raw key events with scancodes and modifier state.
2. **HIToolbox + Text Services Manager (TSM)** identifies the active input source. If it's an input method, TSM routes the event over Mach IPC to the IMK process instead of delivering it directly to the focused app.
3. **IMK process** (separate `.app`, sandboxed) hosts an `IMKServer` (vends a Mach port and connection) plus your `IMKInputController` subclass (handles keystrokes, manages composition state, drives the candidate window).
4. **Focused app process** receives composed text via its `NSTextInputClient` implementation (typically inside an `NSTextView` or `WKWebView`).

The two processes communicate over Mach IPC. The `client` argument passed into your input controller is a remote proxy conforming to the `IMKTextInput` protocol — every method call on it crosses the process boundary.

## Keystroke lifecycle

You press <kbd>n</kbd> <kbd>i</kbd> <kbd>h</kbd> <kbd>a</kbd> <kbd>o</kbd> with Pinyin selected, expecting 你好.

1. WindowServer receives the key event from IOKit, identifies the focused window's app, but before delivering it, checks the active input source.
2. The active source is an IMK input method, so HIToolbox/TSM routes the event over Mach IPC to the IMK process. The client app **never sees the raw keystrokes** while composition is active. This is why password fields with Secure Input enabled cause input methods to break.
3. Inside the IMK process, `IMKServer` dispatches to the `IMKInputController` subclass. The relevant entry point is usually `inputText:client:` (or `handleEvent:client:` for full event objects). The `client` argument is a proxy object conforming to the `IMKTextInput` protocol, the handle to the remote text view.
4. The controller maintains composition state ("nih" so far). It calls `[client setMarkedText:@"nǐh" selectionRange:... replacementRange:...]` to show the underlined preedit in the app. The app's `NSTextInputClient` receives this over IPC and renders it.
5. When the user picks 你好 from the candidate window (which IMK provides via `IMKCandidates`), the controller calls `[client insertText:@"你好" replacementRange:...]`. The marked text is replaced with committed text. Composition state resets.

The candidate window is itself an `NSPanel` rendered by the input method process, floating above the client app's window. Coordinate conversion (where to place it relative to the cursor) goes through `[client attributesForCharacterIndex:...]`.

## Minimal IMKInputController

```objc
@interface MyController : IMKInputController
@property (nonatomic, strong) NSMutableString *buffer;
@end

@implementation MyController

- (instancetype)initWithServer:(IMKServer *)server delegate:(id)delegate client:(id)inputClient {
    self = [super initWithServer:server delegate:delegate client:inputClient];
    if (self) { _buffer = [NSMutableString string]; }
    return self;
}

// Called for each keystroke while this IM is active
- (BOOL)inputText:(NSString *)string client:(id)sender {
    if ([string isEqualToString:@" "]) {
        // Space commits whatever is in the buffer (toy example)
        [sender insertText:[self transliterate:self.buffer]
          replacementRange:NSMakeRange(NSNotFound, NSNotFound)];
        [self.buffer setString:@""];
        return YES;
    }
    [self.buffer appendString:string];
    // Show preedit underlined
    [sender setMarkedText:self.buffer
            selectionRange:NSMakeRange(self.buffer.length, 0)
          replacementRange:NSMakeRange(NSNotFound, NSNotFound)];
    return YES;  // event consumed
}

- (void)commitComposition:(id)sender {
    if (self.buffer.length) {
        [sender insertText:self.buffer
          replacementRange:NSMakeRange(NSNotFound, NSNotFound)];
        [self.buffer setString:@""];
    }
}

@end
```

The bundle's `Info.plist` declares:
- `InputMethodConnectionName` — the Mach port name
- `InputMethodServerControllerClass` — your controller class
- `tsInputMethodCharacterRepertoireKey` — which scripts you handle
- `TICapsLockLanguageOverrideCapable` — opt-in to Caps Lock language switching

The `.app` lives in `~/Library/Input Methods/` (per-user) or `/Library/Input Methods/` (system). After a relaunch of `cfprefsd` and re-login (or `killall TextInputMenuAgent`), it appears in System Settings → Keyboard → Input Sources.

## IMK vs alternatives

| You want to... | Use | Why not IMK |
|---|---|---|
| Build a Pinyin/Telex/Hangul/Japanese IME | **IMK** | This is exactly what it's for |
| Add custom autocomplete inside *your* app | **NSTextInputClient** on your text view | IMK is system-wide; you don't need that scope |
| Remap keys globally (Caps Lock → Esc) | **CGEventTap** or Karabiner | IMK is text-composition, not key remapping |
| Intercept text before it reaches any app | **CGEventTap** (with accessibility) | IMK only fires when *your IM is selected* |
| Build text expansion/snippets | CGEventTap or accessibility APIs | IMK requires the user to switch input source |
| Do speech-to-text | Speech framework / dictation APIs | Different subsystem entirely |

## Pain points

- **Secure Input event blocks IMK entirely.** When an app enables `EnableSecureEventInput` (password fields, sudo prompts, 1Password's main vault), the OS bypasses input methods. There is no fix; it is a security feature. Users of non-Latin input methods experience this as "my input doesn't work in Terminal sometimes."
- **Sandboxing is awkward.** Input methods run out-of-process with limited entitlements. Network access, file access outside the container, and XPC to helpers all need explicit entitlements. Apple has historically been picky about IMs in the Mac App Store.
- **Hot reload is painful.** Changes to the `.app` often require killing the input method process and sometimes logging out. `killall <YourIMName>` during development; `pkill -f /Library/Input\ Methods/` for the nuclear option.
- **The `client` proxy is unreliable across some apps.** Electron apps, older Java apps, and anything using non-standard text views have inconsistent `NSTextInputClient` implementations. `attributesForCharacterIndex:` returning nil for cursor position is the most common bug — the candidate window cannot be placed correctly.
- **Debugging is annoying.** The input method process cannot be attached to in Xcode the normal way before launch. Common workarounds: `NSLog` to a file and `tail -f` it, or attach to the running process after triggering it. Console.app filtered to the bundle ID helps.
- **Performance matters more than expected.** Every keystroke crosses an IPC boundary. If `inputText:client:` does anything slow (synchronous network, heavy computation), typing latency becomes visible. Production IMEs offload candidate generation to background queues and stream results.
- **The framework hasn't really changed since ~2008.** Stable Objective-C with C-flavored APIs. Swift bridging works but documentation is thin. Most production input methods (Squirrel, OpenVanilla, Sogou, Google Japanese IME) remain ObjC++.

## Reference implementations worth reading

- **Squirrel** — RIME engine on macOS, Cantonese/Mandarin. Small enough to read end-to-end.
- **OpenVanilla** — Multi-language IM platform. Older but well-structured.

If the task isn't a full IME for a writing system, IMK is almost certainly the wrong choice. Reach for `CGEventTap` or `NSTextInputClient` instead.