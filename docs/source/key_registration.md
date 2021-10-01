# Key Registration
From the point of view of the host application / server the process of generating and registering keys is completely opaque. Keys are generated and registered lazily when they are required to sign a request.

The diagram shows the steps involved in registering a key with the view decorator. When a request is sent to an endpoint protected by dreiAttest the mobile library first checks whether it already has a key registered. If that is not the case it starts the registration process by requesting a nonce (snonce) from the view decorator to prevent replay attacks. We then calculate a new nonce based on the user id, public key and snonce and give it to DeviceCheck / SafetyNet. In this way Apple / Google effectively sign the pair of a uid and public key and attest that they come from a genuine installation of our app. If available we use a hardware-based keystore, thus ensuring that the private key cannot be moved off device. Any future requests that are signed by this key, therefore, have to come from the same device, allowing us to assume that that device is still genuine.

We can then forward the attestation together with the user id and public key to the server. On the server we verify the attestation including the signature of the user id and public key. If the verification is successful the server stores the public key for future use.

```{mermaid}
sequenceDiagram
participant Attestation Service
participant App Library
participant Server Middleware as View-Decorator
App Library->>+Server Middleware: request nonce
Server Middleware->>App Library: snonce
Note over App Library: nonce = sha256(uid :: pubkey :: snonce)
App Library->>Attestation Service: request attestation
Attestation Service->>App Library: attestation
App Library->>Server Middleware: uid, pubkey, attestation
Note over Server Middleware: evaluate attestation and store public key
Server Middleware->>App Library: ACK or error
```
