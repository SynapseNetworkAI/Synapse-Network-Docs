# MCP Docs Index

Canonical MCP docs:

https://docs.synapse-network.ai/mcp

Important pages:

- Overview: https://docs.synapse-network.ai/mcp
- Quick Start: https://docs.synapse-network.ai/mcp/quickstart
- Authentication: https://docs.synapse-network.ai/mcp/authentication
- ChatGPT Custom MCP App: https://docs.synapse-network.ai/mcp/chatgpt
- OpenAI Responses API: https://docs.synapse-network.ai/mcp/openai-responses-api
- Codex: https://docs.synapse-network.ai/mcp/codex
- Claude: https://docs.synapse-network.ai/mcp/claude
- Cursor: https://docs.synapse-network.ai/mcp/cursor
- VS Code: https://docs.synapse-network.ai/mcp/vscode
- Registry: https://docs.synapse-network.ai/mcp/registry

Runtime surfaces:

- Hosted Remote MCP: `https://mcp.synapse-network.ai/mcp`
- Local stdio MCP: `npx -y @synapse-network-ai/mcp-server`
- Registry name: `io.github.SynapseNetworkAI/synapse-network-mcp-server`

Security boundary:

- `discover_services` and `get_receipt` are read-only.
- `invoke_and_pay` is consequential and should require user confirmation.
- Use a dedicated `agt_xxx` Agent Credential with bounded spend for BYOK clients.
- ChatGPT and Codex OAuth receive OAuth tokens, not raw Agent Keys.
