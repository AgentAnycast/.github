# Contributing to AgentAnycast

Thank you for your interest in contributing to AgentAnycast! This document provides guidelines for contributing to any repository under the `@AgentAnycast` organization.

## Code of Conduct

Please read and follow our [Code of Conduct](https://github.com/AgentAnycast/agentanycast/blob/main/CODE_OF_CONDUCT.md).

## Getting Started

AgentAnycast is organized as multiple repositories. Choose the one that matches your contribution:

| I want to... | Repository | Language | Setup |
|---|---|---|---|
| Fix/improve the P2P daemon | [agentanycast-node](https://github.com/AgentAnycast/agentanycast-node) | Go 1.24+ | `make build && make test` |
| Fix/improve the Python SDK | [agentanycast-python](https://github.com/AgentAnycast/agentanycast-python) | Python 3.10+ | `pip install -e ".[dev]" && pytest` |
| Fix/improve Protobuf definitions | [agentanycast-proto](https://github.com/AgentAnycast/agentanycast-proto) | Protobuf | `buf lint && buf generate` |
| Fix/improve the Relay server | [agentanycast-relay](https://github.com/AgentAnycast/agentanycast-relay) | Go 1.24+ | `go build ./cmd/relay && go test ./...` |
| Improve docs or report bugs | [agentanycast](https://github.com/AgentAnycast/agentanycast) | Markdown | — |

Each sub-repository has its own CONTRIBUTING.md with language-specific guidelines. Check there for detailed build, test, and style instructions.

## Contribution Workflow

1. **Check existing issues** — Look for open issues or discussions before starting work.
2. **Open an issue first** — For non-trivial changes, open an issue in the [main repo](https://github.com/AgentAnycast/agentanycast/issues) to discuss your approach.
3. **Fork & branch** — Fork the relevant repository and create a feature branch.
4. **Make changes** — Follow the coding standards for that repository.
5. **Test** — Ensure all existing tests pass and add tests for new functionality.
6. **Submit a PR** — Open a pull request with a clear description of your changes.

## Licensing & Contributor Agreements

### Apache 2.0 Repositories (proto, python, docs)

These repositories use **DCO (Developer Certificate of Origin)**. Sign off your commits:

```bash
git commit -s -m "feat: add new feature"
```

See [developercertificate.org](https://developercertificate.org/).

### FSL 1.1 Repositories (node, relay)

These repositories require a **Contributor License Agreement (CLA)**. See the [CLA](https://github.com/AgentAnycast/agentanycast/blob/main/CLA.md) for details.

## Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add agent discovery via DHT
fix: handle reconnection after relay timeout
docs: update getting started guide
test: add NAT traversal integration tests
```

## Cross-Repository Changes

Some changes span multiple repositories (e.g., adding a new RPC method):

1. Start with `agentanycast-proto` — define the new interface
2. Update `agentanycast-node` — implement the server side
3. Update `agentanycast-python` — implement the client side

Open linked PRs in each repository and reference them in your descriptions.

## Reporting Security Issues

Please do **NOT** open a public issue for security vulnerabilities. See [SECURITY.md](https://github.com/AgentAnycast/agentanycast/blob/main/SECURITY.md) for responsible disclosure instructions.

## Questions?

- Open a [Discussion](https://github.com/AgentAnycast/agentanycast/discussions) for general questions
- Join the conversation in existing issues
