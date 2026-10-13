# Security Policy

No gateway implementation is currently committed. When one is added, review authentication, authorization, upstream allowlists, header forwarding, SSRF resistance, request-size limits, rate limiting, TLS, secret storage, and sensitive logging.

Never commit service tokens, JWT signing keys, private URLs, user records, or production configuration. Report vulnerabilities privately to Brajesh Kumar (`@brajesh31`) with impact and redacted reproduction details.
