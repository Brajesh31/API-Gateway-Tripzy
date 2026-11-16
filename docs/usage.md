# 📘 Usage guide and request patterns

> The repository has no running server or public routes. This guide describes how to work with the current scaffold and provides illustrative request patterns for future implementation. The examples are not callable endpoints.

## 1. Current repository usage

Clone and install package metadata:

~~~bash
git clone https://github.com/Brajesh31/API-Gateway-Tripzy.git
cd API-Gateway-Tripzy
npm install
~~~

There is currently no server startup command. The package test script is a deliberate placeholder that exits with an explanatory message.

## 2. How a future client should address the gateway

Clients should use a single configured public gateway base URL supplied by their environment. Do not hard-code internal microservice hostnames in a browser or mobile application.

A generic future request shape might look like:

~~~http
GET /api/v1/example-resource HTTP/1.1
Host: gateway.example.invalid
Accept: application/json
Authorization: Bearer <access-token>
X-Request-ID: client-generated-correlation-value
~~~

The reserved example domain is illustrative. It is not a Tripzy deployment URL.

## 3. Illustrative success response

~~~json
{
  "data": {
    "id": "example-resource-001",
    "status": "available"
  },
  "meta": {
    "correlationId": "req_example_123"
  }
}
~~~

The exact envelope must be decided with service owners. Do not introduce a wrapper that breaks existing clients without a versioning plan.

## 4. Illustrative error response

~~~json
{
  "error": {
    "code": "UPSTREAM_TIMEOUT",
    "message": "The requested service did not respond in time.",
    "correlationId": "req_example_123"
  }
}
~~~

Public messages should be actionable but must not reveal private hostnames, stack traces, credentials, database errors, or raw upstream payloads.

## 5. Client integration rules

- Set a reasonable client timeout below the full server-side deadline.
- Treat 401 as a credential problem and 403 as an authorization problem.
- Respect 429 retry guidance rather than creating an immediate retry loop.
- Do not automatically replay non-idempotent operations.
- Send idempotency keys only where the route contract defines them.
- Preserve correlation IDs in support reports.
- Avoid logging authorization headers or personal payloads in client telemetry.

## 6. Route-specific usage documentation

When routes are implemented, add a route reference with method, path, authentication, permissions, request schema, response schema, examples, status codes, quota, timeout, and owner. See [API reference](./api.md) and [service contracts](./service-contracts.md).
