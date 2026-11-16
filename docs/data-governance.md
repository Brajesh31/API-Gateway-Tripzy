# 🗃️ Data governance and ownership

## 1. Guiding principle

The gateway is a transport and policy boundary, not the owner of domain records. Each Tripzy service remains authoritative for its domain entities, validation, retention, and resource-level access decisions.

## 2. Ownership matrix

| Data category | Expected owner | Gateway responsibility |
|---|---|---|
| User profile | User/account service | Verify identity context and route safely |
| Trip or itinerary record | Relevant domain service | Forward approved request and response |
| Booking/payment record | Booking/payment owner | Enforce route-level access; never become system of record |
| Service configuration | Platform/gateway maintainers | Validate and protect deployment configuration |
| Route contract | Gateway and upstream owner jointly | Keep registry and API docs synchronized |
| Security audit event | Security/platform owner | Emit minimal approved event |
| Operational telemetry | Platform operations | Minimise and protect metadata |

The domain names in this table describe common service boundaries, not confirmed services in this repository.

## 3. Data lifecycle questions

Before introducing persistence, document collection purpose, owner, data classification, lawful/approved processing purpose, retention period, access roles, encryption, backup, restore, deletion, and downstream propagation.

## 4. Data quality and contract governance

A gateway should validate transport-level requirements and schema shape where required, but domain services own business invariants. Do not duplicate pricing, booking availability, eligibility, or other domain decisions in gateway middleware.

## 5. Shared-state exception

A shared store for quotas or idempotency is not automatically a domain database. It still requires a clear owner, retention, consistency model, encryption, access control, failure behavior, and recovery procedure.

## 6. Review checklist

- [ ] A named service owns the domain data.
- [ ] The gateway forwards only contract-approved fields.
- [ ] No unnecessary payload copy is stored.
- [ ] Logs and traces are minimised.
- [ ] Retention and access are documented.
- [ ] Domain services enforce resource-level authorization.
- [ ] Shared state has a documented ADR and recovery owner.
