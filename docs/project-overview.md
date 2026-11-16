# 🎯 Project overview

## What this repository is

API Gateway Tripzy is a Node.js repository intended to become a common client-facing entry point for a Tripzy microservices ecosystem. Its current committed state is a package/documentation scaffold, not an operational gateway.

## Problem the gateway may solve

As a product grows, clients can become coupled to multiple service hostnames, authentication conventions, error formats, and operational policies. A gateway can centralise cross-cutting controls while leaving domain logic with the services that own it.

Potential shared responsibilities:
- Route matching and API versioning.
- Identity token verification.
- Authorization at the boundary.
- Request size and schema checks.
- Rate limiting and quotas.
- Upstream timeouts and bounded retries.
- Correlation IDs and telemetry.
- Safe public error normalization.

## Goals

1. Define a secure, explicit boundary between public clients and internal services.
2. Keep routing and policy configuration reviewable.
3. Preserve service ownership of domain behavior and persistence.
4. Make failures observable and predictable.
5. Establish repeatable tests for routing and security behavior.

## Non-goals

Unless a future product decision explicitly changes the scope, the gateway should not:
- Reimplement trip, booking, user, payment, or search business logic.
- Own every microservice's database.
- Accept arbitrary client-supplied upstream URLs.
- Expose internal service discovery details.
- Become a generic open proxy.
- Hide partial failures behind misleading success responses.

## Current repository evidence

- The package manifest is present.
- The manifest names a future `index.js` entry point, but the file is absent.
- No framework, proxy library, auth library, route handlers, or test framework is installed.
- The package test script is a placeholder which exits with code 1.
- The environment example contains only proposed values.
- Existing docs describe intent, not runtime behavior.

## Actors

| Actor | Expected interaction |
|---|---|
| Web client | Calls public versioned gateway routes |
| Mobile client | Calls the same stable API boundary where appropriate |
| Partner client | Calls explicitly exposed and authorized integration routes |
| Identity provider | Issues or verifies identities/tokens |
| Upstream service | Owns domain-specific operations |
| Platform operator | Configures, deploys, monitors and recovers gateway instances |
| Security reviewer | Reviews boundaries, permissions, abuse controls and secret handling |

## Success measures to define before release

- Route availability and latency percentiles.
- Upstream timeout and error rates.
- Authentication and authorization rejection rates.
- Rate limit rejection behavior.
- Correlation coverage across gateway and upstream logs.
- Contract compatibility across service releases.
- No unintended exposure of internal endpoints or sensitive headers.

Numerical targets must be approved from product traffic and reliability needs; this repository does not define invented SLO values.
