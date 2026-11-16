# 🔒 Privacy and data handling

> This is an engineering baseline, not a legal privacy notice or compliance certification. No gateway runtime currently processes requests.

## 1. Data potentially visible to a gateway

A gateway may transiently process:
- Authorization tokens and identity claims.
- Request path, query, headers, and body.
- Client network metadata.
- Correlation and trace identifiers.
- Upstream responses and error metadata.

Do not assume gateway payloads are non-sensitive because domain storage belongs to another service.

## 2. Data minimisation

- Inspect only the fields required for routing, policy, and validation.
- Do not log complete request or response bodies by default.
- Never log bearer tokens, cookies, passwords, private keys, or signing secrets.
- Avoid recording query values that may contain personal information.
- Keep only telemetry dimensions needed to operate the service.
- Do not introduce tracking or analytics without an approved purpose and review.

## 3. Data classification

| Class | Examples | Handling |
|---|---|---|
| Public | Public route ID, documentation version | May appear in public docs |
| Internal | Service identifiers, internal error categories | Restrict to operational context |
| Confidential | User payloads, identity claims, private service metadata | Minimise, protect, avoid logging |
| Secret | Tokens, passwords, signing keys, client secrets | Never log or commit; secret manager only |

The data owner must confirm the actual classification of each route payload.

## 4. Retention and access

Define retention separately for logs, traces, metrics, audit records, and any shared idempotency/session store. Restrict access by role. Confirm whether observability vendors, backups, and exported archives inherit the same retention policy.

## 5. Service responsibility

The gateway forwards only fields required by the approved contract. Services remain responsible for domain data classification, persistence, retention, consent, deletion, and resource-level authorization.

## 6. Incident response

For suspected data exposure:
1. Restrict affected access and preserve controlled evidence.
2. Revoke compromised credentials at the issuing provider.
3. Identify affected route IDs, time range, and upstream services.
4. Review redacted logs and service access records.
5. Notify owners through the approved incident process.
6. Remove unnecessary retained copies where policy permits.
7. Record root cause and corrective actions.

## 7. Design review questions

- [ ] Does the gateway need to inspect this field?
- [ ] Can it be forwarded without logging it?
- [ ] Is the claim necessary for the policy decision?
- [ ] Is any data retained beyond the request?
- [ ] Who can access logs and traces?
- [ ] Are third-party processors approved?
- [ ] Are examples synthetic and non-identifying?
