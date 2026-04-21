---
title: "building a worker pool in Go"
date: 2018-01-29
captured: 2018-01-29T15:24:56Z
tags: [golang, architecture, concurrency, worker-pool]
source: "GitHub issue tieubao/til#353 + https://geeks.uniplaces.com/building-a-worker-pool-in-golang-1e6c0fdfd78c"
aliases: []
status: refined
---

## Context

A practical guide to implementing a worker pool pattern in Go, originally published on Uniplaces engineering blog. The worker pool is one of the most common concurrency patterns in Go, used to bound resource usage while processing work items concurrently. The original URL is no longer available.

**Source:** [Building a Worker Pool in Golang](https://geeks.uniplaces.com/building-a-worker-pool-in-golang-1e6c0fdfd78c) (offline)

## The pattern

A worker pool consists of three components:

1. **Job queue** - a buffered channel that holds work items
2. **Workers** - goroutines that pull from the job queue and process items
3. **Dispatcher** - orchestrates worker creation and job distribution

```go
type Job struct {
    Payload interface{}
}

type Worker struct {
    WorkerPool chan chan Job
    JobChannel chan Job
    quit       chan bool
}

func (w Worker) Start() {
    go func() {
        for {
            // Register this worker's job channel into the pool
            w.WorkerPool <- w.JobChannel
            select {
            case job := <-w.JobChannel:
                // Process the job
                process(job)
            case <-w.quit:
                return
            }
        }
    }()
}
```

## Why not just spawn goroutines?

Unbounded goroutine creation leads to:

- Memory exhaustion under load (each goroutine uses ~2-8 KB stack)
- File descriptor limits hit when goroutines open connections
- Thundering herd on downstream services
- Unpredictable latency from GC pressure

A worker pool caps concurrency at a fixed number of workers, providing backpressure and predictable resource usage.

## Design decisions

**Buffered vs unbuffered job channel.** A buffered channel allows the producer to enqueue work without blocking (up to the buffer size), smoothing out bursts. Choose buffer size based on acceptable queue depth.

**Graceful shutdown.** Use a quit channel or `context.Context` to signal workers to stop. Drain the job queue before exiting to avoid losing work.

**Error handling.** Workers should report errors via a results channel rather than panicking. The dispatcher can then aggregate results and handle failures.

## Takeaways

- Worker pools are the standard Go pattern for bounded concurrency
- Always cap the number of concurrent goroutines in production systems
- Use buffered channels for the job queue to handle burst traffic
- Design for graceful shutdown from the start

## Related
- [[go-concurrency-through-illustrations]] - visual intro to the concurrency primitives used here
- [[million-websockets-and-go]] - worker pools as part of a larger optimization strategy
- [[channels-in-golang]] - channel patterns underlying the worker pool design
