# 🔀 Data flow and information boundaries

## 1. Purpose

This document explains which information a future gateway needs to inspect, transform, forward, or observe. The gateway runtime is not implemented. This is a design and review baseline, not a claim about current data processing.

## 2. Data flow context

~~~mermaid
flowchart LR
  Client[External client] -->|HTTP request| Gateway[Gateway boundary]
  Gateway -->|Validated route and request| Policy[Identity and route policy]
  Policy -->|Allow decision| Registry[Trusted service registry]
  Registry -->|Fixed destination| Service[Approved upstream service]
  Service -->|Response| Gateway
  Gateway -->|Filtered response| Client
  Gateway -. sanitized event .-> Logs[Restricted logs]
  Gateway -. numeric measurements .-> Metrics[Metrics]
  Gateway -. trace context .-> Traces[Tracing system]
~~~

## 3. Data categories

| Category | Examples | Processing expectation |
|---|---|---|
| Routing metadata | Method, path, route ID, service ID | Validate and use for routing |
| Authentication material | Bearer token, key ID, verified claims | Verify, minimise, never log raw token |
| Authorization context | Subject, scopes, roles, tenant context | Derive only from verified identity |
| Request payload | JSON, multipart or binary body | Enforce content and size limits |
| Response payload | Service result | Respect route schema and response limits |
| Correlation metadata | Request ID, trace context | Validate or regenerate, propagate safely |
| Operational data | Status, latency, retry count | Aggregate with bounded cardinality |
| Network metadata | Client address, forwarded chain | Trust only configured proxy hops |

## 4. Data minimisation path

1. Accept only the request fields needed by the route.
2. Resolve the route from method and path, not from a client-supplied destination.
3. Verify credentials and derive a minimal trusted identity context.
4. Evaluate route permissions without copying unnecessary claims.
5. Forward only allowlisted headers and the contract-approved body.
6. Remove or normalise internal response headers.
7. Record a sanitised event containing route ID, status class, duration and correlation ID.
8. Apply retention and access controls to logs and traces.

## 5. Data-flow levels

### Level 0, context view

External clients communicate with one gateway boundary. The gateway calls only approved service destinations and emits restricted operational telemetry.

### Level 1, processing view

The request is separated into transport metadata, credential material, request body, route policy context, and correlation metadata. Each category follows a distinct validation and retention rule.

### Level 2, sensitive identity path

~~~mermaid
flowchart TD
  A[Authorization header received] --> B[Extract bearer credential]
  B --> C[Verify against trusted key and claims policy]
  C --> D{Valid?}
  D -->|No| E[Reject and log safe reason code]
  D -->|Yes| F[Construct minimal trusted identity context]
  F --> G[Evaluate route permission]
  G --> H{Allowed?}
  H -->|No| I[Reject without forwarding]
  H -->|Yes| J[Forward only approved identity context]
  J --> K[Discard raw credential from request-local processing]
~~~

## 6. Header transformation

The gateway must not transparently forward all inbound headers. It should remove hop-by-hop headers, untrusted identity headers, unsupported forwarding metadata, and sensitive cookies unless the contract explicitly requires them. It may create a gateway-owned request ID and propagate approved trace context.

## 7. Telemetry data flow

Logs and traces should contain enough context to diagnose routing and dependency failures without duplicating private payloads. Use stable route/service identifiers. Avoid raw URLs with user-supplied query values and avoid high-cardinality labels such as request IDs or user IDs in metrics.

## 8. Data ownership

The upstream service remains authoritative for its domain records. The gateway should not persist a second copy of booking, trip, user, or payment data. Any shared state requires an ADR, data classification, retention, encryption, access-control, backup, and recovery plan.

## 9. Review checklist

- [ ] Every field forwarded has a contract-defined purpose.
- [ ] Raw tokens and secrets are never logged.
- [ ] User-controlled destination values are never used.
- [ ] Identity context is derived only after verification.
- [ ] Payload sizes and content types are bounded.
- [ ] Telemetry avoids sensitive/high-cardinality values.
- [ ] Retention and access policies are documented.
- [ ] Upstream services enforce their own resource authorization.
