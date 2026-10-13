# Contributing

This repository is a gateway scaffold. Before adding implementation, document the service contract and threat model.

- Keep routes, upstreams, timeouts, retries, and error mappings explicit.
- Never commit credentials, private endpoints, or production configuration.
- Add unit, contract, and integration tests for every gateway route.
- Preserve correlation IDs and avoid logging tokens or personal data.
- Run the package's checks after adding real scripts; replace the placeholder test script with an actual runner.

Pull requests should describe affected services, authorization, failure behavior, observability, and rollback impact.
