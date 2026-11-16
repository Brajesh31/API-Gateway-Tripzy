# 🗂️ Project structure

## Current state

The repository does not yet contain application source files. The following is a proposed layout for the gateway implementation, not a claim that these files already exist.

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
├── src/                       # proposed
│   ├── app.js
│   ├── server.js
│   ├── config/
│   │   ├── env.js
│   │   └── services.js
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
│   ├── clients/upstream-client.js
│   ├── policies/
│   │   ├── headers.js
│   │   ├── retry.js
│   │   └── timeout.js
│   └── observability/
│       ├── logger.js
│       └── metrics.js
├── tests/                     # proposed
│   ├── unit/
│   ├── contract/
│   ├── integration/
│   └── security/
├── .env.example
├── package.json
├── README.md
└── SECURITY.md
```

## Module responsibilities

| Module | Responsibility | Boundary |
|---|---|---|
| `src/app.js` | Compose middleware and routes | No process startup side effects |
| `src/server.js` | Start listener and graceful shutdown | Reads validated config |
| `src/config/env.js` | Parse and validate environment | Fails startup on invalid required settings |
| `src/config/services.js` | Trusted upstream registry | No request-controlled hosts |
| `src/middleware/request-id.js` | Establish request correlation | Validate or regenerate untrusted ID |
| `src/middleware/authentication.js` | Verify identity | No unverified JWT claims |
| `src/middleware/authorization.js` | Enforce route policy | Default deny |
| `src/middleware/rate-limit.js` | Quota enforcement | Explicit identity keys and failure policy |
| `src/routes` | Public route definitions | No duplicated domain logic |
| `src/clients/upstream-client.js` | Outbound service calls | Timeouts, allowlist, response limits |
| `src/policies` | Header, retry, timeout policies | Centrally tested behavior |
| `src/observability` | Structured logs and metrics | Redact sensitive values |
| `tests/unit` | Pure logic tests | No external services |
| `tests/contract` | Route/upstream contract checks | One contract per registered route |
| `tests/integration` | Mock-upstream workflows | No production endpoints |
| `tests/security` | Abuse and bypass regressions | Must run in CI |

## Repository-level files

| File | Responsibility |
|---|---|
| `.env.example` | Safe configuration names and placeholders |
| `.gitignore` | Excludes local secrets, dependencies and generated artifacts |
| `.editorconfig` | Editor conventions |
| `.gitattributes` | Line endings and binary handling |
| `package.json` | Runtime scripts and dependency metadata |
| `package-lock.json` | Exact dependency graph when generated |
| `.github/workflows` | CI, secret scan and dependency audit |
| `docs/` | API, service, architecture, security and operations guidance |

## Design rules

- Keep destination config separate from route handler logic.
- Validate configuration once during startup.
- Avoid mutable global configuration.
- Do not expose arbitrary URL forwarding.
- Authorization must run before any outbound request.
- Centralize public error mapping.
- Keep domain persistence within service owners.
- Add a file only when a real module or workflow requires it.
