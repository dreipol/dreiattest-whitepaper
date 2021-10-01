# Key Endpoints
## GET /dreiattest/nonce
Headers (in addition to the [common headers](common_headers.md)):  
> "Dreiattest-uid": "hello@example.com;123e4567-e89b-12d3-a456-426614174000"  

Response: random 32-byte hex Data as JSON  
e.g. "f81d4fae-7dec-11d0-a765-00a0c91e6bf6"

Get a nonce (snonce) for registering a key. Only one nonce per uid can be valid at a given point in time. The mobile libraries, therefore, have to ensure that a key is only generated and registered once even when multiple requests are initiated at the same time.

## POST /dreiattest/key
Headers (in addition to the [common headers](common_headers.md)):  
> "Dreiattest-uid": "hello@example.com;123e4567-e89b-12d3-a456-426614174000"  
> "Dreiattest-nonce": "<snonce>"  

Body:  
```json
{
    "public_key": "AAAAc3...", // Android
    "key_id": "AAAAc3...", // iOS
    "attestation": "o2NmbX...", //attestation object provied by the platform specific service
    "driver": "apple|google"
}
```

Registers a key. The nonce required by DeviceCheck / SafetyNet is calculated as
```
nonce = sha256(uid :: pubkey :: snonce).
```

For more information see:  
- <https://developer.apple.com/documentation/devicecheck/validating_apps_that_connect_to_your_server/>  
- <https://developer.android.com/training/safetynet/attestation#verify-attestation-response/>  

Response:  
Status: 200  
Body: Empty  

**or**

Status: 403  
Headers:  
> "Dreiattest-error": Error Key  

## DELETE /dreiattest/key
Headers (in addition to the [common headers](common_headers.md)):  
> "Dreiattest-uid": "hello@example.com;123e4567-e89b-12d3-a456-426614174000"  
> "Dreiattest-nonce": "<snonce>" // or 0000… if signOnly is used  
> "Dreiattest-signature":  same as for service requests  

Content: plain text  
<pubkey>  
e.g. AAAAc3...  

Deregister key.
