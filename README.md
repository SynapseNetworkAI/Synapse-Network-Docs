# Synapse AgentPay

The first B2A (Business-to-Agent) crypto payment gateway for AI agents.

[Read the Full Documentation](https://staging.synapse-network.ai/docs) ·
[Python SDK](https://staging.synapse-network.ai/docs/sdk/python) ·
[TypeScript SDK](https://staging.synapse-network.ai/docs/sdk/typescript) ·
[SDK Repository](https://github.com/cliff-personal/Synapse-Network-Sdk)

Synapse lets agents discover services, pay with USDC-priced credentials, invoke APIs, and verify settlement with receipts.

## 30-Second Agent Quickstart

Step 1: Get your Agent Key.

Open the Synapse Gateway Dashboard, connect your wallet, and generate an Agent Key that starts with `agt_`.

Step 2: Let your agent discover and work.

```python
from synapse_client import SynapseClient

client = SynapseClient(api_key="agt_xxx", environment="staging")

services = client.search("free", limit=10)
service = services[0]

result = client.invoke(
    service.service_id,
    {"prompt": "hello"},
    cost_usdc=float(service.price_usdc),
    idempotency_key="agent-job-001",
)

receipt = client.get_invocation(result.invocation_id)
print(receipt.invocation_id, receipt.status, receipt.charged_usdc)
```

```ts
import { SynapseClient } from "@synapse-network/sdk";

const client = new SynapseClient({
  credential: "agt_xxx",
  environment: "staging",
});

const services = await client.search("free", { limit: 10 });
const service = services[0];

const result = await client.invoke(
  service.serviceId ?? service.id!,
  { prompt: "hello" },
  {
    costUsdc: Number(service.pricing?.amount ?? 0),
    idempotencyKey: "agent-job-001",
  }
);

const receipt = await client.getInvocation(result.invocationId);
console.log(receipt.invocationId, receipt.status, receipt.chargedUsdc);
```

## Why Synapse

- Agent-first runtime: agents use `SynapseClient` and an `agt_xxx` key.
- Discovery before execution: agents search services and read the current price before invoking.
- Settlement evidence: every invocation can be checked with a receipt.
- Staging-first preview: production docs and domains are reserved until GA.

## For AI Coding Agents

Use [llm-instructions.md](./llm-instructions.md) as the compact integration prompt for Cursor, Copilot, Codex, and other coding agents.

The most important rule: agent runtime code should use `SynapseClient`, not owner/admin wallet setup.

## Full Docs

This repository is the GitHub funnel and agent-readable index. The polished product documentation lives on the Gateway docs site:

- SDK hub: <https://staging.synapse-network.ai/docs/sdk>
- Python SDK: <https://staging.synapse-network.ai/docs/sdk/python>
- TypeScript SDK: <https://staging.synapse-network.ai/docs/sdk/typescript>
- Concepts: <https://staging.synapse-network.ai/docs/concepts>
- Contracts: <https://staging.synapse-network.ai/docs/contracts>

V1 intentionally does not expose "Edit this page" links from the docs site because this repository is not yet the source of truth for every rendered docs page. Please use GitHub Issues for docs feedback until docs-source sync lands.

## Feedback

- Report docs issues: <https://github.com/cliff-personal/Synapse-Network-Docs/issues>
- Ask in Community / Discussions: <https://github.com/cliff-personal/Synapse-Network-Docs/discussions>
- SDK code and examples: <https://github.com/cliff-personal/Synapse-Network-Sdk>

Credential handling and vulnerability reporting live in the SDK `SECURITY.md`.
