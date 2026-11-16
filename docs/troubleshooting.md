# 🔧 Troubleshooting

## There is no server to start

Expected at the current scaffold stage. The manifest points to `index.js`, but that file is absent. No start script exists.

## `npm test` fails

The current test command is a placeholder that exits with code 1. A test framework and suite have not yet been added.

## Environment variables have no effect

The environment example is planning documentation only. No dotenv loader or config validator exists. Add and verify this with the server implementation.

## Future upstream 502/503/504

1. Capture correlation ID and route ID.
2. Check upstream health and owner status.
3. Inspect sanitized connection, TLS, timeout and response errors.
4. Confirm trusted registry and environment configuration.
5. Check per-service concurrency and circuit status.
6. Do not expose private hosts or raw exceptions to callers.

## Unexpected 401/403

Check issuer, audience, signature key, time, expiry and permission. Never disable verification to work around an authentication incident. Never paste a bearer token into an issue.

## Intermittent timeouts

Compare gateway and upstream durations, retry counts, queueing, connection reuse, DNS, and downstream latency. Ensure retries are not amplifying outages.

## CORS confusion

CORS is a browser policy, not API authorization. Non-browser clients can still call a public server. Use authentication, authorization, network controls and rate limiting.

## Safe diagnostic bundle

Include commit SHA, environment, timestamp/timezone, route ID, status, correlation ID, sanitized duration and error code, and relevant configuration key names.

Exclude access tokens, cookies, passwords, signing keys, full environment output, private payloads, and internal URLs.
