# 📚 Documentation index

This documentation set describes the current scaffold and the engineering requirements for turning it into a secure Tripzy API gateway. It intentionally distinguishes current behavior from proposed architecture.

## Start here

| Need | Guide |
|---|---|
| Understand project intent and boundaries | [Project overview](./project-overview.md) |
| Understand system boundaries and request flow | [Architecture](./architecture.md) |
| See the proposed code layout | [Project structure](./project-structure.md) |
| Install and inspect the current scaffold | [Installation](./installation.md) |
| Work on implementation | [Development](./development.md) |

## API and service integration

| Guide | Coverage |
|---|---|
| [API conventions](./api.md) | Versioning, auth metadata, errors, status codes |
| [Service contract register](./service-contracts.md) | Upstream owners, routes, timeouts and dependencies |
| [Configuration](./configuration.md) | Environment variables, secret handling and validation |
| [Database boundary](./database.md) | Stateless gateway, caching/session exceptions |

## Security and operations

| Guide | Coverage |
|---|---|
| [Security engineering](./security.md) | Authentication, authorization, SSRF, headers, limits |
| [Threat model](./threat-model.md) | Assets, threats, trust boundaries and mitigations |
| [Testing](./testing.md) | Unit, contract, integration, resilience and load testing |
| [Deployment](./deployment.md) | Environment, health, rollout and rollback |
| [Operations](./operations.md) | Logging, metrics, incident runbooks and SLOs |
| [Troubleshooting](./troubleshooting.md) | Diagnostic workflows |
| [FAQ](./faq.md) | Current status and design questions |

## Documentation rules

1. Never describe a proposed route or service as implemented.
2. Use real, reviewed service contracts before defining upstream destinations.
3. Keep examples synthetic and use reserved example domains.
4. Do not include secrets, private network addresses, personal data, or real customer payloads.
5. Update the service register, API reference, threat model, tests, and README together when a route changes.
6. Record retry/idempotency behavior explicitly.
7. Mark unknown owners, paths, timeouts, and auth requirements as undecided rather than guessing.
8. Keep Mermaid source editable in the repository.

## Change impact map

| Change | Required documentation review |
|---|---|
| New route | API, service contracts, architecture, security, testing |
| New upstream | Service contracts, configuration, threat model, operations |
| New auth/permission | Security, API, threat model, testing |
| Timeout/retry changes | Service contracts, API, operations, testing |
| New environment variable | Configuration, installation, deployment |
| New persistent state | Database, privacy review, deployment, threat model |
| New workflow | Development, testing, contributing |
