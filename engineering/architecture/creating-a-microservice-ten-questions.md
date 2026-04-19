---
title: "creating a microservice - answer these 10 questions first"
date: 2016-05-03
captured: "2016-05-03T13:28:00Z"
tags: [microservices, architecture, distributed-systems, checklist]
source: "GitHub issue tieubao/til#221"
aliases: []
status: refined
---

## Context

A checklist published by Datawire for teams about to build a new microservice. The questions go beyond code to cover operational readiness, failure handling, and lifecycle management. Useful as a pre-flight checklist before any new service gets past the design phase.

**Source:** [Creating a Microservice? Answer these 10 Questions First](https://www.datawire.io/creating-a-microservice-answer-these-10-questions-first/)

## The 10 questions

**1. How will it be tested?**
Unit tests alone are not enough. The service needs both isolation testing (mocks, stubs) and integration/staging environments with real dependencies. Automate everything; manual test gates slow teams down and invite the broken windows problem.

**2. How will it be configured?**
Define how internal behaviors can change at runtime: thread pools, feature flags, infrastructure knobs. Clarify whether config changes require a restart. The ideal service needs no config changes at all during its lifetime.

**3. How will it be consumed?**
Other components need clear contracts: sync vs async, caching expectations, retry/idempotency semantics, latency SLAs. Consumers should know when to fire timeouts or trip circuit breakers.

**4. How will it be secured?**
Avoid over-engineering inter-service security behind a firewall. Let traffic flow between services, but pass authentication tokens (JWT, OAuth, SAML) representing the originating user. Never pass cleartext credentials. Document the auth approach in a client library or sample code.

**5. How will it be discovered?**
Hardcoded addresses break at scale. DNS is better but has TTL and availability blind spots. The sophisticated option is a highly-available registry (e.g., ZooKeeper) where services self-register. Discovery must also report what is not available, not just what is running.

**6. How will it scale?**
Define whether it auto-scales, whether it holds in-memory state (session data), and what shards. Know where it will fail first under load: usually the database for stateful services, the load balancer for stateless ones.

**7. How will it handle dependency failures?**
Use consistent request timeouts plus circuit breakers. Dependent service owners may require exponential backoff to prevent thundering herd scenarios. Test failure modes by simply removing dependencies.

**8. How will the system handle its failure?**
A logging service failing over UDP is tolerable. A payment service failing synchronously is catastrophic. Map each service on a criticality spectrum and test cascading failure scenarios.

**9. How will it be upgraded?**
Rolling upgrades are the baseline. Canary testing, blue/green deployments, and feature flags require extra investment. Define API change policies: additive-only JSON changes avoid breaking consumers. Plan rollback criteria in advance.

**10. How will it be monitored?**
Use existing organizational monitoring tools. Developers must have direct access to their service's metrics to close the feedback loop. Microservice monitoring complexity far exceeds monolith monitoring.

## Key takeaway

Not every answer needs to be sophisticated at launch, but every question needs a conscious answer. Knowing what your service cannot do yet is as important as knowing what it can.

## Related

- [[hidden-dividends-of-microservices]] - the benefits side of the microservice equation
- [[conways-law]] - org structure shapes how these 10 questions get answered
- [[devops-team-topologies]] - team structures that affect testing, discovery, and monitoring
- [[choose-boring-technology]] - relevant to question 5 (discovery) and 9 (upgrades)
