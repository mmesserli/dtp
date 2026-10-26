# Webhook notifications

## Overview

This documents the notification WebHook API that the external application **MUST** implement on their side to receive notifications from the identification process. After creating the identification case and on each status update of the identification case a webhook notification will be sent.

The endpoint of this API **SHOULD** be provided by the tenant to the provider in prior to registration of the Tenant. Request to change the endpoint is supported.

Clients are recommended to ignore unused fields in the WebHook request's payload to avoid unnecessary errors in the event that new fields are added.

## TLS

* The implementation of this API **MUST** support HTTPS (`TLS v1.2` and `v1.3`).
* Self-signed Certificates are **NOT** supported.

## Responses

The implementation of the API **MUST** respond with either HTTP `200 OK` or `201 No Content`. Other HTTP status will be treated as failure and the Retry Policy will be applied.

## Request Signature Verification

Each notification sent to the Tenant's endpoint always include a signature. The signature is generated using a **shared secret** so that the Tenant can verify the requests signature.

### Request Headers

IMPORTANT: Please make sure to configure your Web Application Firewall to let one of the headers passthrough.

The following headers are sent along with each HTTP request.

NOTE: Prefixing `X-` with non-standard HTTP headers is a deprecated practice. However, we support `x-ubiid-*` for backward-compatibility.

#### `ubiid-notification-signature` and `x-ubiid-notification-signature`

Contains the HMAC-SHA512 signature. You will need to compute the signature on your side and then compare it with our sigature to determine the request is not tampered with.

#### `ubiid-notification-timestamp` and `x-ubiid-notification-timestamp`

Contains the timestamp in ISO-8601 format (`YYYYMMDD'T'HHMMSS'Z'`). The time is always in UTC.

Examples: `20200623T000032Z`

### Signature Generation Algorithm

Clients are recommended to use the WebHook request's body as raw string to verify against the Signature before unmarshalling to object types.

Output of calculated `HMAC-SHA512` should be in binary format and then encoded to base64.

The following specifies how to verify the signature:

1 - Prepare the payload.

```
* Obtain the timestamp via the HTTP header `ubiid-notification-timestamp`.
* Append a pipe (`|`) character.
* Append the body of the notification. It MUST be a JSON string with all blank spaces need to be removed so that the whole JSON object is in one line. (This is like applying `JSON.stringify(requestBody)`).
```

2 - Use the shared _Secret_ to compute a `HMAC-SHA512` using the payload as the message and encode it using `Base64`.

3 - Compare the computed signature with the one sent in `ubiid-notification-signature`.

## Retry Policy

In case of failure there is a retry policy in place to send the notification with exponential backoff policy that will increase the wait time for each retry attempt until max number of attempts is reached. The maximum of notification will be 10 times.
