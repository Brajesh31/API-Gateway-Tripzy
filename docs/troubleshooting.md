# 🔧 Troubleshooting and diagnostics

## 1. Current scaffold limitations

The repository does not currently contain a running gateway. There is no server entry point, HTTP framework, route registry, authentication middleware, upstream client, or test suite. The npm test script is a placeholder failure. This is expected until implementation work is completed.

## 2. Installation and package issues

### Node version mismatch

Check the installed runtime:

~~~bash
node --version
npm --version
~~~

The package metadata declares Node.js 20 or later. Use a supported Node.js release compatible with the future runtime dependencies.

### npm install fails

Check registry connectivity, proxy configuration, lockfile consistency, and package permissions. Do not bypass certificate validation or add unreviewed registry mirrors.

### npm test fails

The current command intentionally exits with an explanatory message because no test files exist. Add the real test suite and update the package script when runtime implementation begins.

## 3. There is no server to start

No server source or start script exists in the current tree. Do not invent a PORT value and assume the process is listening. Implement the server and health endpoints before attempting local API calls.

## 4. Environment variables have no effect

The .env.example file is a planning template. No dotenv loader or configuration module currently reads it. Environment variables will have an effect only after validated configuration is implemented.

## 5. Future 401 or 403 responses

Check issuer, audience, signing-key ID, signature, token time claims, and permission mapping. Confirm the request reached the intended environment. Do not paste bearer tokens into issues or logs. Never disable token verification as a workaround.

## 6. Future 429 responses

Check the documented quota policy, identity key, window, burst allowance, and retry guidance. Ensure client retries include backoff and do not amplify load. Do not increase quotas during an incident without service-owner review.

## 7. Future 502, 503, or 504 responses

1. Capture route ID, safe error code, and correlation ID.
2. Check upstream health and service owner status.
3. Inspect sanitized connection, TLS, timeout, and response diagnostics.
4. Confirm the trusted route registry and environment configuration.
5. Check per-service concurrency and circuit state.
6. Avoid increasing retries during an outage.
7. Never expose private hosts or raw exceptions to callers.

## 8. Intermittent timeouts

Compare gateway and upstream durations, queueing, connection reuse, DNS behavior, retry count, and downstream latency. Check whether the client deadline is shorter than the gateway budget. Ensure cancellation propagates when requests expire.

## 9. CORS confusion

CORS is a browser-enforced cross-origin policy, not API authentication or authorization. Non-browser clients can still call a public server. Use identity verification, route authorization, network controls, and rate limiting.

## 10. Safe diagnostic bundle

Include commit SHA, environment, timestamp/timezone, route ID, HTTP method, status, correlation ID, sanitized duration, safe error code, runtime version, and relevant configuration key names.

Exclude access tokens, cookies, passwords, signing keys, full environment output, private payloads, customer data, and internal URLs.

## 11. Escalation

For a service-specific failure, contact the verified upstream owner with route ID and redacted correlation evidence. For suspected compromise or data exposure, use the private security reporting process in [SECURITY.md](../SECURITY.md).
