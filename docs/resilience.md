# 🛟 Resilience, timeouts, retries and dependency isolation

## 1. Current status

No outbound HTTP client, timeout policy, retry logic, circuit breaker, or connection pool exists. This guide describes future resilience requirements.

## 2. Deadline model

Each request should have one overall deadline. Each upstream call receives a smaller budget that leaves time for gateway processing and response delivery. Propagate deadlines where the upstream contract supports them.

Do not allow nested services to each consume the full original timeout. That causes end-to-end latency to exceed the caller's expectations.

## 3. Timeout categories

| Timeout | Purpose |
|---|---|
| Connection timeout | Bound connection establishment |
| TLS handshake timeout | Bound secure connection setup |
| Response/header timeout | Bound time to first response |
| Idle timeout | Bound inactivity during streaming |
| Overall request deadline | Bound total gateway work |
| Upstream service deadline | Service-specific budget |

Timeout values must be based on service expectations and measured latency, not invented as universal constants.

## 4. Retry policy

Retries require an explicit decision per route:
- Which failures are retryable?
- Which HTTP methods are safe?
- Does the service guarantee idempotency?
- Is an idempotency key supported?
- What is the maximum attempt count?
- What backoff and jitter are used?
- What is the total retry time budget?
- How are client cancellation and request deadlines handled?

Never automatically retry a mutation when its outcome is uncertain.

## 5. Circuit breakers

A circuit breaker may stop calls to an unhealthy dependency. Define error-rate/latency thresholds, open duration, half-open probes, recovery criteria, and alerting. Avoid global breakers that couple unrelated services.

## 6. Bulkheads and concurrency

Limit active requests globally and per upstream. Use bounded connection pools and queues. Reject work before memory or socket exhaustion. A slow upstream should not consume all resources required by healthy services.

## 7. Cancellation

Propagate client disconnect and deadline cancellation to upstream calls. Ensure cleanup releases connection, concurrency, and quota reservations even when a request fails.

## 8. Failure isolation flow

~~~mermaid
flowchart TD
  A[Request scheduled for upstream] --> B[Acquire service concurrency slot]
  B --> C{Slot available?}
  C -->|No| D[Reject or queue within bounded policy]
  C -->|Yes| E[Start request deadline]
  E --> F[Call approved upstream]
  F --> G{Completed before deadline?}
  G -->|Yes| H[Map response]
  G -->|No| I[Cancel and classify timeout]
  I --> J{Retry safe and budget remains?}
  J -->|Yes| K[Backoff with jitter]
  K --> F
  J -->|No| L[Return mapped failure]
  H --> M[Release resources]
  L --> M
~~~

## 9. Verification

Fault-inject connection refusal, DNS failure, slow headers, slow body, connection reset, malformed response, oversized response, client disconnect, and dependency recovery. Confirm that one dependency cannot exhaust unrelated service capacity.
