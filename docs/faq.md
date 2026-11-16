# ❓ Frequently asked questions

## Is this a functioning API gateway?

No. The current repository is a Node.js package and documentation foundation. It has no server process, HTTP framework, route handlers, upstream proxy client, authentication middleware, or authorization engine.

## Why does the package manifest declare Node.js 20+?

It records the intended runtime baseline for future implementation. It does not mean a Node server currently starts.

## Which Tripzy services are connected?

None are confirmed or configured in this repository. Service IDs, destinations, owners, identity policies, and contracts must come from the relevant service teams.

## Which HTTP framework is used?

No framework has been selected or installed. The choice should consider supported Node.js versions, team familiarity, middleware requirements, request lifecycle, security, and performance.

## Are any API endpoints available?

No. There are no route handlers or route registry. Examples in the documentation are illustrative and use placeholder service names.

## Is JWT authentication implemented?

No. There is no token verifier or identity-provider integration. The identity provider, issuer, audience, signing algorithms, and permission claims are still unknown.

## Does the gateway use a database?

No database driver or schema exists. The preferred design is stateless. Domain services should own their data. A shared quota or idempotency store would require a separate architecture decision.

## Can I start the server locally?

No server entry point or start script exists yet. npm install only installs the currently declared package tree.

## Why does npm test fail?

The current test script is an explicit placeholder that exits with a message. There are no test files. This is intentionally documented rather than reporting a fake passing suite.

## Do the diagrams represent implemented functionality?

No. They describe a proposed architecture and request pipeline. Every diagram must be reconciled with code when runtime implementation begins.

## Can the client pass an upstream URL?

No. Future routes must map to preconfigured service identifiers. A client-selected destination would create an open-proxy and SSRF risk.

## Does gateway authorization replace service authorization?

No. The gateway can enforce route-level permissions. The service owning the data must still enforce resource-level access and business invariants.

## Why are there no screenshots of a working gateway?

There is no running gateway UI or API server to capture. The repository includes an original architecture banner and editable technical diagrams. No mock screenshot is labelled as a real application screenshot.

## How should I propose a new route?

Provide the owning service, approved contract, public path/method, identity model, permission, request/response schemas, timeout, quota, retry/idempotency behavior, test environment, and escalation owner. Use the service contract template and route review process.

## How should I report a security issue?

Use the private GitHub Security Advisory link in SECURITY.md. Do not post credentials, private service URLs, customer data, or exploit details in a public issue.
