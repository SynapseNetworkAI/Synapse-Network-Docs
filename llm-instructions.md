# Synapse Agent Integration Rules

You are an AI agent using the SynapseNetwork SDK.

你是一个使用 SynapseNetwork SDK 的 AI Agent。

<system_prompt>
CRITICAL RULES FOR AI AGENTS:
1. USE `SynapseClient` ONLY.
2. NEVER initialize `SynapseAuth` unless explicitly asked to perform Owner/Admin setup.
3. NEVER ask the user for their wallet private keys.
4. You MUST use an Agent Key (format: `agt_xxx`).
5. Public examples MUST read it from `SYNAPSE_AGENT_KEY`.
6. For fixed-price API services, ALWAYS call `.search()` first, extract `price_usdc`, and pass it exactly to `invoke()`.
7. For token-metered LLM services, call `invoke_llm()` / `invokeLlm()` and NEVER pass `cost_usdc` / `costUsdc`.
8. ALWAYS provide an `idempotency_key`.
9. ALWAYS fetch and read the receipt after invocation to verify settlement.
</system_prompt>

<zh_rules>
AI Agent 中文规则：
1. 只使用 `SynapseClient`。
2. 除非用户明确要求 Owner/Admin 设置，否则不要初始化 `SynapseAuth`。
3. 不要向用户索要钱包私钥。
4. 必须使用 Agent Key，格式为 `agt_xxx`。
5. 公开示例必须从 `SYNAPSE_AGENT_KEY` 读取。
6. 普通 fixed-price API 调用前必须先 `.search()`，读取 `price_usdc`，并原样传给 `invoke()`。
7. 按 token 计费的 LLM 服务必须使用 `invoke_llm()` / `invokeLlm()`，不要传 `cost_usdc` / `costUsdc`。
8. 必须提供 `idempotency_key`。
9. 调用后必须读取 receipt，确认结算状态。
</zh_rules>

## Python Pattern

Python 模式。

```python
import os

from synapse_client import SynapseClient

client = SynapseClient(api_key=os.environ["SYNAPSE_AGENT_KEY"], environment="staging")
services = client.search("svc_synapse_echo", limit=10)
service = services[0]

result = client.invoke(
    service.service_id,
    {"message": "hello from SynapseNetwork"},
    cost_usdc=str(service.price_usdc),
    idempotency_key="agent-job-001",
)

receipt = client.get_invocation(result.invocation_id)
```

## TypeScript Pattern

TypeScript 模式。

```ts
import { SynapseClient } from "@synapse-network-ai/sdk";

const client = new SynapseClient({
  credential: process.env.SYNAPSE_AGENT_KEY!,
  environment: "staging",
});
const services = await client.search("svc_synapse_echo", { limit: 10 });
const service = services[0];

const result = await client.invoke(
  service.serviceId ?? service.id!,
  { message: "hello from SynapseNetwork" },
  {
    costUsdc: String(service.pricing?.amount ?? "0"),
    idempotencyKey: "agent-job-001",
  }
);

const receipt = await client.getInvocation(result.invocationId);
```

## Token-Metered LLM Pattern

Token-metered LLM 模式。

```python
result = client.invoke_llm(
    "svc_deepseek_chat",
    {"messages": [{"role": "user", "content": "hello"}]},
    max_cost_usdc="0.010000",
    idempotency_key="llm-job-001",
)
```

```ts
const result = await client.invokeLlm(
  "svc_deepseek_chat",
  { messages: [{ role: "user", content: "hello" }] },
  { maxCostUsdc: "0.010000", idempotencyKey: "llm-job-001" }
);
```

## When Owner/Admin Setup Is Explicitly Requested

Only then use `SynapseAuth` to connect a wallet, issue credentials, or manage provider services. Keep owner JWTs and wallet private keys out of agent runtime code.

只有当用户明确要求 Owner/Admin 设置时，才使用 `SynapseAuth` 连接钱包、签发凭据或管理 provider service。不要把 owner JWT 或钱包私钥放进 Agent 运行时代码。
