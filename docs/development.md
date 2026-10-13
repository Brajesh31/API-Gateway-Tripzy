# Development

Start by writing versioned API contracts and upstream service fixtures. Implement one route at a time with authentication, validation, timeout, error mapping, logging, and tests.

Use local environment variables for non-secret endpoints and a secret manager for credentials. Never log authorization headers, cookies, tokens, or full user payloads.
