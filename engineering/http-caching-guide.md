---
title: "quick guide to HTTP caching"
date: 2017-04-17
captured: 2017-04-17T10:55:45Z
tags: [http, caching, web-performance]
source: "GitHub issue tieubao/til#297 + http://kamranahmed.info/blog/2017/03/14/quick-guide-to-http-caching/"
aliases: [web-cache-everything-you-need-to-know]
status: refined
---

## Context

Kamran Ahmed provides a practical guide to HTTP caching, covering the headers, strategies, and decision points for implementing caching in web applications. Understanding HTTP caching is foundational for web performance optimization.

**Source:** [Quick Guide to HTTP Caching](http://kamranahmed.info/blog/2017/03/14/quick-guide-to-http-caching/)

**Attachment:** [Web Cache - Everything you need to know.pdf](https://github.com/tieubao/til/files/925402/Web.Cache.-.Everything.you.need.to.know.pdf)

## Cache locations

- **Browser cache** - private to a single user, stored on their device
- **Proxy cache** - shared cache sitting between client and server (CDN, corporate proxy)
- **Gateway cache / reverse proxy** - sits in front of the server, caches responses for all users

## Key HTTP headers

**Cache-Control** - the primary mechanism for caching directives:

- `public` - response can be cached by any cache (browser, CDN, proxy)
- `private` - response is for a single user only (browser cache, not CDN)
- `no-cache` - must revalidate with the server before using cached copy (does NOT mean "don't cache")
- `no-store` - do not cache at all. The only directive that truly prevents caching.
- `max-age=N` - cache is valid for N seconds from the time of the request

**ETag** - a fingerprint of the resource content. The server sends an ETag with the response. On subsequent requests, the browser sends `If-None-Match` with the ETag. If the content has not changed, the server returns 304 Not Modified (no body).

**Last-Modified / If-Modified-Since** - date-based revalidation. Less precise than ETags but simpler.

**Expires** - an older header specifying an absolute expiry date. Superseded by `Cache-Control: max-age` but still used for backward compatibility.

## Caching strategy decision tree

1. Is the response cacheable at all? If it contains sensitive data, use `no-store`.
2. Should it be revalidated on every request? Use `no-cache`.
3. Can intermediaries cache it? Use `public`. Otherwise `private`.
4. What is the maximum freshness lifetime? Set `max-age`.
5. Does the content need fingerprinting? Add `ETag`.

## Common patterns

- **Immutable assets** (hashed filenames like `app.a1b2c3.js`): `Cache-Control: public, max-age=31536000` (1 year)
- **HTML pages**: `Cache-Control: no-cache` (always revalidate, but cache the response)
- **API responses**: `Cache-Control: private, max-age=0` or `no-store` depending on sensitivity
- **Shared static assets** (images, fonts): `Cache-Control: public, max-age=86400` with ETag

## Key takeaway

The most common mistake is confusing `no-cache` with `no-store`. `no-cache` still caches the response but forces revalidation. `no-store` is the nuclear option. Getting this distinction right prevents both stale content bugs and unnecessary server load.

## Related
