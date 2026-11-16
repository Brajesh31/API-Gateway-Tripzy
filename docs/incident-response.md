# 🚨 Incident response playbooks

## Current status

No production gateway is deployed from this repository. This document defines a response framework for the future service. Operators must adapt contact paths, alert thresholds, and escalation owners to the actual deployment.

## 1. Incident roles

| Role | Responsibility |
|---|---|
| Incident lead | Coordinates response and prioritisation |
| Gateway operator | Investigates gateway process, configuration and telemetry |
| Service owner | Investigates affected upstream behavior |
| Security responder | Handles suspected compromise and data exposure |
| Communications owner | Shares approved status updates |

One person may hold multiple roles in a small team, but responsibilities should remain explicit.

## 2. Gateway unavailable

1. Confirm the alert and affected environment.
2. Check edge/load-balancer health and target registration.
3. Inspect process restarts, startup failures, and configuration validation.
4. Check CPU, memory, event-loop delay, open sockets, and connection saturation.
5. Compare the incident start with the latest deployment/configuration change.
6. Determine whether the outage is global or route-specific.
7. Roll back only when evidence supports a recent release as the cause.
8. Record incident timeline, commit, artifact, config version, and redacted correlation examples.

## 3. Upstream service unavailable

1. Identify the service ID and affected route IDs.
2. Check the owning service's status and escalation channel.
3. Inspect gateway timeout and circuit state.
4. Do not increase retries during an outage.
5. Preserve capacity for unaffected services.
6. Return the documented 502/503/504 behavior.
7. Escalate with timestamps, status codes, and redacted correlation IDs.

## 4. Authentication failure spike

1. Check identity provider availability.
2. Verify signing-key rotation and JWKS cache behavior.
3. Check issuer, audience, algorithm, expiry, and clock configuration.
4. Compare the failure pattern by route and environment.
5. Review recent identity or gateway changes.
6. Never disable token verification as a workaround.

## 5. Suspected credential compromise

1. Restrict affected instances or route access where appropriate.
2. Revoke or rotate the exposed credential at its issuer/provider.
3. Preserve restricted evidence and access logs.
4. Identify routes, services, and time windows potentially affected.
5. Inspect upstream access records for misuse.
6. Redeploy from a trusted commit if integrity is in doubt.
7. Confirm secret rotation and least-privilege access.
8. Complete a post-incident review.

## 6. Data exposure

- Stop ongoing exposure where safe.
- Preserve evidence with restricted access.
- Determine data categories and affected routes.
- Involve service/data owners and the approved privacy/legal response path.
- Follow applicable notification obligations as determined by responsible parties.
- Do not publish personal or confidential incident data in public issues.

## 7. Incident record template

~~~text
Incident ID:
Environment:
Start and end time:
Severity:
Incident lead:
Affected route IDs:
Affected service IDs:
Customer impact:
Detection source:
Timeline:
Mitigation:
Root cause:
Follow-up actions:
Related commit/deployment:
~~~

## 8. Post-incident review

Focus on system conditions and corrective actions, not blame. Record what happened, how it was detected, what worked, what delayed response, and measurable actions with owners and due dates.
