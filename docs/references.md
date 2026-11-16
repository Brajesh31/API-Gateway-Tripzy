# 📚 Technical references and standards

This list contains authoritative background material for implementation decisions. A reference is not evidence that a standard or control has already been implemented.

## HTTP and API design

- [RFC 9110: HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)
- [RFC 9112: HTTP/1.1](https://www.rfc-editor.org/rfc/rfc9112)
- [RFC 9457: Problem Details for HTTP APIs](https://www.rfc-editor.org/rfc/rfc9457)
- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

Use HTTP semantics consistently and define a stable error contract. If adopting RFC 9457, document any extension members and compatibility requirements.

## Identity and security

- [RFC 7519: JSON Web Token](https://www.rfc-editor.org/rfc/rfc7519)
- [RFC 8725: JSON Web Token Best Current Practices](https://www.rfc-editor.org/rfc/rfc8725)
- [RFC 7517: JSON Web Key](https://www.rfc-editor.org/rfc/rfc7517)
- [OWASP API Security Top 10](https://owasp.org/API-Security/)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)

JWT implementation must follow explicit algorithm and claim validation rules. Never trust a token merely because it can be decoded.

## Runtime and platform

- [Node.js documentation](https://nodejs.org/docs/latest/api/)
- [Node.js release schedule](https://github.com/nodejs/release)
- [npm documentation](https://docs.npmjs.com/)
- [GitHub Actions documentation](https://docs.github.com/actions)

Choose a supported Node.js LTS line for the deployment lifecycle and verify framework compatibility before adding dependencies.

## Observability

- [OpenTelemetry documentation](https://opentelemetry.io/docs/)
- [W3C Trace Context](https://www.w3.org/TR/trace-context/)
- [Prometheus documentation](https://prometheus.io/docs/introduction/overview/)

Telemetry must be designed with privacy, cardinality, retention, and access control in mind.

## Accessibility and privacy

- [W3C Web Content Accessibility Guidelines](https://www.w3.org/WAI/standards-guidelines/wcag/)
- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)

## Repository-specific implementation rule

When adopting a standard, record the exact version, implementation scope, exceptions, and test evidence in the relevant guide or ADR. Avoid claiming certification or compliance based only on documentation.
