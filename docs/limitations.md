# ⚠️ Current limitations and known gaps

## Why this document exists

A clear limitations register prevents design documentation from being mistaken for implemented product behavior. The current repository is a documentation and package-metadata foundation.

## 1. Runtime limitations

| Gap | Current evidence | Consequence |
|---|---|---|
| No server entry point | No server source file in repository tree | Nothing listens for HTTP requests |
| No HTTP framework | No runtime framework dependency | No request pipeline exists |
| No routes | No route handlers or route registry | No public endpoint is available |
| No upstream client | No proxy/HTTP client implementation | No service forwarding occurs |
| No authentication | No verifier or identity adapter | No token is verified |
| No authorization | No policy middleware | No route permission is enforced |
| No rate limiting | No middleware or quota store | No traffic quota is enforced |
| No request IDs | No middleware | No correlation propagation |
| No health endpoint | No listener or health route | No runtime health probe exists |
| No telemetry adapter | No logger/metrics/tracing integration | No gateway telemetry is emitted |

## 2. Engineering limitations

- No application test files exist.
- The current npm test script is a placeholder failure.
- No lint or formatter is configured for application source.
- No OpenAPI document describes real endpoints.
- No verified upstream contract or service owner is registered.
- No deployment manifest, container image, or runtime health check exists.
- No production SLO, capacity baseline, or benchmark exists.
- No database or shared-state design has been implemented.

## 3. Documentation limitations

The diagrams are target-state design diagrams. Example route payloads and status mappings are illustrative. They must be reconciled with actual Tripzy services before implementation.

## 4. What is required to remove these limitations

1. Obtain actual Tripzy service contracts and owners.
2. Agree the public API and identity model.
3. Implement the server and validated configuration.
4. Add explicit routes and fixed upstream registry.
5. Implement authentication, authorization and abuse controls.
6. Add tests, observability and operational runbooks.
7. Add deployment and rollback automation.
8. Validate behavior against staging services.

## 5. Reporting a new limitation

Create an issue with the affected guide or module, evidence, impact, safe reproduction, and proposed acceptance criteria. Do not publish credentials, private URLs, or customer data.
