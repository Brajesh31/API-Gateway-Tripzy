# 🏛️ Project governance

## Purpose

This document defines how technical decisions, service contracts, security reviews, and repository changes should be governed. It does not create a formal organisation or imply that a larger maintainer group currently exists.

## 1. Roles

| Role | Responsibility |
|---|---|
| Repository maintainer | Maintains project direction, reviews changes, manages releases |
| Gateway contributor | Implements and documents approved gateway behavior |
| Upstream service owner | Confirms service contract, ownership and operational expectations |
| Security reviewer | Reviews identity, authorization, network boundaries and abuse cases |
| Platform operator | Owns runtime, deployment, monitoring and incident response |
| API consumer | Reports integration needs and compatibility issues |

## 2. Decision principles

- Security and correctness take precedence over convenience.
- Route definitions require verified service contracts.
- No endpoint or upstream URL is invented to make documentation appear complete.
- Breaking changes require a versioning and deprecation plan.
- Significant architectural trade-offs are recorded as ADRs.
- Runtime claims must be backed by code and tests.
- Documentation changes accompany behavior changes.

## 3. Route approval

A new public route requires service owner confirmation, public API review, authentication and permission definition, schema review, timeout/retry/idempotency policy, quota policy, contract tests, security review, and operational ownership.

## 4. Change approval

Reviewers should assess correctness, security, test evidence, compatibility, maintainability, documentation, and rollout risk. A successful CI run does not replace code review or service-owner approval.

## 5. Conflict resolution

Start with the relevant service contract and written architecture decisions. If disagreement remains, record options, consequences, risks, and the decision in an ADR. Revisit the decision when assumptions materially change.

## 6. Security-sensitive changes

Changes to authentication, authorization, route destinations, header forwarding, TLS, secret handling, rate limits, or error disclosure require explicit security review and negative tests.

## 7. Governance updates

Update this document when maintainer responsibilities, release authority, route approval, or security escalation changes.
