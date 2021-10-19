# Key Endpoints
## GET /dreiattest/nonce
Headers (in addition to the [common headers](common_headers.md)):  
> "Dreiattest-uid": "hello@example.com;123e4567-e89b-12d3-a456-426614174000"  

Response: random 32-byte base64 encoded string  
e.g. "BZsLqvMo1ayGJ+Y/BOdTHgrDQec8N015JuAUV9Uzptw="

Get a nonce (snonce) for registering a key.

**Discussion**  
Only one nonce per uid can be valid at a given point in time. The mobile libraries, therefore, have to ensure that a key is only generated and registered once even when multiple requests are initiated at the same time. The nonce expires once it has been used or after one minute.

## POST /dreiattest/key
Headers (in addition to the [common headers](common_headers.md)):  
> "Dreiattest-uid": "hello@example.com;123e4567-e89b-12d3-a456-426614174000"  
> "Dreiattest-nonce": "<snonce>"  

Body:  
```none
{
    "public_key": "AAAAc3...", // Android
    "key_id": "AAAAc3...", // iOS
    "attestation": "o2NmbX...", //attestation object provied by the platform specific service
    "driver": "apple|google"
}
```

Registers a key.

Response:  
Status: 200  
Body: `{"success": True, "key_id": "AAAAc3..."}`  

**or**

Status: 403  
Headers:  

> "Dreiattest-error": [Error Key](error_codes.md)  

**Discussion**  
The nonce required by DeviceCheck / SafetyNet is calculated as
```
nonce = sha256(uid :: pubkey :: snonce)
```
Thus, the request is effectively signed by the attestation service.

For more information see:  
- <https://developer.apple.com/documentation/devicecheck/validating_apps_that_connect_to_your_server/>  
- <https://developer.android.com/training/safetynet/attestation#verify-attestation-response/> 
