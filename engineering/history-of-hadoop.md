---
title: "the history of Hadoop"
date: 2016-02-16
captured: 2016-02-16T00:00:00Z
tags: [hadoop, big-data, distributed-systems, history]
source: "GitHub issue tieubao/til#166"
aliases: []
status: refined
---

## Context

A timeline of Hadoop's evolution from Doug Cutting's full-text search library to the foundation of the big data ecosystem. The story traces how two Google papers changed the trajectory of distributed computing.

**Source:** [The History of Hadoop](https://medium.com/@markobonaci/the-history-of-hadoop-68984a11704)

## Timeline

| Year | Event |
|---|---|
| 1997 | Doug Cutting begins developing Lucene, a full-text search library |
| 2000-01 | Lucene open-sourced, moves to Apache Software Foundation |
| 2001 | Cutting and Mike Cafarella launch Apache Nutch (web crawler) to index the entire web |
| 2003 | Google publishes the Google File System paper - the blueprint for distributed storage |
| 2004 | Team implements NDFS (Nutch Distributed File System) in Java: file chunking and replication for fault tolerance |
| 2004-05 | Google releases MapReduce paper; Cutting and Cafarella integrate it into Nutch |
| 2006 | Hadoop becomes standalone Apache project. Yahoo! employs Cutting to transition their search backend |
| 2007 | Twitter, Facebook, LinkedIn begin serious Hadoop development |
| 2008 | Hadoop graduates to top-level Apache project. HBase, ZooKeeper, Pig, Hive emerge. Cloudera founded |
| 2009 | Amazon launches Elastic MapReduce |
| 2011 | Hortonworks bootstrapped by former Yahoo! engineers |
| 2012 | Yahoo!'s cluster reaches 42,000 nodes |

## Key people

- **Doug Cutting** - architect behind Lucene, Nutch, and Hadoop
- **Mike Cafarella** - co-developer of Nutch and HDFS
- **Eric Baldeschwieler** - led Yahoo!'s Hadoop transition
- **Jeffrey Dean and Sanjay Ghemawat** - Google researchers who authored the MapReduce paper

## Key insight

Hadoop's entire existence traces back to two Google papers that the company published openly. The system evolved from solving single-machine indexing limitations to a distributed paradigm: schemaless storage, automatic failure recovery, and parallel processing became foundational principles for processing previously insurmountable data volumes.

## Related

- [[choose-boring-technology]] - Hadoop was once the exciting choice that later became "boring" infrastructure
