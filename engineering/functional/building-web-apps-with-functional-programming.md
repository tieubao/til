---
title: "building web apps with functional programming"
date: 2020-04-16
captured: 2020-04-16T03:24:50Z
tags: [functional]
source: "GitHub issue tieubao/til#489"
aliases: []
status: refined
---

**Source:** [Building a web app with functional programming - PatchGirl blog](https://blog.patchgirl.io/nixos/2020/03/31/nixos.html)

## Context

A series documenting the experience of building PatchGirl, an open-source REST client, using functional programming across the entire stack: Elm on the frontend, Haskell on the backend, and NixOS for infrastructure.

## The stack

**Elm (frontend)** - A functional language that compiles to JavaScript. Provides type safety, no runtime exceptions, and the Model-View-Update architecture pattern.

**Haskell (backend)** - Pure functional language with a strong type system. Handles the server-side logic and API.

**NixOS (infrastructure)** - A declarative package and configuration management system. Enables reproducible builds through a single declarative configuration file.

## NixOS as infrastructure

NixOS manages packages, services, and system settings uniformly. The nginx setup with SSL automation is straightforward through declarative configuration. Reproducible builds require pinning packages to specific versions rather than relying on channels alone.

The initial deployment strategy used simple git pulls via SSH, with potential for NixPkgs and NixOps for more sophisticated package distribution.

## Key takeaways

- NixOS offers transformative benefits for software management but has a steep learning curve
- Documentation remains fragmented across GitHub repos and blog posts
- The author found NixOS superior to traditional Docker approaches despite setup complexity
- While praised as "amazing," NixOS was not deemed fully production-ready at the time due to accessibility barriers for newcomers
- A fully functional stack (Elm + Haskell + NixOS) is viable for real-world web applications

## Related

