# 🔐 Authentication and authorization design

## 1. Scope and status

No identity provider, token verifier, authentication middleware, or authorization engine is implemented in this repository. This guide defines the integration contract required before a protected route can be exposed.

## 2. Authentication versus authorization

Authentication establishes a verified principal. Authorization evaluates whether that principal may perform a specific operation. A valid token does not automatically grant access to every route or resource.

## 3. Identity-provider discovery checklist

Obtain written answers from the identity provider owner:
- Token format and signing algorithms.
- Issuer and audience values.
- JWKS/key endpoint and cache/rotation behavior.
- Required claims and their types.
- Expiry and not-before behavior.
- Clock-skew tolerance.
- Token revocation or short-lifetime strategy.
- User, service, client, tenant, and admin identity distinctions.
- Permission/scope naming and change lifecycle.
- Outage and key-fetch failure expectations.

Do not implement a generic verifier with guessed claims or permissive defaults.

## 4. Verification flow

~~~mermaid
flowchart TD
  A[Credential received] --> B[Parse credential safely]
  B --> C[Resolve trusted key]
  C --> D[Verify signature and algorithm]
  D --> E[Validate issuer, audience and time]
  E --> F[Validate required claims]
  F --> G{All checks pass?}
  G -->|No| H[401 and safe audit event]
  G -->|Yes| I[Create minimal trusted principal]
  I --> J[Load route policy]
  J --> K{Permission present?}
  K -->|No| L[403 and no upstream request]
  K -->|Yes| M[Continue to quota and routing]
~~~

## 5. Authorization policy

Each protected route should declare a permission set and policy mode. Define whether multiple permissions use AND or OR semantics. Do not infer admin privileges from user-controlled headers or arbitrary claims.

The upstream domain service must still validate access to individual resources, ownership, tenant boundaries, and business state. Gateway route permission is not a substitute for object-level authorization.

## 6. Header and identity context

Remove client-supplied identity headers. After verification, construct a minimal internal identity context. Forward only fields required by the upstream contract, preferably through a service-authenticated channel rather than blindly forwarding the original user token.

## 7. Failure behavior

Protected routes fail closed if token verification cannot be trusted. Distinguish invalid credentials from insufficient permissions. Avoid detailed public errors that reveal signing-key or account state.

## 8. Test matrix

- Missing and malformed credentials.
- Invalid signature and unknown key.
- Wrong issuer and audience.
- Expired and not-yet-valid token.
- Unsupported algorithm.
- Missing subject, tenant, or required claim.
- Permission missing or revoked.
- Spoofed identity headers.
- Cross-tenant access.
- Identity provider/JWKS outage.
- Denial before any upstream request.

## 9. Acceptance criteria

No protected route is active until identity configuration is explicit, verifier behavior is covered by negative tests, route permissions are documented, and upstream resource authorization remains intact.
