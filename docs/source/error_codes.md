# Error Keys
Errors returned by the server via the `Dreiattest-error` header:

- **dreiAttest_policy_violation**: Validation of the attestation failed (also returned if a plugin rejects the attestation)
- **dreiAttest_nonce_mismatch**: The nonce used in a [POST /dreiattest/key](key_endpoints.html#post-dreiattest-key) request is invalid or has expired
- **dreiAttest_invalid_key**: The key used to sign a request is not (or no longer) registered. The mobile libraries will automatically try to register a new key and retry the request.
