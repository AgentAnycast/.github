<p align="center">
  <img src="https://raw.githubusercontent.com/AgentAnycast/agentanycast/main/docs/assets/logo.png" alt="AgentAnycast" width="50%">
</p>

<h3 align="center">Connect AI agents across any network.</h3>

<p align="center">
  <strong>No public IP. No VPN. No configuration.</strong>
</p>

<p align="center">
  <a href="https://github.com/AgentAnycast/agentanycast-python"><img src="https://img.shields.io/pypi/v/agentanycast?label=Python%20SDK&color=blue" alt="PyPI"></a>
  <a href="https://github.com/AgentAnycast/agentanycast-ts"><img src="https://img.shields.io/npm/v/agentanycast?label=TypeScript%20SDK&color=blue" alt="npm"></a>
  <a href="https://github.com/AgentAnycast/agentanycast-node/releases"><img src="https://img.shields.io/github/v/release/AgentAnycast/agentanycast-node?label=Daemon" alt="Release"></a>
  <a href="https://github.com/AgentAnycast/agentanycast#license"><img src="https://img.shields.io/badge/license-Apache%202.0%20%2F%20FSL-green" alt="License"></a>
</p>

---

AgentAnycast is a decentralized P2P runtime for the [A2A (Agent-to-Agent)](https://github.com/a2aproject/A2A) protocol, powered by [libp2p](https://libp2p.io/). It lets AI agents securely communicate across any network — your laptop, a corporate server, or the other side of the internet — without public IPs, VPNs, or any infrastructure setup.

```bash
pip install agentanycast    # or: npm install agentanycast
```

### Why

A2A requires every agent to be an HTTP server with a public URL. This excludes agents behind NAT (laptops, dev machines), behind firewalls (corporate networks), and in privacy-sensitive environments where centralized gateways are unacceptable. AgentAnycast is the transport layer that closes this gap.

### What It Does

**Zero-config connectivity** — One command makes your agent reachable, like ngrok for A2A agents.

**Capability-based routing** — Send tasks by skill, not by address. The network finds the right agent.

**End-to-end encryption** — All traffic is encrypted with Noise_XX. Relay servers only see ciphertext. No plaintext path exists in the codebase.

```
┌──────────────┐
│  Your App    │  Python or TypeScript
└──────┬───────┘
       │ gRPC (local)
┌──────▼───────┐
│  Daemon      │  Go binary, auto-managed by the SDK
│  (libp2p)    │  P2P connections, encryption, NAT traversal
└──────┬───────┘
       │ TCP / QUIC
┌──────▼───────┐
│  Network     │  mDNS (LAN) / Relay (WAN) / DHT (decentralized)
└──────────────┘
```

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

Three ways to send a task:

```python
await node.send_task(peer_id="12D3KooW...", message=msg)        # direct
await node.send_task(skill="translate", message=msg)             # by skill
await node.send_task(url="https://agent.example.com", message=msg)  # HTTP bridge
```

## Repositories

| Repository | Description | Language |
|---|---|---|
| **[agentanycast](https://github.com/AgentAnycast/agentanycast)** | Documentation, specs, discussions — **start here** | — |
| **[agentanycast-python](https://github.com/AgentAnycast/agentanycast-python)** | Python SDK — `pip install agentanycast` | Python |
| **[agentanycast-ts](https://github.com/AgentAnycast/agentanycast-ts)** | TypeScript SDK — `npm install agentanycast` | TypeScript |
| **[agentanycast-node](https://github.com/AgentAnycast/agentanycast-node)** | Core daemon — P2P, E2E encryption, A2A engine | Go |
| **[agentanycast-relay](https://github.com/AgentAnycast/agentanycast-relay)** | Relay server + skill registry, self-hosted | Go |
| **[agentanycast-proto](https://github.com/AgentAnycast/agentanycast-proto)** | Protocol Buffer definitions — single source of truth | Protobuf |

## Documentation

- **[Getting Started](https://github.com/AgentAnycast/agentanycast/blob/main/docs/getting-started.md)** — Install, run your first agent, connect two agents
- **[Architecture](https://github.com/AgentAnycast/agentanycast/blob/main/docs/architecture.md)** — Sidecar model, security design, NAT traversal
- **[Python SDK Reference](https://github.com/AgentAnycast/agentanycast/blob/main/docs/python-sdk.md)** — Complete API documentation
- **[Deployment Guide](https://github.com/AgentAnycast/agentanycast/blob/main/docs/deployment.md)** — Production relay, HTTP bridge, metrics, security
- **[Protocol Reference](https://github.com/AgentAnycast/agentanycast/blob/main/docs/protocol.md)** — A2A envelopes, task lifecycle, gRPC service
- **[Examples](https://github.com/AgentAnycast/agentanycast/blob/main/docs/examples.md)** — Skill routing, framework adapters, streaming, LLM agents

## License

SDKs and Proto definitions are [Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0). Daemon and Relay are [FSL-1.1-Apache-2.0](https://fsl.software/) (auto-converts to Apache-2.0 after 2 years).

## Community

- [GitHub Discussions](https://github.com/AgentAnycast/agentanycast/discussions) — Questions, ideas, show & tell
- [Contributing Guide](https://github.com/AgentAnycast/agentanycast/blob/main/CONTRIBUTING.md) — How to get involved
