# 🗂️ Project structure

## Current state versus target layout

The repository currently has no application source directory. The following is a proposed layout for implementation, not a claim that these files already exist.

```text
API-Gateway-Tripzy/
├── .github/
│   ├── ISSUE_TEMPLATE/
│   ├── workflows/
│   └── PULL_REQUEST_TEMPLATE.md
├── docs/
│   ├── diagrams/
│   ├── api.md
│   ├── architecture.md
│   ├── configuration.md
│   ├── development.md
│   ├── deployment.md
│   ├── operations.md
│   ├── security.md
│   ├── service-contracts.md
│   ├── testing.md
│   └── threat-model.md
├── src/                         # proposed, not yet implemented
│   ├── app.js                   # HTTP app composition
│   ├── server.js                # process startup and shutdown
│   ├── config/
│   │   ├── env.js               # validated environment
│   │   └── services.js          # allowlisted upstream registry
│   ├── middleware/
│   │   ├── request-id.js
│   │   ├── authentication.js
│   │   ├── authorization.js
│   │   ├── rate-limit.js
│   │   ├── request-size.js
│   │   └── error-handler.js
│   ├── routes/
│   │   ├── health.routes.js
│   │   └── index.js
│   ├── clients/
│   │   └── upstream-client.js
│   ├── policies/
│   │   ├── headers.js
│   │   ├── retry.js
│   │   └── timeout.js
│   └── observability/
│       ├── logger.js
│       └── metrics.js
├── tests/                       # proposed
│   ├── unit/
│   ├── contract/
│   ├── integration/
│   └── security/
├── .env.example
├── .gitignore
├── package.json
├── README.md
└── SECURITY.md
```

## Directory responsibilities

| Directory | Responsibility |
|---|---|
| `src/config` | Validate environment and trusted service registry |
| `src/middleware` | Cross-cutting inbound request controls |
| `src/routes` | Public route definitions only |
| `src/clients` | Outbound HTTP behavior |
| `src/policies` | Header, timeout, retry and policy helpers |
| `src/observability` | Safe structured logs and metrics |
| `tests/unit` | Pure function and middleware tests |
| `tests/contract` | Validate upstream request/response contracts |
| `tests/integration` | Exercise gateway against mock upstreams |
| `tests/security` | Abuse, bypass, SSRF and header tests |

## File design rules

- Keep route configuration separate from request handlers.
- Keep environment parsing in one module.
- Do not export mutable global configuration objects.
- Keep network calls behind an upstream client that enforces timeouts.
- Ensure authorization failures occur before outbound requests.
- Keep error conversion centralized.
- Never log credentials or full request/response bodies by default.
- Avoid adding a database until a requirement justifies durable gateway state.
