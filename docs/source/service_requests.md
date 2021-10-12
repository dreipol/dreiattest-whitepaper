# Service Requests

Service requests have to be signed using a [registered](key_registration.md) key pair. dreiAttest automatically intercepts requests that match a specified url prefix (e.g. example.com/attested) and adds the signature and other necessary information as HTTP headers. The server validates the signature and if this validation is successful it forwards the request to the view.

To prevent replay-attacks dreiAttest will support obaining a nonce from the server in the future. Currently `00000000-0000-0000-0000-000000000000` is always used in place of the nonce. We recommend using SSL with certificate pinning to mitigate the risk of replay-attacks.

**Important: the service should validate that the user id matches the expected value. However the user id is NOT authenticated at this point. For authentication other techniques (JWT, etc.) should be used.**

```{mermaid}
sequenceDiagram
participant App
participant App Library
participant Middleware as View-Decorator
participant Server as Service
App->>App Library: initiate request
App Library->>Middleware: signed request
Note over Middleware: verify signature
alt verification successful
Middleware->>Server: app request, uid
Note over Server: execute request, ensure uid matches
Note over Server: uid is not authenticated, yet!
Server->>Middleware: response, shouldDelete
opt shouldDelete
Note over Middleware: delete pubkey
end
Middleware->>App Library: response
App Library->>App: response
else
Middleware->>App Library: error
Note over App Library: handle error<br>e.g. by generating new<br>keys and retrying or<br>forwarding to app
end
```

## Specification
Service requests are automatically signed by dreiAttest. The signature is added to the request as a header. Additionally the view decorator requires the uid and and the nonce that was used so it can verify the signature. The corresponding headers are added before the signature is generated, i.e. they are also signed.

Headers (in addition to the [common headers](common_headers.md)):
> "Dreiattest-uid": "hello@example.com;123e4567-e89b-12d3-a456-426614174000"  
> "Dreiattest-nonce": "00000000-0000-0000-0000-000000000000"  
> "Dreiattest-signature": sign(requestHash :: snonce),  
>
> > with requestHash = sha256(<request_url> :: <request_method> :: <request_headers_as_JSON (keys sorted alphabetically)> :: <request_body_data>)  

> "Dreiattest-user-headers": comma separated list of header names (e.g. "Accept,Cache-Control")

For the signature only the headers specified in "Dreiattest-user-headers" are considered. The reason for this is that proxies or other middlewares could add their own headers which are not part of the signature.

If the validation is successful the request is forwarded to the view which can then return its intended response.

**Validation not successful:**

Status: 403  
Headers:

> "Dreiattest-error": [Error Key](error_codes.md)

## Discussion
Requests that do not match the specified url prefix (e.g. example.com/attested) are forwarded to the service directly.
