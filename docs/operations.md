# 📈 Operations, observability and incident response

## Current state
No runtime or telemetry integration is implemented. The requirements below must be completed before production traffic.

## Signals to collect
| Signal | Useful dimensions | Exclude |
|---|---|---|
| Request count | Route ID, method, status class | Raw tokens and IDs |
| Request duration | Route ID, status class | Payload content |
| Upstream duration | Service ID, route ID | Private URL/query values |
| Timeout/retry count | Service ID, route ID | Credentials |
| Rate denials | Policy ID, client category | Raw access token |
| Auth denials | Safe reason code, route ID | Token claims unless reviewed |
| Process saturation | CPU, memory, event loop, connections | High-cardinality labels |
| Dependency health | Service ID, status | Internal hostnames in public output |

Do not use user IDs, raw request paths, request IDs, or token values as high-cardinality metric labels.

## Structured logs
Recommended safe fields: timestamp, severity, environment, service/version, route ID, method, status, duration, correlation ID, upstream service ID, and sanitized error code. Never log authorization headers, cookies, passwords, private keys, or complete request/response bodies.

## Correlation IDs
Generate an ID when absent or invalid, validate length/characters, replace untrusted values, propagate only to approved upstreams, and include it in safe error responses. Do not use request IDs as metric labels.

## SLO planning
Define availability and latency targets with product/platform owners. Candidate indicators include successful request ratio, p95/p99 latency, upstream timeout ratio, authentication dependency availability, and quota correctness. This repository does not invent numeric SLO targets.

## Runbook: gateway unavailable
1. Check edge/load-balancer target health.
2. Check process status and restart history.
3. Inspect startup configuration errors.
4. Check saturation and connection limits.
5. Compare last deployment/config change.
6. Determine whether failure is global or route-specific.
7. Roll back when evidence supports the latest change as cause.
8. Record commit, deployment ID and redacted correlation examples.

## Runbook: upstream unavailable
1. Identify service ID and affected route IDs.
2. Check upstream health and owning team's incident status.
3. Inspect timeout/circuit state.
4. Do not increase retries during an outage.
5. Preserve availability for unrelated services.
6. Return documented 502/503/504 behavior.
7. Escalate with timestamps and redacted correlation IDs.

## Runbook: authentication failures spike
Check identity provider health, signing-key rotation, issuer/audience/clock configuration, recent changes, and aggregate error codes. Never disable token verification as a workaround.

## Runbook: suspected compromise
Restrict affected instances, revoke exposed credentials at their issuer/provider, preserve restricted logs, inspect route/secret changes, check upstream access logs, redeploy from a trusted commit, rotate credentials, and complete incident review.

## Operational readiness
Before production, name an on-call owner, escalation map, incident channel, alert thresholds, maintenance process, and post-incident review policy.
