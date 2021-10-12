# Configuration
## Mobile
The user id (**uid**) is the login name a user uses with the service protected by dreiAttest. It should be 0-255 characters in length and contain only the following characters: `a-zA-Z0-9._-@`
To make this id unique across multiple devices dreiAttest will automatically append a RFC-4122-UUID (seperated by `;`) to the uid you provide. The result may look something like "hello@example.com;123e4567-e89b-12d3-a456-426614174000".

Apple's DeviceCheck counts the number of keys it issues for different users as part of its [risk metric](https://developer.apple.com/documentation/devicecheck/assessing_fraud_risk), allowing you to identify devices that switch users a lot wich may be a sign of fraud. If a service does not require a login the uid may be omitted resulting in an effective uid similar to ";123e4567-e89b-12d3-a456-426614174000."

-----------

The **validation level** can currently only be set to `signOnly`. In the future it will be possible to request a nonce from the server for every request in order to prevent replay-attacks. For the time being we recommend using SSL and certificate pinning to prevent replay attacks. 
When `signOnly` is set `00000000-0000-0000-0000-000000000000` is used instead of a nonce. During key registration a nonce is always requested independent of this setting.

## Server
The server needs knowledge of the used App ID in case of Apple and the APK name as well as the APK certificate digest for google. There are more fine grained configuration possibilities described in the [django-dreiattest docs](https://github.com/dreipol/django-dreiattest).

