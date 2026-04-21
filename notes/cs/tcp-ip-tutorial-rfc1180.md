---
title: "a TCP/IP tutorial - RFC 1180"
date: 2020-04-16
captured: 2020-04-16T03:20:40Z
tags: [network, tcp-ip, protocol, rfc]
source: "GitHub issue tieubao/til#488 + https://tools.ietf.org/html/rfc1180"
aliases: []
status: refined
---

## Context

RFC 1180 (January 1991) is an informational document by Theodore Socolofsky and Claudia Kale that provides a foundational walkthrough of how TCP/IP works. It remains one of the clearest introductions to internetworking concepts.

**Source:** [RFC 1180 - A TCP/IP Tutorial](https://datatracker.ietf.org/doc/html/rfc1180)

## Layered architecture

TCP/IP operates through distinct layers, each handling a specific responsibility:

- **Application layer** - network applications (FTP, TELNET, SNMP)
- **Transport layer** - TCP (reliable byte streams) or UDP (unreliable datagrams)
- **IP layer** - creates logical networks from physical segments
- **ARP** - translates IP addresses to Ethernet (physical) addresses
- **Ethernet** - physical transmission

The boxes represent processing of data as it passes through the computer; the lines connecting boxes show the path of data.

## Key concepts

**Address Resolution (ARP):** Converts IP addresses to Ethernet addresses via table lookups. Tables auto-populate through request/response pairs on the local network.

**Routing:** Two modes exist:
- **Direct routing** - communication within a single IP network
- **Indirect routing** - data passes through IP routers to reach different networks. The path is not determined by a central source but by consulting each routing table along the journey.

**IP's role:** The IP module creates logical networks from physical segments, enabling interoperability across diverse hardware.

## Transport layer comparison

| Protocol | Service Type | Guarantees | Use Cases |
|----------|-------------|------------|-----------|
| TCP | Connection-oriented byte stream | Delivery guaranteed | FTP, TELNET |
| UDP | Connectionless datagrams | No guarantees | NFS, SNMP |

## Practical takeaways

1. **Layered abstraction** protects applications from hardware changes
2. **Route tables** are fundamental to network decision-making; errors severely disrupt communication
3. **Address resolution** bridges logical and physical addressing
4. **Protocol selection** depends on reliability vs. efficiency tradeoffs
5. **Network administration** requires careful attention to configuration consistency across hosts

## Related
