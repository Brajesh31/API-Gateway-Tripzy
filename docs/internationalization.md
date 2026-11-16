# 🌐 Internationalization and localization

## Scope

The gateway is an API boundary, not a translation engine. It should preserve a predictable encoding and locale contract while avoiding hidden transformations of domain content.

## 1. Character encoding

- Prefer UTF-8 for JSON request and response bodies.
- Validate content type and charset where the selected framework supports it.
- Do not silently reinterpret invalid byte sequences.
- Preserve Unicode content unless a service contract requires normalisation.
- Test combining characters, non-Latin scripts, and emoji in safe fields.

## 2. Locale negotiation

If localized responses are supported, document whether locale comes from an explicit request parameter, an allowlisted Accept-Language value, or a user preference handled by the domain service. Avoid guessing a locale from IP address.

Do not use locale as an authorization signal.

## 3. Error messages

Stable machine-readable error codes should remain language-neutral. Human-readable messages may be localized only when a clear translation ownership and fallback policy exists. Clients should branch on error code, not message text.

## 4. Date, time, and number values

Prefer unambiguous ISO 8601 timestamps with timezone/offset information. Numeric values should use documented units and machine-readable formats. Currency values should include an explicit currency code where relevant.

The gateway should not convert domain dates, currencies, or measurement units unless the contract explicitly assigns that responsibility to it.

## 5. Testing

- Include UTF-8 and non-Latin request values.
- Test URL encoding and decoding consistently.
- Test invalid byte sequences and unsupported charsets.
- Confirm error codes remain stable across locales.
- Verify that logs do not unintentionally expose localized personal content.
