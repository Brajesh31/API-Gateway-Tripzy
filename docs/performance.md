# ⚡ Performance, latency budgets and capacity

## Current status

No gateway process exists, so no throughput, latency, resource or benchmark measurements are available. This guide establishes how performance should be measured before production claims are made.

## 1. Performance objectives

Performance must be evaluated from the client's perspective and broken down by gateway processing and upstream dependency time. A low average can hide severe tail latency.

| Indicator | Definition | Why it matters |
|---|---|---|
| Throughput | Completed requests per second | Capacity under expected traffic |
| p50 latency | Median end-to-end duration | Typical user experience |
| p95 latency | 95th percentile duration | Slow-tail experience |
| p99 latency | 99th percentile duration | Worst-case tail behavior |
| Error ratio | Failed requests / total requests | Reliability under load |
| Event-loop delay | Runtime scheduling delay | Node process saturation |
| Active connections | Current in-flight sockets | Connection pool pressure |
| Upstream duration | Time spent waiting for dependency | Dependency bottleneck |
| Queue depth | Work waiting for execution | Overload and backpressure |

## 2. Latency budget model

Define one overall inbound deadline. Each downstream call receives a smaller deadline that leaves time for gateway processing, response transformation, and network delivery.

For sequential dependencies, the total budget must account for cumulative calls. For parallel dependencies, it must account for the slowest required branch plus aggregation time. Do not set every upstream timeout equal to the full client timeout.

## 3. Concurrency and backpressure

- Bound global in-flight requests.
- Bound concurrency per upstream service.
- Set explicit connection-pool limits.
- Avoid unbounded promise queues.
- Reject or shed load before memory exhaustion.
- Apply request and response body limits.
- Avoid expensive synchronous work on the Node.js event loop.
- Propagate cancellation when the client disconnects and when the request deadline expires.

## 4. Connection management

Choose a maintained HTTP client and document keep-alive behavior, DNS refresh, TLS verification, proxy settings, maximum sockets, idle timeout, and connection reuse. Never disable TLS certificate validation to work around an environment issue.

## 5. Retry policy

Retries can multiply traffic during an outage. Define retryable status/error categories, attempt limit, exponential backoff, jitter, total retry budget, and idempotency requirement.

A timeout after a mutation is ambiguous. The upstream may have committed the operation even though the gateway did not receive its response. Do not retry that operation unless the service contract provides a durable idempotency mechanism.

## 6. Circuit breakers and bulkheads

If circuit breakers are introduced, document the failure threshold, open duration, half-open probe policy, success/failure transition, and telemetry. Bulkheads should prevent one dependency from exhausting all gateway connections or workers.

## 7. Load-test methodology

Only load-test environments and services for which the team has authorization.

1. Record commit SHA, runtime version, host shape, and configuration.
2. Use synthetic data and representative request/response sizes.
3. Warm up the service and connection pools.
4. Increase concurrency gradually.
5. Record throughput, p50/p95/p99, error rate, CPU, memory, event-loop delay and connection counts.
6. Repeat runs to identify variance.
7. Test one upstream failure mode at a time.
8. Define stop conditions before starting.

## 8. Release gate

Do not claim performance readiness without repeatable results, agreed service objectives, an understood saturation point, and a tested overload/degradation strategy.
