---
title: "Go performance optimization guide"
date: 2016-02-25
captured: 2016-02-25T09:20:18Z
tags: [golang]
source: "GitHub issue tieubao/til#172"
aliases: []
status: refined
---

## Context

Tyler Treat's deep dive into Go performance optimization, originally proposed as a GopherCon talk. Written from experience at Workiva building cloud-based financial reporting systems (graph traversal, distributed calculation engines, low-latency messaging) where performance at scale became critical after migrating off App Engine.

**Source:** [So You Wanna Go Fast?](http://bravenewgeek.com/so-you-wanna-go-fast/)

## Two kinds of fast

Software has two speeds that are often at odds:

- **Delivery fast** - getting to market, shipping features, iterating quickly
- **Performance fast** - low latency, high throughput, efficient resource usage

Customers want the first. Developers want the second. CTOs want both. The author's App Engine-to-microservices migration illustrates the tension: App Engine made scaling easy by restricting what you could do, but leaving it meant trading delivery speed for performance control.

## Channels vs lock-free ring buffers

Channels are convenient but use locking under the hood. In multithreaded scenarios (GOMAXPROCS > 1), a lock-free ring buffer using CAS operations significantly outperforms channels:

- Single producer/consumer: ring buffer ~3x faster (182 vs 542 ns/op)
- Multiple producer/consumer with contention: ring buffer ~1.7x faster (182,557 vs 314,428 ns/op)

Exception: in single-threaded mode (GOMAXPROCS=1), channels can outperform ring buffers on contention-heavy workloads.

## Defer overhead

`defer` is not zero-cost. Benchmarks show ~5x overhead (96.6 vs 19.5 ns/op for mutex unlock). In tight loops on critical paths, this adds up. The compiler cannot always optimize defers away because they can appear in conditionals and loops, and the compiler must account for panics as additional exit points.

## Reflection and JSON

Avoid reflection in latency-sensitive code. Code-generated JSON (ffjson) is ~38% faster than reflection-based `encoding/json`. MessagePack with code generation is dramatically faster still:

- Marshal: 555 ns/op (msgpack) vs 7,063 ns/op (JSON reflection)
- Unmarshal: 94.6 ns/op (msgpack) vs 9,362 ns/op (JSON reflection)

Interface method calls also carry overhead (~2.97 vs 0.44 ns/op). Sorting 1M interfaces is ~2.7x slower than sorting 1M structs.

## Memory management

Stack allocation avoids expensive malloc: 11.6 vs 62.3 ns/op. For short-lived objects in concurrent code, `sync.Pool` yields ~5x improvement over raw heap allocation (65.5 vs 337 ns/op). Note that `sync.Pool` drains during GC; for persistence across GC cycles, maintain your own free list.

## False sharing

When two frequently accessed fields fit in the same 64-byte CPU cache line, writing one evicts the other. Adding padding between contended struct fields can help:

- Single producer/consumer: ~15% improvement
- 100 producers/consumers: ~36% improvement

## Lock-freedom tradeoffs

Lock-free data structures can provide major wins (e.g., Ctrie snapshots are 110x faster than synchronized maps) but are treacherous to implement. The author's Ctrie had a subtle bug: Go's spec says two zero-size variables may share the same memory address, which broke generation comparisons. Debugging concurrent lock-free code is described as "hell."

## Practical advice

Optimize along the critical path and outward only as necessary. Be empirical, not impulsive. Identify what "adequate performance" means and do not spend time going beyond that point. Speed comes at the cost of simplicity, development time, and continued maintenance.

## Related
