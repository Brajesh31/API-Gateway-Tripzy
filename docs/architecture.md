# 🏗️ Architecture and system boundaries

> **Status:** Proposed architecture. The current repository has no gateway server, route registry, authentication middleware, or upstream client.

## 1. Architectural intent

API Gateway Tripzy is intended to provide a single governed HTTP boundary for client applications and partner integrations. It should centralise transport-level concerns while keeping domain behavior and persistence inside the services that own them.

The architecture must be based on real Tripzy service contracts. No service names, internal hostnames, routes, or identity provider are confirmed in this repository.

## 2. Logical architecture

~~~mermaid
flowchart TB
  Client[Web, mobile and partner clients]
  Edge[HTTPS edge and load balancer]
  Gateway[Gateway application]
  Context[Request context]
  Identity[Identity verification]
  Policy[Authorization and quota]
  Registry[Trusted route registry]
  Client --> Edge --> Gateway
  Gateway --> Context --> Identity --> Policy --> Registry
  Registry --> ServiceA[Approved upstream A]
  Registry --> ServiceB[Approved upstream B]
  Registry --> ServiceC[Approved upstream C]
  Gateway --> Telemetry[Logs, metrics and traces]
  Config[Validated deployment config] --> Gateway
  Config --> Registry
~~~

The service nodes are placeholders. The actual upstream inventory must be supplied by service owners.

## 3. Component responsibilities

| Component | Responsibility | Security/reliability requirement |
|---|---|---|
| Edge | TLS and connection controls | Trusted proxy chain and request framing consistency |
| Gateway listener | Accept HTTP requests | Connection and body limits |
| Request context | Correlation and timing | Validate or regenerate client-provided IDs |
| Identity adapter | Verify caller credentials | Cryptographic validation and fail-closed policy |
| Authorization policy | Evaluate route permission | Default deny and no client-controlled claims |
| Route registry | Resolve fixed destination | No arbitrary client-selected URL |
| Quota policy | Enforce rate/concurrency limits | Explicit identity key and store failure policy |
| Upstream client | Forward approved request | Deadline, size, header, redirect controls |
| Error mapper | Stable public response | No internal details |
| Telemetry | Operational visibility | Redaction and bounded cardinality |

## 4. Request processing order

1. Edge accepts HTTPS and applies connection policy.
2. Gateway validates request framing, method, path, content type, and size.
3. Gateway creates or validates correlation metadata.
4. Route resolution finds an exact public method/path mapping.
5. Authentication verifies the credential when the route requires it.
6. Authorization evaluates the required permission.
7. Rate and concurrency policies are applied.
8. A fixed destination is resolved from trusted configuration.
9. The outbound request is filtered and sent with a deadline.
10. The response or failure is normalized and recorded as safe telemetry.

Unknown routes must not reach a generic forwarder. A denied request must not trigger an upstream call.

## 5. Trust boundaries

| Boundary | Threat | Control |
|---|---|---|
| Client → edge | Malformed request, abuse | TLS, parser consistency, size and connection limits |
| Edge → gateway | Spoofed forwarded metadata | Explicit trusted proxy configuration |
| Gateway → identity | Token/key compromise | Trusted issuer, signature and claim validation |
| Gateway → upstream | SSRF, header spoofing | Fixed service registry and filtered headers |
| Gateway → telemetry | Sensitive data leakage | Redaction, retention and access controls |
| CI → deployment | Supply-chain compromise | Least privilege, reviewed workflows and immutable artifact |

## 6. Failure mapping

| Failure | Candidate response | Isolation expectation |
|---|---:|---|
| Unknown public route | 404 | No upstream call |
| Invalid credential | 401 | Reject protected request |
| Missing permission | 403 | No upstream call |
| Quota exceeded | 429 | Reject before forwarding |
| Upstream invalid response | 502 | Isolate affected service |
| Upstream unavailable | 503 | Preserve unrelated routes |
| Upstream timeout | 504 | Cancel/stop work within deadline |
| Internal gateway error | 500 | Safe public error, restricted diagnostics |

Exact behavior must be approved in the API contract.

## 7. Stateless deployment model

Multiple gateway replicas behind an edge/load balancer are a possible target. Statelessness improves horizontal scaling and reduces the risk of gateway persistence becoming a second source of truth. Shared quota/idempotency state, if required, must be designed explicitly.

## 8. Architecture diagrams

Editable sources live in [diagrams](./diagrams/). The architecture guide should remain aligned with the route register, security guide, data-flow guide, and implementation.

## 9. Decisions still open

- HTTP framework and server lifecycle.
- Identity provider and token contract.
- Upstream service inventory and network boundaries.
- Rate-limit store and fail-open/fail-closed policy.
- Public API versioning.
- Telemetry backend and retention.
- Hosting and deployment topology.

Record material choices in [ADRs](./ADRs/README.md).
