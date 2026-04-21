---
title: "advantages of monolithic version control"
date: 2016-09-28
captured: "2016-09-28T06:46:36Z"
tags: [monorepo, version-control, tooling, better-dev]
source: "GitHub issue tieubao/til#262"
aliases: []
status: refined
---

## Context

Dan Luu's essay on why companies like Google, Facebook, Twitter, Digital Ocean, and Etsy choose monorepos. The piece counters the common reaction that monorepos are ridiculous by laying out concrete benefits.

**Source:** [danluu.com/monorepo](https://danluu.com/monorepo/)

## Key advantages

**Simplified organization.** Projects can be grouped by logic, not by VCS constraints. Navigation uses a single filesystem idiom instead of two levels (within-project and between-project). Dev environment setup becomes natural: if you can `cd` between projects, you expect `cd; make` to work, and the tooling effort to make it work gets done.

**Simplified dependencies.** One universal version number for all projects. Atomic cross-project commits keep the repo in a consistent state at every commit. Build files (Makefiles, BUILD) don't need version numbers.

**Better tooling.** Tools only need to read files and a dependency format, not understand inter-repo relationships. Google's build system (later open-sourced as Bazel) enabled Lego-like development: want a crawler? Add a few lines. RSS parser? A few more. This created a complexity barrier between Google-internal and open-source development.

**Cross-project changes.** Refactoring an API with thousands of usages across hundreds of projects becomes a single commit. With many repos, the same task requires tedious manual coordination or hacky scripts. David Turner (Twitter) noted that monorepos also force dependees to update, preventing stagnation.

**Easier tracking.** `git bisect` works across project boundaries without separate meta-information tools.

## Downsides (acknowledged)

Monorepos are not strictly superior. Git and Mercurial need patching for scale (Twitter patched git, Facebook patched Mercurial). The essay deliberately does not detail downsides because they are already widely discussed.

## Takeaway

Monorepos trade scaling challenges for organizational simplicity, better tooling, and painless cross-project changes. The choice is not obvious either way, but it is not unreasonable.

## Related
