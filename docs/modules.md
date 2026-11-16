# 🧩 Module responsibilities and dependency boundaries

> Proposed source layout only. No application modules exist in the current repository tree.

## 1. Why boundaries matter

Gateway middleware often becomes a long chain of loosely related checks. Clear module boundaries make security order, testability, and failure behavior easier to review. Each module should have one reason to change and should expose a small interface.

## 2. Proposed module map

~~~mermaid
flowchart TD
  Server[server entry point] --> App[application composition]
  App --> Context[request context middleware]
  App --> Resolver[route resolver]
  Resolver --> Auth[authentication adapter]
  Auth --> Authorization[authorization policy]
  Authorization --> Quota[rate and concurrency policy]
  Quota --> Upstream[upstream client]
  Upstream --> Mapper[error mapper]
  App --> Logger[structured logger]
  App --> Metrics[metrics and tracing adapter]
  Config[validated configuration] --> App
  Config --> Resolver
  Config --> Upstream
~~~

## 3. Module contracts

| Proposed module | Inputs | Outputs | Must not do |
|---|---|---|---|
| Server entry point | Validated runtime config | Listener lifecycle | Contain route business logic |
| Application composition | Middleware, routes, adapters | Composed HTTP application | Open sockets on import |
| Environment config | Process environment | Validated immutable config | Log secret values |
| Route registry | Reviewed route definitions | Validated route registry | Read destination from request |
| Request context | Inbound request metadata | Correlation and timing context | Trust arbitrary identity claims |
| Authentication | Credential and trusted key adapter | Verified principal | Decode token and trust claims |
| Authorization | Principal and route policy | Allow/deny decision | Replace domain resource checks |
| Rate limiter | Policy key and quota store | Allow/deny plus retry metadata | Use raw token as key |
| Upstream client | Service ID and approved request | Bounded upstream response | Accept arbitrary URL |
| Header policy | Inbound/outbound headers | Filtered header set | Forward hop-by-hop headers |
| Retry policy | Method, idempotency contract, failure | Retry decision | Retry unsafe mutations by default |
| Error mapper | Internal failure category | Public error envelope | Return stack traces |
| Structured logger | Sanitised event | Structured log | Log tokens or payloads |
| Metrics adapter | Bounded measurements | Metrics and traces | Add unbounded labels |

## 4. Dependency direction

Core policies should depend on interfaces rather than specific vendors. The HTTP framework, identity provider, quota store, and telemetry vendor should be adapters at the boundary. Avoid circular imports between route definitions, middleware, and clients.

## 5. Startup and shutdown lifecycle

~~~mermaid
stateDiagram-v2
  [*] --> LoadingConfiguration
  LoadingConfiguration --> ValidatingConfiguration
  ValidatingConfiguration --> BuildingApplication
  ValidatingConfiguration --> StartupFailed
  BuildingApplication --> BindingListener
  BindingListener --> Ready
  Ready --> Draining
  Draining --> ClosingDependencies
  ClosingDependencies --> Stopped
  StartupFailed --> [*]
  Stopped --> [*]
~~~

Startup should fail if security-critical configuration or route definitions are invalid. Shutdown should stop accepting new work, allow bounded in-flight completion, close outbound clients and stores, then exit.

## 6. Testing boundaries

- Unit-test route matching, config parsing, header policies, retry decisions, and error mapping without network access.
- Middleware-test authentication and authorization with deterministic fake identity adapters.
- Contract-test each service adapter against a mock service.
- Integration-test the composed app with local HTTP servers.
- Security-test SSRF, header spoofing, redirects, payload limits, and permission bypass.

## 7. Naming and file conventions

Use lowercase kebab-case for multiword filenames, one module per file where practical, and explicit .js extensions for Node.js ESM imports. Keep pure policy functions separate from framework-specific request/response adapters.
