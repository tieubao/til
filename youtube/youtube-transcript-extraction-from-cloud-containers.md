---
title: "YouTube transcript extraction from cloud containers"
date: 2026-03-28
captured: 2026-03-28T02:37:28.383Z
tags: ["youtube", "nodejs", "proxy", "undici", "transcript", "fallback-chain"]
source: "Claude.ai chat"
---
## Problem

YouTube aggressively blocks cloud/datacenter IPs from accessing transcripts. Every standard extraction method fails from cloud environments like Claude.ai's container, AWS, GCP, etc. This includes `youtube-transcript-api` (Python), `yt-dlp`, YouTube's Innertube API, and Jina Reader.

The core challenge: how to reliably extract YouTube transcripts from a cloud container that YouTube actively blocks.

## The breakthrough: Node.js fetch vs container proxy

Node.js native `fetch()` does NOT respect the container's `HTTPS_PROXY` environment variable. This is the root cause of extraction failures -- even though `curl` can reach YouTube fine (it uses the proxy automatically), any Node.js or Python library using native HTTP clients will fail with DNS errors or IP blocks.

The fix: inject a proxy-aware `fetch` function via undici's `ProxyAgent`:

```javascript
import { ProxyAgent, fetch as undiciFetch } from 'undici';
import { YoutubeTranscript } from 'youtube-transcript';

const dispatcher = new ProxyAgent({
  uri: process.env.HTTPS_PROXY,
  requestTls: { rejectUnauthorized: false }
});

const proxyFetch = (url, opts = {}) => undiciFetch(url, { ...opts, dispatcher });

// The youtube-transcript package accepts a custom fetch function
const segments = await YoutubeTranscript.fetchTranscript(videoId, { fetch: proxyFetch });
```

This works because `youtube-transcript` npm package accepts a `fetch` option, and undici's `ProxyAgent` routes requests through the container's egress proxy -- the same path `curl` uses.

**Python's `youtube-transcript-api` does NOT work** even with explicit proxy configuration via `GenericProxyConfig` or `http_client` session. The library's internal HTTP path gets blocked differently from Node's.

## 4-layer fallback chain

![YouTube transcript extraction fallback chain](https://assets.han-ws.workers.dev/i/2026/03/youtube-capture-fallback-chain.png)

### Layer 1: youtube-transcript npm + undici ProxyAgent (HIGH reliability)

Node.js package with proxy-aware fetch. Returns structured JSON with timestamps per segment. Works for any public video with captions.

Setup:
```bash
cd /home/claude && npm install youtube-transcript undici
```

### Layer 2: curl watch page + captionTracks parse (MEDIUM reliability)

Fetch YouTube HTML via `curl` (which uses container proxy natively), parse `captionTracks` from `ytInitialPlayerResponse` JSON, fetch timedtext XML. Same IP as Layer 1 but different code path.

### Layer 3: web_search + web_fetch on transcript sites (MEDIUM reliability)

Search for `"VIDEO_TITLE transcript"` via Anthropic's search infrastructure. Different IP pool from the container, so works even when YouTube blocks the container entirely. Known working sites: podscripts.co, recapio.com, creator-owned sites.

### Layer 4: User pastes transcript from YouTube UI (ALWAYS works)

Manual fallback. Click "..." > "Show transcript" in YouTube, copy-paste into chat.

## Key insight: independent failure domains

Layers 1-2 share the container's egress proxy IP. They rate-limit together. Layer 3 goes through Anthropic's own infrastructure -- completely different IP pool. This is what makes the chain actually reliable rather than just "try the same thing three ways."

## YouTube rate-limiting behavior

- YouTube rate-limits per **video+IP combination**, not globally
- After 3-5 requests to the same video in quick succession, that specific video gets blocked
- Other videos on the same IP may still work
- The block clears after 1-10 minutes
- The container's egress proxy IP is shared across sessions, so other users' YouTube requests can consume quota

## What does NOT work from cloud IPs (tested March 2026)

| Method | Failure mode |
|---|---|
| `youtube-transcript-api` (Python) | `IpBlocked` error, even with proxy config |
| `yt-dlp` | 429 + bot detection + requires JS runtime |
| YouTube Innertube API (direct POST) | "Sign in to confirm you're not a bot" |
| Jina Reader on YouTube URLs | YouTube returns 429 to Jina's servers |
| YTScribe (`ytscribe.com`) | Paywalled as of early 2026 |
| `youtubetranscript.com` | JS-rendered, curl returns empty shell |
| Node.js native `fetch` to YouTube | DNS fails (ignores `HTTPS_PROXY`) |

## What DOES work

| Method | Notes |
|---|---|
| `youtube-transcript` npm + undici ProxyAgent | Full structured transcript with timestamps |
| `noembed.com/embed?url=` | Video metadata (title, channel, thumbnail) |
| `web_search` + `web_fetch` on transcript sites | When video is indexed by third parties |
| YouTube thumbnail URLs (`i.ytimg.com`) | Public, never blocked |
| `curl` to YouTube (container proxy) | Works but returns HTML that needs parsing |