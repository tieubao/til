---
title: "what if GitHub is the devil - curl's pragmatic take"
date: 2021-02-07
captured: "2021-02-07T13:44:06Z"
tags: [open-source, github, community, infrastructure]
source: "GitHub issue tieubao/til#536"
aliases: []
status: refined
---

## Context

Daniel Stenberg, author of curl and libcurl, responds to critics who argue the curl project should leave GitHub. His analysis is a pragmatic framework for evaluating platform dependency in open source projects.

**Source:** [What if GitHub is the devil?](https://daniel.haxx.se/blog/2021/01/28/what-if-github-is-the-devil/)

## Why GitHub

The curl project switched from SourceForge to GitHub almost eleven years ago. GitHub provides practical features, stable service, and acts as a developer hub where millions already have accounts. Being on GitHub reduces friction for contributors and lowers the bar for participation. This network effect is the key advantage.

## Self-hosting is not better

Matching GitHub's uptime and responsiveness with self-hosting would consume volunteer time and energy better spent on development. A small open source project has no "infrastructure department." Self-hosting also loses the network effect and integration with CI and code analysis services.

## Proprietary is not the real issue

Even with an open source hosting alternative, the code still lives on remote servers you cannot physically access. If any host decides to shut down or block your project, the result is the same regardless of whether their platform is proprietary or open source. The risk is in remote hosting itself, not in the license of the hosting software.

## Contingency if GitHub disappears

If GitHub shut down with zero warning:

- **Code**: still safe. Hundreds of developers pull the git repo frequently, creating distributed backups worldwide.
- **CI**: most configuration lives in yaml files in the repo, reusable on another platform.
- **Issues**: daily extractions preserve metadata. Long-standing bugs are documented in the repo. Open issues could be resubmitted.

It would be a significant speed bump but not fatal. Development could resume on a new platform within days.

## The pragmatic stance

Nothing lasts forever. The curl project has switched services several times over twenty years and expects to do so again. The strategy is: minimize risk, have a contingency plan, and when the time comes, make the jump and continue forward.

## Related
