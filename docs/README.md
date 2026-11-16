# 📚 Documentation index

This documentation set describes the gateway scaffold and the engineering requirements for a secure Tripzy API gateway. Proposed architecture must not be presented as shipped runtime behavior.

## Product and architecture

| Guide | Coverage |
|---|---|
| [Project overview](./project-overview.md) | Goals, scope, non-goals and current evidence |
| [Architecture](./architecture.md) | Components, boundaries and request lifecycle |
| [Project structure](./project-structure.md) | Proposed source layout and responsibilities |

## API and integration

| Guide | Coverage |
|---|---|
| [API conventions](./api.md) | Versioning, auth metadata, errors and status codes |
| [Service contracts](./service-contracts.md) | Upstream ownership, route template, destination safety |
| [Configuration](./configuration.md) | Environment, validation and secret handling |
| [Database](./database.md) | Stateless design and persistence exceptions |

## Engineering and operations

| Guide | Coverage |
|---|---|
| [Installation](./installation.md) | Current package scaffold setup |
| [Development](./development.md) | Contribution and implementation sequence |
| [Testing](./testing.md) | Unit, contract, integration, security and load tests |
| [Security](./security.md) | Authentication, authorization, SSRF, headers and limits |
| [Threat model](./threat-model.md) | Assets, trust boundaries and mitigation priorities |
| [Deployment](./deployment.md) | Readiness, rollout and rollback |
| [Operations](./operations.md) | Telemetry, SLO planning and incident runbooks |
| [Troubleshooting](./troubleshooting.md) | Diagnostics and known scaffold limitations |
| [FAQ](./faq.md) | Scope and implementation questions |

## Diagrams

Editable Mermaid sources are in [diagrams/](./diagrams/). These are design documents, not routing configuration.

## Documentation maintenance rules

1. Mark proposals clearly and describe current behavior accurately.
2. Use verified Tripzy service contracts before defining routes or destinations.
3. Use synthetic examples and reserved example domains.
4. Never commit secrets, private addresses, tokens, or customer data.
5. Update API, service register, threat model, tests, and README when a route changes.
6. State timeout, retry and idempotency behavior explicitly.
7. Mark unknown owners and policies as undecided rather than guessing.
