---
title: "Apache ZooKeeper for distributed coordination"
date: 2017-02-02
captured: "2017-02-02T04:21:42Z"
tags: [distributed-systems, zookeeper, coordination, architecture]
source: "GitHub issue tieubao/til#291"
aliases: []
status: refined
---

## Context

A Cloudera blog post and presentation on using Apache ZooKeeper to build distributed applications. ZooKeeper solves the coordination problem that every distributed system eventually faces: how do multiple nodes agree on configuration, leadership, and group membership?

**Source:** [How-to: Use Apache ZooKeeper to Build Distributed Apps (and Why)](http://blog.cloudera.com/blog/2013/02/how-to-use-apache-zookeeper-to-build-distributed-apps-and-why/)

**Attachment:** [zookeeper-120427165658-phpapp02.pdf](https://github.com/tieubao/til/files/746814/zookeeper-120427165658-phpapp02.pdf)

## What ZooKeeper is

A centralized service for maintaining configuration information, naming, distributed synchronization, and group services. It provides a simple file-system-like data model (a tree of znodes) with strong consistency guarantees.

## Core abstractions

**Znodes** are data nodes in a hierarchical namespace (like a filesystem). They can be persistent (survive server restarts) or ephemeral (deleted when the creating session ends). Ephemeral znodes are the key primitive for detecting node failures.

**Watches** are one-time triggers that notify clients when a znode changes. A client sets a watch on a path and gets notified once when that path is created, deleted, or modified. This enables reactive coordination without polling.

**Sessions** represent a client's connection to the ZooKeeper ensemble. Ephemeral znodes are tied to sessions; when a session expires (client crashes or disconnects), its ephemeral znodes are automatically cleaned up.

## Common use cases

- **Configuration management** - store config in znodes, watch for changes, propagate updates to all nodes
- **Leader election** - candidates create sequential ephemeral znodes; lowest sequence number becomes leader; others watch the node just ahead of them
- **Service discovery** - services register as ephemeral znodes under a path; consumers watch that path; dead services auto-deregister when sessions expire
- **Distributed locks** - similar to leader election using sequential ephemeral znodes
- **Group membership** - each member creates an ephemeral znode; the group is the set of children under a path

## Design considerations

ZooKeeper is designed for coordination data (small, frequently read), not bulk data storage. Write throughput is limited by the consensus protocol (ZAB). Read throughput scales with ensemble size since any server can serve reads. The typical ensemble is 3 or 5 nodes for fault tolerance (tolerates N/2-1 failures).

## Related

- [[creating-a-microservice-ten-questions]] - question 5 mentions ZooKeeper for service discovery
- [[history-of-hadoop]] - ZooKeeper originated from the Hadoop ecosystem
