# 🧾 Architecture Decision Records

Architecture Decision Records (ADRs) capture important technical choices, the context that led to them, alternatives considered, and the consequences of the decision.

## Why use ADRs?

Gateway decisions have long-lived effects. Choosing an identity provider, route registry, retry policy, shared state store, HTTP framework, or public versioning scheme changes the security and operational model. Recording the rationale prevents decisions from becoming hidden assumptions.

## Status vocabulary

- **Proposed:** under discussion, not approved.
- **Accepted:** approved for implementation.
- **Rejected:** considered and not selected.
- **Superseded:** replaced by a later decision.
- **Deprecated:** no longer recommended, but retained for historical context.

## ADR index

| ID | Decision | Status |
|---|---|---|
| [0001](./0001-gateway-boundary-and-service-ownership.md) | Gateway boundary and service ownership | Proposed |
| 0002 | HTTP framework and runtime composition | Not written |
| 0003 | Identity provider and token verification | Not written |
| 0004 | Route registry and upstream allowlisting | Not written |
| 0005 | Rate-limit state and failure policy | Not written |
| 0006 | Observability and telemetry pipeline | Not written |

## Creating an ADR

1. Copy [the ADR template](./template.md).
2. Assign the next sequential four-digit identifier.
3. Describe verified context separately from assumptions.
4. Compare meaningful alternatives and trade-offs.
5. Include security, privacy, operational, and migration consequences.
6. Obtain required service-owner and security review.
7. Link implementation and tests after they exist.

Keep one decision per file. Do not silently rewrite accepted historical decisions. Create a new ADR when a decision changes.

## Review expectations

Security-sensitive decisions require security review. Decisions affecting an upstream contract require service-owner review. Accepted ADRs should link to implementation and tests when those exist.
