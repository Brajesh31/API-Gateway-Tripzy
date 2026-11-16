# ⚠️ Error handling and response normalization

## 1. Objective

The gateway should expose stable public errors while preserving enough restricted diagnostic context for operators. It must not leak internal hostnames, stack traces, credentials, database messages, or raw upstream payloads.

No error mapper is implemented yet. The examples are proposed conventions.

## 2. Error categories

| Category | Candidate status | Public code |
|---|---:|---|
| Invalid request syntax | 400 | INVALID_REQUEST |
| Missing/invalid authentication | 401 | UNAUTHENTICATED |
| Permission denied | 403 | FORBIDDEN |
| Unknown route/resource | 404 | NOT_FOUND |
| State conflict | 409 | CONFLICT |
| Payload too large | 413 | PAYLOAD_TOO_LARGE |
| Unsupported media type | 415 | UNSUPPORTED_MEDIA_TYPE |
| Semantic validation | 422 | VALIDATION_ERROR |
| Quota exceeded | 429 | RATE_LIMITED |
| Unexpected gateway failure | 500 | INTERNAL_ERROR |
| Invalid upstream response | 502 | UPSTREAM_INVALID_RESPONSE |
| Dependency unavailable | 503 | UPSTREAM_UNAVAILABLE |
| Upstream deadline exceeded | 504 | UPSTREAM_TIMEOUT |

Final status mapping must be agreed with service owners and clients.

## 3. Proposed public envelope

~~~json
{
  "error": {
    "code": "UPSTREAM_TIMEOUT",
    "message": "The requested service did not respond in time.",
    "correlationId": "req_example_123"
  }
}
~~~

Keep error codes stable. Messages should be concise and safe. Field-level validation details may be returned only when they do not disclose sensitive information.

## 4. Failure classification flow

~~~mermaid
flowchart TD
  A[Internal failure] --> B[Classify by trusted error type]
  B --> C{Known client error?}
  C -->|Yes| D[Map to 4xx]
  C -->|No| E{Upstream failure?}
  E -->|Yes| F{Deadline exceeded?}
  F -->|Yes| G[504 UPSTREAM_TIMEOUT]
  F -->|No| H[502 or 503]
  E -->|No| I[500 INTERNAL_ERROR]
  D --> J[Build safe public envelope]
  G --> J
  H --> J
  I --> J
  J --> K[Emit redacted diagnostic event]
  K --> L[Return response with correlation ID]
~~~

## 5. Upstream error mapping

Do not pass upstream error bodies through without review. Map known service error codes to public equivalents. Unknown upstream failures should become a generic safe error and retain detailed information only in restricted logs.

## 6. Correlation and support

Every response should include a correlation identifier according to the chosen header/body contract. It must not encode user identity or secrets. Support instructions should ask for the ID and timestamp, not the original token or full payload.

## 7. Cancellation and partial failure

If the client disconnects or the overall deadline expires, cancel downstream work where possible. For fan-out operations, define whether partial results are valid, whether one failed dependency fails the whole request, and how partial status is represented. Do not invent partial-success semantics route by route.

## 8. Test requirements

Test every public error code, status, body schema, correlation ID, redaction behavior, upstream malformed body, oversized body, timeout, reset, and cancellation path.
