<p align="center">
  <img src="https://raw.githubusercontent.com/AgentAnycast/agentanycast/main/docs/assets/logo.png" alt="AgentAnycast" width="50%">
</p>

<h3 align="center">ngrok + DNS for AI Agents</h3>

<p align="center">
  <strong>Zero-config connectivity. Capability-based routing. End-to-end encrypted.</strong>
</p>

<p align="center">
  <a href="https://github.com/AgentAnycast/agentanycast-python"><img src="https://img.shields.io/pypi/v/agentanycast?label=Python%20SDK&color=blue" alt="PyPI"></a>
  <a href="https://github.com/AgentAnycast/agentanycast-ts"><img src="https://img.shields.io/npm/v/agentanycast?label=TypeScript%20SDK&color=blue" alt="npm"></a>
  <a href="https://github.com/AgentAnycast/agentanycast-node/releases"><img src="https://img.shields.io/github/v/release/AgentAnycast/agentanycast-node?label=Daemon" alt="Release"></a>
  <a href="https://github.com/AgentAnycast/agentanycast#license"><img src="https://img.shields.io/badge/license-Apache%202.0%20%2F%20FSL-green" alt="License"></a>
</p>

---

AgentAnycast is a **decentralized P2P runtime for the [A2A (Agent-to-Agent) protocol](https://github.com/a2aproject/A2A)**. It lets AI agents securely discover and collaborate across any network — no public IP, no VPN, no centralized gateway.

```bash
pip install agentanycast    # or: npm install agentanycast
```

### The Problem

The A2A protocol defines how agents collaborate, but its HTTP transport requires every agent to be a server with a public URL. This excludes local agents (behind NAT), corporate agents (behind firewalls), privacy-sensitive deployments, and edge devices.

### The Solution

AgentAnycast provides the missing transport layer:

- **Agent's ngrok** — one command, your agent is reachable from anywhere
- **Agent's DNS** — find agents by capability, not by address (`send_task(skill="translate", ...)`)
- **Agent's zero-trust network** — Noise_XX end-to-end encryption; even relays see only ciphertext

### Architecture

```
┌──────────────┐
│  Your App    │  Python or TypeScript
└──────┬───────┘
       │ gRPC (Unix socket)
┌──────▼───────┐
│   Daemon     │  Go binary, auto-managed by SDK
│  (libp2p)    │  NAT traversal + Noise_XX + A2A engine
└──────┬───────┘
       │ TCP / QUIC
┌──────▼───────┐
│   Network    │  mDNS (LAN) / Relay (WAN) / DHT
└──────────────┘
```

A thin SDK communicates over gRPC with a local Go daemon that handles P2P networking, encryption, and protocol logic. Agents on a LAN discover each other via mDNS with zero configuration; across networks, a self-hosted relay provides NAT traversal and skill-based routing.

## Repositories

| Repository | Description | Language |
|---|---|---|
| **[agentanycast](https://github.com/AgentAnycast/agentanycast)** | Docs, specs, getting started — **start here** | — |
| **[agentanycast-python](https://github.com/AgentAnycast/agentanycast-python)** | Python SDK — `pip install agentanycast` | Python |
| **[agentanycast-ts](https://github.com/AgentAnycast/agentanycast-ts)** | TypeScript SDK — `npm install agentanycast` | TypeScript |
| **[agentanycast-node](https://github.com/AgentAnycast/agentanycast-node)** | Core daemon — P2P, E2E encryption, A2A engine | Go |
| **[agentanycast-relay](https://github.com/AgentAnycast/agentanycast-relay)** | Relay server — Circuit Relay v2, skill registry | Go |
| **[agentanycast-proto](https://github.com/AgentAnycast/agentanycast-proto)** | Protocol Buffer definitions — single source of truth | Protobuf |

## Key Features

| | |
|---|---|
| **Three addressing modes** | Direct (Peer ID), Anycast (skill-based routing), HTTP Bridge (standard A2A interop) |
| **End-to-end encryption** | Noise_XX (Curve25519 + ChaCha20-Poly1305) — no plaintext path exists |
| **NAT traversal** | AutoNAT + DCUtR hole-punching + Circuit Relay v2 fallback |
| **Cryptographic identity** | Ed25519 keypair → Peer ID → W3C `did:key` — self-sovereign, no CA |
| **Framework adapters** | CrewAI and LangGraph integrations included |
| **Ecosystem interop** | A2A native, HTTP Bridge, MCP Tool ↔ Skill mapping, AGNTCY directory |
| **Self-hosted relay** | `docker compose up` — you own your infrastructure |

## Quick Example

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

## Documentation

- **[Getting Started](https://github.com/AgentAnycast/agentanycast/blob/main/docs/getting-started.md)** — Installation, first agent, addressing modes
- **[Architecture](https://github.com/AgentAnycast/agentanycast/blob/main/docs/architecture.md)** — Sidecar model, security, NAT traversal
- **[Python SDK Reference](https://github.com/AgentAnycast/agentanycast/blob/main/docs/python-sdk.md)** — Complete API documentation
- **[Deployment Guide](https://github.com/AgentAnycast/agentanycast/blob/main/docs/deployment.md)** — Production relay, HTTP bridge, metrics
- **[Protocol Reference](https://github.com/AgentAnycast/agentanycast/blob/main/docs/protocol.md)** — A2A envelopes, task lifecycle, gRPC service
- **[Examples](https://github.com/AgentAnycast/agentanycast/blob/main/docs/examples.md)** — Anycast, streaming, LLM agents, framework adapters

## License

SDKs (Python, TypeScript) and Proto definitions are [Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0). Daemon and Relay are [FSL-1.1-Apache-2.0](https://fsl.software/) (auto-converts to Apache-2.0 after 2 years).

## Community

- [GitHub Discussions](https://github.com/AgentAnycast/agentanycast/discussions) — Questions, ideas, show & tell
- [Contributing Guide](https://github.com/AgentAnycast/agentanycast/blob/main/CONTRIBUTING.md) — How to get involved
