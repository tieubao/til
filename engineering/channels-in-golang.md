---
title: "channels in Golang"
date: 2016-12-25
captured: 2016-12-25T04:29:29Z
tags: [golang, concurrency, channels]
source: "GitHub issue tieubao/til#274 + https://go101.org/article/channel.html"
aliases: []
status: refined
---

## Context

Channels are Go's primary concurrency primitive, embodying the philosophy: "don't communicate by sharing memory, share memory by communicating." Understanding their internal mechanics and edge cases is essential for writing correct concurrent Go programs.

**Source:** [Channels in Golang](https://go101.org/article/channel.html)

**Attachment:** [Channels in Golang.pdf](https://github.com/tieubao/til/files/672056/Channels.in.Golang.pdf)

## Channel types and buffering

Go provides three channel directions:

- **Bidirectional** (`chan T`) - send and receive
- **Send-only** (`chan<- T`) - transmit only
- **Receive-only** (`<-chan T`) - accept only

Bidirectional channels convert implicitly to directional types, but not the reverse. Unbuffered channels (zero capacity) block until a matching send/receive pair exists. Buffered channels proceed until the buffer fills or empties.

## Nil, closed, and open channel behavior

| Operation | Nil Channel     | Closed Channel              | Open Channel         |
|-----------|----------------|-----------------------------|----------------------|
| Close     | Panic          | Panic                       | Succeeds             |
| Send      | Blocks forever | Panic                       | Blocks or succeeds   |
| Receive   | Blocks forever | Never blocks; returns zero   | Blocks or succeeds   |

Receiving from a closed channel never blocks and yields zero values indefinitely. Use the optional boolean flag (`v, ok := <-ch`) to detect closure.

## Internal architecture

Each channel maintains three FIFO queues:

1. **Receiving goroutine queue** - blocked receivers awaiting data
2. **Sending goroutine queue** - blocked senders waiting for capacity
3. **Value buffer queue** - circular queue holding actual values

All operations acquire an internal mutex lock for thread-safety.

## Practical patterns

**Try-send / try-receive** using select with a default case prevents blocking when a channel is full or empty. **Range iteration** (`for v := range ch`) processes values until the channel closes. When multiple select cases are ready, Go randomly selects one, preventing starvation.

## Gotchas

- Avoid concurrent send-and-close; it causes race conditions and panics
- Large element types should use pointers to minimize copy overhead
- Goroutines blocked on channel queues cannot be garbage collected until they exit
- Channels are not always the answer; consider `sync` package primitives for simpler coordination

## Related
