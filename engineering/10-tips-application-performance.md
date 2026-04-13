---
title: "10 tips to improve application performance"
date: 2018-01-26
captured: 2018-01-26T09:31:39Z
tags: [performance, web, nginx]
source: "GitHub issue tieubao/til#350 + https://www.nginx.com/blog/10-tips-for-10x-application-performance/"
aliases: []
status: refined
---

## Context

NGINX published a guide on achieving up to 10x application performance improvements through a combination of infrastructure, web server, and code-level optimizations. These tips apply broadly to web applications regardless of language or framework.

**Source:** [10 Tips for 10x Application Performance - NGINX](https://www.nginx.com/blog/10-tips-for-10x-application-performance/)

**Attachment:** [10 Tips to Improve Application Performance - NGINX.pdf](https://github.com/tieubao/til/files/1667278/10.Tips.to.Improve.Application.Performance.NGINX.pdf)

## The 10 tips

**1. Use a reverse proxy server.** Place NGINX or similar in front of your application server to handle static content, SSL termination, and connection management. This alone can yield 2-3x throughput.

**2. Add load balancing.** Distribute requests across multiple application servers. Start with round-robin, then move to least-connections or IP-hash for session affinity.

**3. Cache static and dynamic content.** Use both server-side caching (reverse proxy cache) and client-side caching (Cache-Control headers). Even caching dynamic content for 1 second helps under high load.

**4. Compress data.** Enable gzip/deflate compression for text-based responses. The CPU cost is almost always worth the bandwidth savings.

**5. Optimize SSL/TLS.** Enable HTTP/2, use session caching, use OCSP stapling, and tune cipher suites. SSL overhead is manageable with proper configuration.

**6. Implement HTTP/2.** HTTP/2's multiplexing, header compression, and server push can dramatically improve page load times. Requires SSL but the combined benefit outweighs the cost.

**7. Tune Linux and web server configuration.** Adjust backlog queues, worker processes, file descriptors, and ephemeral ports. Default OS settings are rarely optimal for high-traffic servers.

**8. Monitor real-time activity.** Instrument your application with metrics collection. You cannot optimize what you do not measure. Track response times, error rates, and resource utilization.

**9. Optimize application code.** Profile before optimizing. Common wins: reduce blocking I/O, minimize database queries per request, use connection pooling, avoid synchronous external API calls.

**10. Scale up and scale out.** Vertical scaling (bigger machines) is simpler; horizontal scaling (more machines) is more resilient. Most applications benefit from a combination of both.

## Key takeaway

The biggest gains come from the infrastructure layer (reverse proxy, load balancing, caching) rather than code-level micro-optimizations. Start from the outside in.

## Related
