# ADR 0001: Gateway boundary and service ownership

- **Status:** Proposed
- **Date:** 2026-11-16
- **Decision owners:** Repository maintainer and affected Tripzy service owners

## Context

The repository is intended to become a gateway for a Tripzy service ecosystem, but the actual service list, public contracts, identity provider, and deployment topology have not yet been supplied or verified.

A gateway can reduce client coupling and centralise cross-cutting controls. It can also become a dangerous concentration point if it accepts arbitrary destinations, duplicates domain logic, or becomes a shadow database.

## Decision

Adopt the following proposed principles as the baseline for future implementation:

1. The gateway exposes only reviewed public routes.
2. Each route maps to a fixed service identifier and approved internal path.
3. The gateway owns transport-level concerns such as route resolution, request limits, coarse route authorization, deadlines, and public error mapping.
4. Domain services own business rules, domain persistence, and resource-level authorization.
5. The gateway does not accept arbitrary client-provided target URLs.
6. The gateway remains stateless unless a specific shared-state requirement is approved separately.
7. Each route requires a service owner, contract, tests, and operational expectations.

This ADR remains Proposed until the relevant service owners confirm the model and the implementation is accepted.

## Alternatives considered

### A. Clients call every service directly

**Advantages:** fewer gateway components and no central proxy hop.

**Disadvantages:** clients learn internal topology and duplicate cross-cutting integration behavior.

### B. Gateway becomes a domain orchestration service

**Advantages:** complex workflows can be composed centrally.

**Disadvantages:** business logic and data ownership become coupled to the gateway, making service evolution harder.

### C. Generic open proxy

**Advantages:** flexible forwarding with little route configuration.

**Disadvantages:** unacceptable SSRF, access-control, and observability risk. Rejected.

## Consequences

### Positive

- Explicit service ownership.
- Smaller public attack surface.
- Consistent route-level policy.
- Clear separation between transport and domain behavior.
- Testable route contracts.

### Negative and trade-offs

- Every route requires contract governance.
- Gateway availability becomes important for client traffic.
- A central policy layer adds latency and operational complexity.
- Identity and quota integrations need clear ownership.

## Follow-up work

- Confirm real Tripzy service names and owners.
- Obtain OpenAPI or equivalent contracts.
- Agree identity and authorization model.
- Select runtime framework and deployment topology.
- Implement route registry and negative security tests.
- Revisit this ADR when evidence or requirements change.
