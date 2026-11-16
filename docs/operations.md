# 📈 Operations, observability and incident response

## 1. Current operational state

No gateway runtime or telemetry integration exists in the repository. There is no listener, health endpoint, logger adapter, metrics exporter, trace exporter, dashboard, or on-call configuration. This guide defines the operational model required before production.

## 2. Operational objectives

Operators should be able to answer:
- Is the gateway process healthy and accepting traffic?
- Are failures global or isolated to one route/service?
- Is latency caused by gateway processing or upstream waiting?
- Are requests rejected by authentication, authorization, or quota policy?
- Are instances saturated?
- Did a deployment or configuration change precede the issue?
- Can the team safely roll back?

## 3. Signals and dimensions

| Signal | Useful dimensions | Avoid |
|---|---|---|
| Request count | Route ID, method, status class | Raw user IDs and tokens |
| Request duration | Route ID, status class | Request body values |
| Upstream duration | Service ID, route ID | Private URL/query values |
| Timeout/retry count | Service ID, safe failure class | Credentials |
| Auth denial | Route ID, safe reason code | Raw token claims |
| Quota denial | Policy ID, route group | Client secrets |
| Process saturation | CPU, memory, event loop, sockets | High-cardinality labels |
| Dependency health | Service ID, status | Internal hostnames in public output |

Never use request IDs, raw paths, arbitrary query strings, or user IDs as metric labels.

## 4. Structured logging

Recommended fields include timestamp, severity, environment, service version, route ID, method, status, duration, correlation ID, upstream service ID, and sanitized error code.

Never log authorization headers, cookies, passwords, private keys, signing material, or complete private payloads. Sanitize user-controlled values to prevent log injection.

## 5. Correlation IDs and tracing

Generate a correlation ID when absent or invalid. Bound its length and accepted characters. Replace untrusted values rather than blindly reflecting them. Propagate approved trace context to upstream services and include the correlation ID in safe error responses.

A correlation ID is diagnostic metadata, not an authentication credential. Do not use it as a metric label.

## 6. Health model

- **Liveness:** process is alive and able to make progress.
- **Readiness:** instance can accept traffic with current configuration.
- **Dependency health:** state of individual upstream services.

One unavailable upstream should not cause all gateway replicas to fail liveness. Report dependency status separately and apply route-specific degradation.

## 7. Runbook: gateway unavailable

1. Confirm alert and environment.
2. Check edge/load-balancer target health.
3. Inspect process restarts and startup configuration errors.
4. Check CPU, memory, event-loop delay, and connections.
5. Compare recent deployment/configuration changes.
6. Determine global versus route-specific failure.
7. Roll back if evidence supports a recent release as cause.
8. Record commit, artifact, config version, and sanitized correlation examples.

## 8. Runbook: upstream unavailable

1. Identify service ID and affected route IDs.
2. Check upstream health and owner status.
3. Inspect timeout, circuit, and connection-pool state.
4. Do not increase retries during an outage.
5. Preserve capacity for unrelated services.
6. Return documented 502/503/504 behavior.
7. Escalate with timestamps and redacted correlation IDs.

## 9. Runbook: authentication failures spike

Check identity-provider health, signing-key rotation, issuer/audience/clock configuration, recent changes, and aggregate safe error codes. Never disable token verification as a workaround.

## 10. Runbook: suspected compromise

Restrict affected instances, revoke exposed credentials at the provider, preserve restricted logs, inspect route/secret changes, review upstream access logs, redeploy from a trusted commit, rotate credentials, and complete incident review.

## 11. SLO planning

Agree availability and latency targets with product and service owners. Candidate indicators include successful request ratio, p95/p99 latency, upstream timeout ratio, identity-provider availability, and quota correctness. Numeric targets must come from observed baselines and business requirements.

## 12. Operational readiness checklist

- [ ] Named on-call owner and escalation map.
- [ ] Health and dependency checks implemented.
- [ ] Logs, metrics, and traces are redacted.
- [ ] Alerts have owners and actionable runbooks.
- [ ] Capacity and saturation are observable.
- [ ] Release and rollback procedures are tested.
- [ ] Upstream service contacts are documented.
- [ ] Incident review process is defined.
