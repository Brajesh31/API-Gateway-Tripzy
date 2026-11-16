# 🧾 Route contract template

Copy this document for each proposed public route. Replace every placeholder with verified information. Do not merge a route with unknown service ownership or destination.

## Route identity

| Field | Value |
|---|---|
| Route ID | TBD |
| Public name | TBD |
| Owning service | TBD |
| Service owner/team | TBD |
| Contract reviewer | TBD |
| Status | Proposed |

## Public interface

| Field | Value |
|---|---|
| HTTP method | TBD |
| Public path/version | TBD |
| Content type | TBD |
| Authentication mode | TBD |
| Required permissions | TBD |
| Rate-limit policy | TBD |

## Upstream mapping

| Field | Value |
|---|---|
| Service ID | TBD |
| Environment destination key | TBD |
| Internal method/path | TBD |
| Network boundary | TBD |
| Service identity | TBD |

Do not place a production secret or arbitrary URL in this document.

## Request and response

- Path parameters:
- Query parameters:
- Required headers:
- Request schema:
- Request size limit:
- Success response schema:
- Response size/streaming policy:
- Public error codes:

## Resilience

- Overall deadline:
- Upstream timeout:
- Retryable failures:
- Maximum attempts:
- Idempotency mechanism:
- Concurrency limit:
- Circuit behavior:
- Client cancellation behavior:

## Security and privacy

- Identity claims consumed:
- Resource-level checks in service:
- Header allowlist:
- Sensitive data classification:
- Logging exclusions:
- SSRF and redirect review:
- Abuse/rate-limit review:

## Verification

- Unit tests:
- Contract tests:
- Integration tests:
- Security regression tests:
- Staging verification:
- Monitoring and alert:
- Rollback plan:

## Approval

- [ ] Service owner approved.
- [ ] Gateway maintainer approved.
- [ ] Security review completed.
- [ ] Contract tests pass.
- [ ] Documentation and diagrams updated.
