---
title: "million WebSockets and Go"
date: 2019-12-23
captured: 2019-12-23T23:11:43Z
tags: [golang, websocket, performance, optimization]
source: "GitHub issue tieubao/til#470 + https://gbws.io/articles/million-websocket-and-go/"
aliases: []
status: refined
---

## Context

Mail.Ru needed to handle 3 million concurrent WebSocket connections for real-time mail notifications. Their previous approach polled via HTTP at 50,000 requests per second, with 60% returning no changes. This article by Sergey Kamardin documents the optimization journey from a naive Go implementation to a production-ready system.

**Source:** [A Million WebSockets and Go](https://www.freecodecamp.org/news/million-websockets-and-go-cc58418460bb/)

## The problem: memory at scale

The idiomatic Go approach (goroutine per connection) required enormous memory:

| Component | Memory Cost |
|-----------|------------|
| Goroutine stacks (2 per conn) | 24 GB |
| Reader buffers (4 KB each) | 12 GB |
| Writer buffers (4 KB each) | 12 GB |
| HTTP handler buffers | 24 GB |
| **Total** | **72 GB** |

All of this overhead existed before handling any application logic.

## Four critical optimizations

**1. Netpoll instead of blocking goroutines.** Replace continuous read goroutines with event-driven monitoring using epoll/kqueue. Connection lifetimes range from seconds to days, making persistent goroutines wasteful. Savings: 24 GB.

**2. On-demand writer goroutines.** Start write goroutines only when packets arrive, reuse buffers via `sync.Pool`. Savings: 24 GB.

**3. Goroutine pool with resource limits.** A bounded worker pool (e.g., 128 workers) prevents self-DDoS during overload, controls simultaneous request handling, and reuses goroutine stacks.

**4. Zero-copy upgrade protocol.** Abandon `net/http` for WebSocket upgrades. Direct TCP connection to `ws.Upgrade()` eliminates HTTP parsing overhead entirely. Standard HTTP upgrade: 8,576 bytes allocated, 9 allocations. Zero-copy TCP: 0 bytes, 0 allocations. Savings: 24 GB.

## Result

From 72 GB down to 24 GB, a 66% reduction. The key libraries used:

- `github.com/mailru/easygo` for netpoll implementation
- `github.com/gobwas/ws` for low-level WebSocket protocol with streaming I/O

## Takeaways

- Profile before optimizing. Understand where goroutines are blocking unnecessarily.
- Leverage OS primitives (epoll/kqueue) via netpoll instead of goroutine-per-connection.
- Reuse memory with `sync.Pool` and limit concurrency via worker pools.
- Choose libraries that expose `io.Reader`/`io.Writer` for buffer control.

## Related
- [[go-concurrency-through-illustrations]] - foundational goroutine and channel concepts used here
- [[building-worker-pool-in-go]] - the worker pool pattern is one of the key optimizations
- [[channels-in-golang]] - channel mechanics underlying the architecture
