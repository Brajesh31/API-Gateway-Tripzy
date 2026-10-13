# Troubleshooting

## There is no start command

This is expected: the repository has no server entry point yet. Implement and test the gateway before adding deployment instructions.

## Tests fail

The current `npm test` script is a placeholder that exits with an error. Replace it with the chosen test runner when implementation begins.

## Upstream requests fail

For a future implementation, inspect sanitized status, timeout, correlation ID, allowlist, and service health information. Never expose private upstream URLs or tokens in client responses.
