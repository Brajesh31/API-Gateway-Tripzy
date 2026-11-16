# 🧩 HTTP headers, proxy trust and forwarding policy

## 1. Current status

There is no HTTP listener or proxy middleware in the repository. This guide defines the requirements for a future implementation.

## 2. Header handling principles

Never forward every inbound header by default. Treat headers as untrusted input unless they are set by a known trusted proxy or created by the gateway after validation.

## 3. Hop-by-hop headers

Remove connection-specific hop-by-hop headers and any headers named by the Connection header. Ensure the selected HTTP client and framework handle protocol-specific hop-by-hop behavior correctly.

## 4. Forwarded client metadata

Only trust forwarded client address and protocol metadata from explicitly configured proxy hops. Do not trust arbitrary X-Forwarded-For or Forwarded values from the public internet. Document the exact edge-to-gateway topology and trusted proxy count.

## 5. Identity headers

Strip client-supplied values such as X-User-Id, X-Role, X-Tenant-Id, X-Permissions, or equivalent internal identity metadata. Construct trusted context only after credential verification. Forward only the minimum claims needed by the service contract.

## 6. Cookie handling

Do not forward browser cookies automatically. If a route requires cookies, document cookie names, scope, security attributes, SameSite behavior, and whether the gateway or service owns the session. Avoid logging cookie values.

## 7. Host and destination safety

The Host header must not determine the upstream destination. Route resolution should use the registered public method/path and a fixed service identifier. Validate destination configuration independently.

## 8. Trace and correlation headers

Accept only documented trace-context headers, validate their syntax and size, and avoid blindly propagating arbitrary baggage. Generate a gateway correlation ID when missing or invalid. Do not use correlation IDs as authentication.

## 9. Response header filtering

Remove internal server/version headers, private diagnostics, hop-by-hop headers, and cookies not intended for clients. Forward cache, security, and content headers only according to the public route contract.

## 10. Review checklist

- [ ] Hop-by-hop headers removed.
- [ ] Forwarded metadata trusted only from known proxy hops.
- [ ] Client identity headers stripped.
- [ ] Host cannot select destination.
- [ ] Cookies are explicitly handled.
- [ ] Trace context is validated.
- [ ] Response headers are filtered.
- [ ] Header count and size are bounded.
