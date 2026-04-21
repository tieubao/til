---
title: "MCP tool schema caching in Claude.ai connectors"
date: 2026-03-26
captured: 2026-03-26T15:53:16.404Z
tags: ["mcp", "cloudflare", "debugging"]
source: "Claude.ai chat"
aliases: []
status: refined
---
## Question

When you update a Cloudflare Worker that serves as a custom MCP connector in Claude.ai, how long does it take for schema changes (new parameters, updated tool definitions) to propagate?

## Answer

Claude.ai caches the MCP tool schema when the connector is first loaded in a session. Redeploying the worker alone is not enough to refresh the schema on the Claude side.

To force a schema refresh:
1. Disconnect the custom connector in Claude.ai settings
2. Reconnect it
3. Start a new conversation (existing conversations may still use the cached schema)

The worker itself serves the updated schema immediately after deploy. The delay is on the client caching side, not the server.

## Key Takeaway

If you change MCP tool parameters and Claude still shows the old schema, it's a client cache issue. Disconnect and reconnect the connector.

## Related

- [[security-gates-for-mcp-tools-that-bridge-private-to-public]] - building MCP tools on Cloudflare Workers where schema updates need to propagate
- [[context-hub-vs-context7-vs-the-context-layer-ecosystem]] - other MCP-based tools in the context layer that face similar caching behavior