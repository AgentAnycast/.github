<p align="center">
  <img src="https://raw.githubusercontent.com/AgentAnycast/agentanycast/main/docs/assets/logo.png" alt="AgentAnycast" width="50%">
</p>

<p align="center">
  <strong>The Connection Layer for AI Agents</strong><br>
  <em>Connect agents across any network — end-to-end encrypted, NAT traversal built in, zero configuration.</em>
</p>

<p align="center">
  <a href="https://pypi.org/project/agentanycast/"><img src="https://img.shields.io/pypi/v/agentanycast?label=Python%20SDK&color=3776AB" alt="PyPI"></a>
  <a href="https://www.npmjs.com/package/agentanycast"><img src="https://img.shields.io/npm/v/agentanycast?label=TypeScript%20SDK&color=3178C6" alt="npm"></a>
  <a href="https://pypi.org/project/agentanycast-mcp/"><img src="https://img.shields.io/pypi/v/agentanycast-mcp?label=MCP%20Server&color=8B5CF6" alt="MCP Server"></a>
  <a href="https://github.com/AgentAnycast/agentanycast-node/releases"><img src="https://img.shields.io/github/v/release/AgentAnycast/agentanycast-node?label=Daemon&color=00ADD8" alt="Daemon"></a>
  <a href="https://github.com/AgentAnycast/agentanycast#license"><img src="https://img.shields.io/badge/license-Apache%202.0%20%2F%20FSL-green" alt="License"></a>
</p>

---

AgentAnycast is a decentralized P2P runtime for the [A2A (Agent-to-Agent)](https://github.com/a2aproject/A2A) protocol. It gives AI agents the ability to find and talk to each other across any network — laptops behind NAT, corporate firewalls, or across the internet — with automatic encryption ([Noise_XX](https://noiseprotocol.org/)), NAT traversal, and skill-based anycast routing.

It also runs as an **MCP server** for Claude Desktop, Cursor, VS Code, and [13+ AI tool platforms](https://github.com/AgentAnycast/agentanycast-mcp).

```bash
pip install agentanycast          # Python SDK
npm install agentanycast          # TypeScript SDK
uvx agentanycast-mcp              # MCP server for AI tools
```

### Why

A2A requires every agent to be an HTTP server with a public URL. This excludes agents behind NAT, behind firewalls, and in environments where centralized gateways are unacceptable. AgentAnycast provides the missing transport layer:

- **No public IP needed** — `pip install` and your agent is reachable from anywhere
- **Find by skill, not address** — anycast routing discovers agents by capability
- **E2E encrypted** — Noise_XX through relay servers that see only ciphertext
- **Works behind any firewall** — automatic NAT traversal (DCUtR + relay fallback)
- **Cross-language** — Python and TypeScript agents interoperate seamlessly

### Quick Example

```python
from agentanycast import Node, AgentCard, Skill

card = AgentCard(
    name="EchoAgent",
    skills=[Skill(id="echo", description="Echo the input")],
)

async with Node(card=card) as node:
    @node.on_task
    async def handle(task):
        text = task.messages[-1].parts[0].text
        await task.complete(artifacts=[{"parts": [{"text": f"Echo: {text}"}]}])

    await node.serve_forever()
```

Three ways to reach another agent:

```python
await node.send_task(peer_id="12D3KooW...", message=msg)                # by Peer ID
await node.send_task(skill="translate", message=msg)                    # by skill (anycast)
await node.send_task(url="https://agent.example.com", message=msg)      # via HTTP bridge
```

### How It Works

A thin SDK (Python or TypeScript) communicates over gRPC with a local Go daemon that handles P2P networking, Noise_XX encryption, and A2A protocol logic. On a LAN, agents discover each other via mDNS. Across networks, a self-hosted relay provides NAT traversal and hosts a skill registry for capability-based routing — the relay cannot read any traffic.

```
App (Python / TypeScript)  ←— gRPC (Unix socket) —→  Daemon (Go)  ←— libp2p —→  Remote Agent
                                                         ↕
                                                   Relay (optional)
```

### Ecosystem

Built-in adapters for **CrewAI**, **LangGraph**, **Google ADK**, **OpenAI Agents SDK**, **Claude Agent SDK**, and **AWS Strands Agents**. Bidirectional MCP Tool ↔ A2A Skill mapping. HTTP bridge for interop with standard A2A agents. W3C DID (`did:key`) identity with Verifiable Credentials support.

## Repositories

| Repository | Description |
|---|---|
| **[agentanycast](https://github.com/AgentAnycast/agentanycast)** | Documentation, protocol definitions, examples — **start here** |
| **[agentanycast-python](https://github.com/AgentAnycast/agentanycast-python)** | Python SDK — `pip install agentanycast` |
| **[agentanycast-ts](https://github.com/AgentAnycast/agentanycast-ts)** | TypeScript SDK — `npm install agentanycast` |
| **[agentanycast-mcp](https://github.com/AgentAnycast/agentanycast-mcp)** | MCP server — `uvx agentanycast-mcp` — works with 13+ AI tools |
| **[agentanycast-node](https://github.com/AgentAnycast/agentanycast-node)** | Go daemon — P2P networking, encryption, A2A engine |
| **[agentanycast-relay](https://github.com/AgentAnycast/agentanycast-relay)** | Relay server — Circuit Relay v2, skill registry, federation |
| **[agentanycast-identity](https://github.com/AgentAnycast/agentanycast-identity)** | Identity library — W3C DID, Verifiable Credentials, SPIFFE/OIDC |

## Documentation

- **[Getting Started](https://github.com/AgentAnycast/agentanycast/blob/main/docs/getting-started.md)** — Install, run your first agent, connect two agents
- **[Architecture](https://github.com/AgentAnycast/agentanycast/blob/main/docs/architecture.md)** — Sidecar model, security, NAT traversal, MCP, federation
- **[Python SDK Reference](https://github.com/AgentAnycast/agentanycast/blob/main/docs/python-sdk.md)** — Complete API documentation
- **[Deployment Guide](https://github.com/AgentAnycast/agentanycast/blob/main/docs/deployment.md)** — Production relay, HTTP bridge, metrics, security
- **[Protocol Reference](https://github.com/AgentAnycast/agentanycast/blob/main/docs/protocol.md)** — Wire format, task lifecycle, gRPC services
- **[Examples](https://github.com/AgentAnycast/agentanycast/blob/main/docs/examples.md)** — Skill routing, framework adapters, streaming, LLM agents

## Community

- [GitHub Discussions](https://github.com/AgentAnycast/agentanycast/discussions) — questions, ideas, show & tell
- [Contributing Guide](https://github.com/AgentAnycast/agentanycast/blob/main/CONTRIBUTING.md) — how to get involved
- [Website](https://agentanycast.io) — project homepage
