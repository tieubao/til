---
title: "testing strategies in a microservice architecture"
date: 2016-08-17
captured: "2016-08-17T11:33:52Z"
tags: [microservices, testing, architecture]
source: "GitHub issue tieubao/til#253"
aliases: []
status: refined
---

## Context

Toby Clemson's article on Martin Fowler's site, laying out a systematic testing strategy for microservices. The core insight: testing strategies that work for monolithic applications need fundamental rethinking once network partitions exist between components.

**Source:** [Testing Strategies in a Microservice Architecture](http://martinfowler.com/articles/microservice-testing/)

## The five testing layers

**Unit tests** validate individual components in isolation. Fast, cheap, and numerous. They form the broad base of the test pyramid.

**Integration tests** verify that communication paths between a service and its external dependencies (databases, other services, message queues) work correctly. These catch serialization bugs, protocol mismatches, and config drift.

**Component tests** exercise a single microservice as a standalone unit, stubbing or mocking its downstream dependencies. They validate the service's behavior through its public API without requiring the full system to be running.

**Contract tests** verify that the interface between two services meets the expectations of both producer and consumer. They catch breaking API changes early without needing end-to-end environments. Tools like Pact formalize these contracts.

**End-to-end tests** validate complete user journeys across the full system. They are slow, expensive, and brittle, but catch integration issues that nothing else can. Use sparingly.

## The test pyramid for microservices

The classic test pyramid still applies, but with adjustments:

- Many unit tests (fast, focused)
- Fewer integration tests (external boundaries)
- Component tests replace some integration tests
- Contract tests are a new layer specific to microservices
- Minimal end-to-end tests (slow, high maintenance cost)

The key tradeoff: as you move up the pyramid, tests become slower and more realistic. As you move down, tests become faster but more isolated from real behavior. Contract tests help bridge this gap by verifying inter-service agreements without full deployment.

## Key takeaway

Microservices do not reduce testing complexity; they redistribute it. The shift from in-process calls to network calls means you must explicitly test boundaries that were previously implicit. Contract testing is the most important new tool in the microservice testing arsenal.

## Related

- [[creating-a-microservice-ten-questions]] - question 1 is "how will it be tested?"
- [[hidden-dividends-of-microservices]] - testing complexity is one of the hidden costs
- [[devops-team-topologies]] - team structure affects testing ownership
