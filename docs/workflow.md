# 🔄 Application and engineering workflows

> This guide documents intended workflows. The gateway runtime is not implemented, so diagrams describe target behavior rather than observations from a live service.

## 1. End-to-end request workflow

~~~mermaid
flowchart TD
  A([Client sends request]) --> B[Edge terminates HTTPS]
  B --> C[Gateway validates framing and size]
  C --> D{Valid request?}
  D -->|No| E[Return 400 or 413]
  D -->|Yes| F[Resolve registered route]
  F --> G{Route exists?}
  G -->|No| H[Return 404]
  G -->|Yes| I[Verify identity where required]
  I --> J{Identity valid?}
  J -->|No| K[Return 401]
  J -->|Yes| L[Check permission and quota]
  L --> M{Allowed?}
  M -->|No| N[Return 403 or 429]
  M -->|Yes| O[Call fixed upstream]
  O --> P[Normalize response]
  P --> Q[Emit redacted telemetry]
  Q --> R([Return response])
~~~

## 2. Route registration workflow

1. A service owner submits a route contract.
2. Gateway maintainers verify the public method/path and intended consumer.
3. Security review confirms authentication, authorization, destination, and header rules.
4. Request and response schemas are reviewed.
5. Timeout, retry, idempotency, rate and concurrency budgets are agreed.
6. Contract tests and negative security tests are added.
7. The route is merged behind the appropriate environment configuration.
8. Staging verifies the real service contract.
9. Production exposure follows the release and rollback policy.

~~~mermaid
stateDiagram-v2
  [*] --> Draft
  Draft --> ServiceOwnerReview
  ServiceOwnerReview --> SecurityReview
  SecurityReview --> ContractTests
  ContractTests --> Approved
  ContractTests --> ChangesRequested
  ChangesRequested --> Draft
  Approved --> Staging
  Staging --> Production
  Production --> Deprecated
  Deprecated --> Retired
  Retired --> [*]
~~~

## 3. Development workflow

- Start from an issue with a clear problem and acceptance criteria.
- Inspect the current repository and relevant service contract.
- Write or update an ADR when the change alters an architectural constraint.
- Implement the smallest focused change.
- Add unit, contract, integration, and security tests as appropriate.
- Update diagrams, route reference, configuration docs, and threat model.
- Run local checks and inspect the final diff.
- Submit a pull request with test evidence and rollout notes.

## 4. CI workflow

~~~mermaid
flowchart TD
  A[Push or pull request] --> B[Checkout]
  B --> C[Validate package metadata]
  C --> D[Check source syntax when source exists]
  D --> E[Validate Markdown and internal references]
  E --> F[Run tests when actual test files exist]
  F --> G[Dependency and secret checks]
  G --> H{Required jobs pass?}
  H -->|No| I[Block merge and investigate]
  H -->|Yes| J[Eligible for review and merge]
~~~

CI must not claim that a runtime build or test suite passed when the repository has no runtime source or tests. The workflow should report skipped stages explicitly.

## 5. Deployment workflow

A deployment requires a real server, verified environment configuration, upstream contracts, health endpoints, telemetry, and rollback artifact. The release sequence is documented in [deployment](./deployment.md).

## 6. Failure and retry workflow

When an upstream call fails, classify the failure before deciding whether to retry. A network timeout after a mutation is ambiguous: the service may have completed the action. Only retry when the route contract and idempotency mechanism make the operation safe. Record a safe error code and correlation ID, not the raw credential or private payload.

## 7. Workflow ownership

| Workflow | Responsible role |
|---|---|
| Route contract | Gateway maintainer and upstream service owner |
| Identity contract | Identity provider owner and security reviewer |
| Quota policy | Platform operator and service owner |
| CI and release | Repository maintainer |
| Incident response | On-call operator and affected service owner |
| API deprecation | Gateway maintainer and client/service stakeholders |
