---
title: "benefits of continuous delivery"
date: 2017-11-21
captured: 2017-11-21T07:32:46Z
tags: ["continuous-delivery", "ci-cd", "deployment", "devops"]
source: "GitHub issue tieubao/til#336"
aliases: []
status: refined
---

## Context

Henrik Warne reflecting on the shift from 12-18 month release cycles to continuous delivery, and why deploying each feature as soon as it is ready is the best way to work.

**Source:** [Benefits of Continuous Delivery](https://henrikwarne.com/2017/11/19/benefits-of-continuous-delivery/)

## Benefits

**Lower risk.** Small deploys mean less code to look through when problems arise. A small deploy is also easier to revert than a large one.

**Fresh in your mind.** When you deploy immediately after finishing a feature, everything about it is fresh. Troubleshooting is easier, and you free up mental energy by being completely done with one thing before moving on.

**Features reach customers faster.** Having a feature ready for production but not deploying it is wasteful.

**Faster feedback.** The sooner customers use new features, the sooner you learn what works and what doesn't. Production reveals problems that testing never will - real configuration, real data, real traffic patterns.

## Prerequisites

- **Central servers.** Cloud-based or centrally hosted. Cannot deploy many times a day to customer-controlled on-premise systems.
- **DevOps culture.** "You built it, you run it." (Werner Vogels, CTO of Amazon). Same person writes, tests, deploys, and debugs.
- **Automation.** Almost all build and deploy mechanics must be automated. Docker and Kubernetes work well.
- **Rolling upgrades.** No service interruption during deploys. Deploy server by server.
- **Revertible.** Easy rollback to the previous version.
- **Knowing what is running.** Version numbers combined with git hashes. Every deploy committed in a versions file.

## Common objections

**"More bugs?"** No. There were occasional bugs before, and there are occasional bugs now. The difference is they are easier and faster to find and fix with small deploys.

**"What about soaking?"** Letting features sit in test environments before release sounds good in theory. In practice, remaining bugs are almost always found in production, with real config and data. The advantages of frequent deploys outweigh the potential of finding lurking bugs by delaying.

## Related
