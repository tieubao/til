---
title: "the actor model in 10 minutes"
date: 2017-07-02
captured: 2017-07-02T19:54:46Z
tags: [concurrency, actor-model, distributed-systems]
source: "GitHub issue tieubao/til#305 + https://www.brianstorti.com/the-actor-model/"
aliases: []
status: refined
---

## Context

Brian Storti explains the actor model as an alternative to thread-based concurrency. The actor model replaces shared memory and locks with isolated units that communicate through asynchronous message passing, eliminating entire categories of concurrency bugs.

**Source:** [The actor model in 10 minutes](https://www.brianstorti.com/the-actor-model/)

**Attachment:** [The actor model in 10 minutes.pdf](https://github.com/tieubao/til/files/1118049/The.actor.model.in.10.minutes.pdf)

## What is an actor?

An actor is the primitive unit of computation. It receives a message and does computation based on it. The key properties:

- **Isolation** - actors operate independently without shared memory. Each actor maintains private state that is inaccessible to others.
- **Sequential processing** - each actor handles one message at a time, even though multiple actors run simultaneously.
- **Mailboxes** - incoming messages are queued in a mailbox while the actor processes the current message.

## What an actor can do

When receiving a message, an actor can do exactly three things:

1. **Create new actors** - spawn child actors to handle sub-tasks
2. **Send messages** - communicate with other actors asynchronously
3. **Designate behavior** - define how to handle the next message (this is how actors mutate state)

Everything in an actor system is an actor, including supervisors.

## Fault tolerance

The actor model embraces the "let it crash" philosophy. Instead of defensive programming with try-catch blocks everywhere, failed actors are restarted to a known good state by their supervisors. This creates self-healing systems where failures are contained and recovered automatically.

## Distribution

Because actors communicate only through messages, it does not matter if an actor is running locally or on another node. The message-passing abstraction makes distribution transparent, enabling multi-machine systems and failure recovery across network boundaries.

## Implementations

- **Erlang / Elixir** - actors (processes) are the fundamental building block
- **Akka** - actor framework for the JVM (Scala, Java)
- **Celluloid** - actor library for Ruby

## Key takeaway

The actor model solves concurrency by eliminating shared state entirely. Instead of "how do I safely share data between threads," the question becomes "how do I design message protocols between isolated actors." This shift in thinking removes deadlocks, race conditions, and most synchronization bugs by construction.

## Related
