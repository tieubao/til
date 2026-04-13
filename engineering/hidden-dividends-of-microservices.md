---
title: "the hidden dividends of microservices"
date: 2016-06-26
captured: "2016-06-26T01:11:04Z"
tags: [microservices, architecture, distributed-systems, better-dev]
source: "GitHub issue tieubao/til#242"
aliases: []
status: refined
---

## Context

Tom Killalea's article on the less obvious benefits of microservices beyond the usual pitch of agility, resilience, and scalability. The piece also includes warning signs that your microservices adoption is incomplete.

**Source:** ACM Queue article on hidden dividends of microservices

## The eight dividends

1. **Permissionless innovation.** When service APIs are sufficient for integration, teams don't need cross-team meetings to coordinate. High experimentation rate + low cross-team meeting rate = permissionless innovation working.

2. **Enable failure.** Microservices increase the number of failures, but designing for routine failure leads to healthy conversations about blast radius, caching, throttling, load shedding, and graceful degradation. Individual service failure should be expected; cascading failure should be impossible.

3. **Disrupt trust.** As teams grow past Dunbar's number, personal trust breaks down. Microservices replace implicit trust with explicit APIs and SLAs, combining autonomy with accountability. This aligns with Conway's law.

4. **You build it, you own it.** Werner Vogels' model: the team that builds a service also operates it. This drives adoption of continuous deployment, containerization, automated elasticity, and self-healing.

5. **Accelerate deprecations.** In a monolith, deprecation is dangerous. With microservices, you can see call volume, stand up competing versions, or transfer ownership to the team that cares most. Unused services naturally fade.

6. **End centralized metadata.** No more DB Cabal reviewing every schema change. Consumers should not know or care how data persists behind APIs. Persistence mechanisms become swappable.

7. **Concentrate the pain.** Compliance burden can be concentrated in a small number of critical services, freeing the rest to innovate with less governance overhead.

8. **Test differently.** Clearer ownership incentivizes better coverage. Continuous deployment + smoke tests + phased deployment yield higher fidelity testing. Test effectiveness is measured by the rate of change enabled, not just problem detection rate.

## Warning signs you are not really doing microservices

- Different services do coordinated deployments
- You ship client libraries
- A change in one service requires changes in others
- Services share a persistence store
- Engineers need intimate knowledge of other teams' schemas
- Compliance controls apply uniformly to all services
- Infrastructure is not programmable

## Related
