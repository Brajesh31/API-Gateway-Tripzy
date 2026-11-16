# 🧱 Technical debt and implementation gap register

## Purpose

This register tracks known engineering gaps based on the repository tree inspected on the current branch. It is intentionally direct. Documentation is not a substitute for implementation.

## Current gap register

| ID | Priority | Gap | Evidence | Impact | Exit criteria |
|---|---|---|---|---|---|
| TD-001 | P0 | No gateway server | No server source file | No requests can be served | Server starts and exposes tested health route |
| TD-002 | P0 | No real upstream contracts | Service registry absent | Destinations cannot be safely routed | Owner-approved contract per route |
| TD-003 | P0 | No authentication | No verifier or provider adapter | Caller identity cannot be trusted | Cryptographic verification and negative tests |
| TD-004 | P0 | No authorization | No policy middleware | Protected routes cannot be enforced | Default-deny policy and tests |
| TD-005 | P0 | No destination allowlist | No outbound client/registry | No SSRF-safe forwarding path | Fixed destinations and SSRF regression tests |
| TD-006 | P1 | No automated test suite | No test files | Runtime changes cannot be verified | Unit, contract, integration and security tests |
| TD-007 | P1 | No observability | No telemetry adapter | Production diagnosis unavailable | Redacted logs, metrics and traces |
| TD-008 | P1 | No deployment setup | No deployment manifests | No repeatable runtime release | Staging, production and rollback documented |
| TD-009 | P1 | No API schema | No OpenAPI contract | Client integration cannot be validated | Versioned OpenAPI for approved routes |
| TD-010 | P2 | No SLO baseline | No production measurements | Reliability targets unknown | Owner-approved SLI/SLO and alert policy |

## Prioritisation

P0 items block internet-facing production use. P1 items block safe operations and controlled delivery. P2 items improve long-term reliability and maintainability.

## Debt acceptance

A temporary gap requires a named owner, rationale, risk, compensating control, and review date. Security-critical controls should not be waived simply to ship a route.

## Review cadence

Review this register when runtime implementation begins, during release readiness, and after incidents or security assessments. Close an item only with code/test evidence and updated documentation.
