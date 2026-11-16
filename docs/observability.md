# 📈 Observability, logs, metrics and tracing

## Current status

No telemetry library, exporter, logger, or metrics endpoint is implemented. This document defines a future observability contract.

## 1. Three signal types

- **Logs** explain discrete events and errors.
- **Metrics** show aggregate behavior and trends.
- **Traces** show where time is spent across gateway and upstream calls.

They should share route/service context and correlation metadata without duplicating sensitive payloads.

## 2. Structured log fields

Recommended fields:
- Timestamp and severity.
- Environment and service version.
- Route ID and HTTP method.
- Public status code and safe error code.
- Request duration.
- Upstream service ID and upstream duration.
- Correlation ID and trace ID.
- Retry count and timeout classification where applicable.

Never log authorization headers, raw bearer tokens, cookies, passwords, private keys, signing material, or full private request/response bodies.

## 3. Metrics catalogue

| Metric concept | Type | Labels |
|---|---|---|
| Gateway requests | Counter | route ID, method, status class |
| Gateway duration | Histogram | route ID, status class |
| Upstream duration | Histogram | service ID, route ID |
| Upstream failures | Counter | service ID, safe failure class |
| Timeouts | Counter | service ID, route ID |
| Retries | Counter | service ID, route ID |
| Rate-limit denials | Counter | policy ID, route group |
| Authentication denials | Counter | safe reason class, route ID |
| Active requests | Gauge | service ID or bounded route group |
| Process resources | Gauge | instance and runtime |

Do not use user IDs, request IDs, raw URLs, arbitrary query strings, or tokens as metric labels. High-cardinality labels can make the telemetry system expensive and unstable.

## 4. Trace propagation

Use a documented trace-context standard. Validate incoming trace headers and avoid blindly forwarding arbitrary baggage. A gateway span should include the route ID and approved upstream service ID. Avoid attaching request bodies or credential claims to spans.

## 5. Sampling and retention

Define trace sampling by environment and traffic profile. Security events may require separate audit retention. Document retention periods, access roles, export destinations, and deletion behavior before enabling a vendor.

## 6. Alert design

Candidate alerts:
- Elevated gateway 5xx ratio.
- Sustained p95/p99 latency regression.
- Upstream timeout spike for one service.
- Authentication rejection spike.
- Rate-limit store unavailable.
- Process saturation or repeated restarts.
- Readiness failures across multiple instances.

Set numeric thresholds only after baseline measurement and owner approval.

## 7. Operator questions

A useful dashboard should answer:
1. Is the gateway receiving traffic?
2. Are failures global or isolated to a service/route?
3. Is latency in gateway processing or upstream waiting?
4. Are failures caused by auth, quota, timeout, or upstream errors?
5. Are instances saturated?
6. Did a deployment or configuration change precede the incident?
