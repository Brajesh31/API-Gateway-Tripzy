# 🚦 Rate limiting and abuse protection

## 1. Current status

No rate-limit middleware, quota algorithm, shared store, or concurrency controller exists. The following is a design guide, not an active quota policy.

## 2. Why quotas are required

A gateway concentrates traffic. Without quotas and concurrency limits, one client or one expensive route can exhaust gateway resources or overwhelm an upstream dependency. Limits must be aligned with service capacity and product expectations.

## 3. Quota dimensions

Possible keys include:
- Verified user subject.
- API client or partner identity.
- Tenant identifier from trusted claims.
- Route ID or route group.
- Carefully derived network identity for unauthenticated routes.

Do not key quotas on raw bearer tokens, arbitrary headers, or untrusted X-Forwarded-For values.

## 4. Policy dimensions

Every policy should define:
- Algorithm and window.
- Sustained request rate.
- Burst allowance.
- Maximum concurrent requests.
- Maximum request cost or body size where relevant.
- Quota identity key.
- Response status and error code.
- Retry/reset metadata.
- Store outage behavior.
- Alerting and owner.

## 5. Algorithm options

| Algorithm | Strength | Trade-off |
|---|---|---|
| Fixed window | Simple and efficient | Boundary bursts |
| Sliding log | Precise rolling window | Higher storage cost |
| Sliding counter | Approximate rolling behavior | More complex calculation |
| Token bucket | Controlled burst and refill | Requires clear capacity semantics |
| Leaky bucket | Smooths output rate | Can add queueing latency |

Select based on traffic shape and operational requirements, not fashion.

## 6. Request decision flow

~~~mermaid
flowchart TD
  A[Request passes identity and route checks] --> B[Resolve trusted quota key]
  B --> C[Load route policy]
  C --> D[Check request rate]
  D --> E{Rate available?}
  E -->|No| F[429 rate limited]
  E -->|Yes| G[Check concurrency budget]
  G --> H{Capacity available?}
  H -->|No| I[429 or 503 per contract]
  H -->|Yes| J[Reserve capacity]
  J --> K[Forward request]
  K --> L[Release concurrency reservation]
  L --> M[Record aggregate metrics]
~~~

## 7. Distributed state and failure policy

Multiple gateway instances require consistent quota state if quotas are expected to be global. A shared store introduces latency and a new failure dependency. Decide fail-open or fail-closed behavior by route risk. Authentication-sensitive or costly operations may require fail-closed; public low-risk reads may use a reviewed fallback.

## 8. Client response

Use 429 for a quota rejection where appropriate. Document Retry-After or reset metadata. Do not tell clients to retry immediately. Clients should use bounded exponential backoff and jitter.

## 9. Testing

Test window boundaries, bursts, concurrent requests, identity-key isolation, multi-instance behavior, store outage, clock drift, reset metadata, and attempts to spoof quota identity.

## 10. Governance

Quota values must be agreed with service owners and platform operators. Review limits after traffic growth, incidents, or service capacity changes. Avoid publishing limits that have not been measured or configured.
