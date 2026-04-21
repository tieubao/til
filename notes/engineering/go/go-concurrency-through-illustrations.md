---
title: "Go concurrency through illustrations"
date: 2018-06-07
captured: 2018-06-07T19:35:58Z
tags: [golang, concurrency, goroutines, channels]
source: "GitHub issue tieubao/til#377 + https://medium.com/@trevor4e/learning-gos-concurrency-through-illustrations-8c4aff603b3"
aliases: []
status: refined
---

## Context

A visual introduction to Go's concurrency model by Trevor Forrey, using mining analogies and illustrations to explain goroutines, channels, and select statements. Good entry-level reference for developers new to Go's concurrency primitives.

**Source:** [Learning Go's Concurrency Through Illustrations](https://medium.com/@trevor4e/learning-gos-concurrency-through-illustrations-8c4aff603b3)

## Core primitives

**Goroutines** are lightweight threads created with the `go` keyword. They enable independent execution without the overhead of OS threads. The Go runtime multiplexes goroutines onto a small number of OS threads.

**Channels** are typed communication pipes that allow goroutines to synchronize and exchange data safely. Send with `ch <- value`, receive with `value := <-ch`.

## Channel behavior

| Type | Behavior |
|------|----------|
| Unbuffered | Sender blocks until receiver is ready, and vice versa |
| Buffered | Sender blocks only when buffer is full; receiver blocks when empty |

Key insight: buffered channels do not eliminate blocking entirely. A sufficiently fast sender will still block against a slower receiver once the buffer fills.

## Patterns

**Blocking for completion.** The main goroutine must block (via channel receive or `sync.WaitGroup`) to prevent premature exit before worker goroutines finish.

**Range over channels.** Use `for value := range ch` to receive values until the channel is closed. No need to hardcode iteration counts.

**Select for multiplexing.** The `select` statement listens on multiple channels simultaneously, executing whichever case is ready first. Adding a `default` case makes it non-blocking.

```go
select {
case msg := <-ch1:
    fmt.Println(msg)
case msg := <-ch2:
    fmt.Println(msg)
default:
    fmt.Println("no message ready")
}
```

## Takeaways

- Use channels for inter-goroutine communication rather than shared memory
- Always ensure the main function blocks long enough for goroutines to complete
- Buffered channels decouple send/receive timing but do not eliminate backpressure
- `select` is the idiomatic way to handle multiple concurrent channel operations

## Related
- [[channels-in-golang]] - deeper dive into channel mechanics
- [[building-worker-pool-in-go]] - practical application of these concurrency primitives
- [[million-websockets-and-go]] - advanced concurrency optimization at scale
