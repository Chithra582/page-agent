---
name: "web-mcp-protocol-server"
description: "Bridges browser DOM interactions into standard Model Context Protocol (MCP) tool endpoints."
---

# Web MCP Protocol Server

## Overview
This skill provides a Model Context Protocol (MCP) server bridge, exposing browser automation and live DOM inspection capabilities as standard MCP tools to external AI coding agents and IDEs.

## Key Capabilities
- **JSON-RPC Over WebSockets**: Streams MCP tool requests and responses between external clients and the browser.
- **Tool Catalog Publishing**: Automatically publishes `click`, `type`, `inspect_page`, and `navigate` tools.
- **Resource Streaming**: Streams live page screenshots and accessibility summaries as MCP resources.

## Operational Workflow
1. **Server Initialization**: Start local WebSocket server on the configured MCP port.
2. **Handshake Negotiation**: Process MCP initialize request and return supported tool catalog.
3. **Tool Call Routing**: Receive `tools/call` JSON-RPC requests and forward to in-page dispatcher.
4. **Result Packaging**: Format DOM execution results into structured MCP response payloads.
