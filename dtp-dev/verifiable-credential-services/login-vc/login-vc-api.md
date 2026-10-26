# Login VC API

All API calls are authenticated as described in [API Authentication](../../general-information/api-authentication.md). To revoke a login VC see [Revoke login VC](revoke-login-vc.md).

## Create login VC

`POST /{tenantId}/verifiable-credentials`

Creates a credential offer for the end user. Every call creates a new offer with a unique, single-use link.

{% hint style="warning" %}
Call this endpoint only for a customer who is already identified or logged in, and deliver the offer only within this customer's authenticated session. See [Responsibility of the integrating customer](login-vc-issuance-flow.md).
{% endhint %}

### Request

```json
{
  "vcProfileId": 7,
  "customerId": "CUST-000123",
  "redirectUri": "https://app.example.com/wallet/done",
  "state": "q2Zt8kLmV0",
  "language": "de",
  "extApplication": "e-banking"
}
```

<table data-full-width="true"><thead><tr><th width="180">Field</th><th width="120">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>vcProfileId</code></td><td>yes</td><td>ID of the login VC profile. Provided by the DTP admin.</td></tr><tr><td><code>customerId</code></td><td>yes</td><td>Unique identifier of a known customer in the external application. Written into the login VC and returned as <code>sub</code> in the ID token on every login.</td></tr><tr><td><code>claims</code></td><td>no</td><td>Additional claim values, if the VC profile defines any (e.g. <code>contractNumber</code>). Must match the claim schema of the profile.</td></tr><tr><td><code>redirectUri</code></td><td>no</td><td>Redirect target after the issuance. Must match an allowed redirect URI of the profile. If omitted, the static success / failed redirect URLs of the profile are used.</td></tr><tr><td><code>state</code></td><td>no</td><td>Opaque value returned unchanged on the redirect.</td></tr><tr><td><code>validUntil</code></td><td>no</td><td>Expiry of the login VC. Must not exceed the maximum validity of the profile. Default from the profile.</td></tr><tr><td><code>language</code></td><td>no</td><td>Language of the DTP pages (<code>de</code>, <code>fr</code>, <code>it</code>, <code>en</code>). Default from the profile.</td></tr><tr><td><code>extCaseId</code>, <code>extApplication</code></td><td>no</td><td>External references, returned in the webhook and the GET response.</td></tr></tbody></table>

### Response `201 Created`

```json
{
  "vcId": "b1e3a9f2-0c4d-4f61-8a2e-9d7c5e3f1a08",
  "vcProfileId": 7,
  "customerId": "CUST-000123",
  "status": "SCHEDULED",
  "offerUrl": "https://{dtp-host}/{tenantId}/vc/offer/b1e3a9f2-0c4d-4f61-8a2e-9d7c5e3f1a08",
  "credentialOfferUri": "openid-credential-offer://?credential_offer_uri=https%3A%2F%2F{dtp-host}%2F...",
  "txCodeRequired": true,
  "txCode": "493817",
  "expiresAt": "2026-10-20T13:00:00Z"
}
```

* `offerUrl` is the DTP page that opens the wallet (same-device) or shows a QR code (cross-device). Use it like the `authUrl` of the authentication.
* `credentialOfferUri` is the raw OpenID4VCI offer, for tenants that want to render their own QR code or app link.
* `txCode` is returned **only once**. Show it to the end user in the logged-in session of the external application. It is never shown on the DTP page.
* The offer is valid until `expiresAt` (default 30 minutes, configured in the profile).

## Get login VC

`GET /{tenantId}/verifiable-credentials/{vcId}`

Returns the current `status`, `statusReason` and `statusDetails` of a login VC. Can be used e.g. for polling in the cross-device flow or for support cases.

```json
{
  "vcId": "b1e3a9f2-0c4d-4f61-8a2e-9d7c5e3f1a08",
  "vcProfileId": 7,
  "customerId": "CUST-000123",
  "status": "ISSUED",
  "statusReason": null,
  "statusDetails": null,
  "issuedAt": "2026-10-20T12:34:10Z",
  "validUntil": "2027-10-20T12:34:10Z",
  "lastUsedAt": "2026-10-21T08:02:44Z",
  "createdAt": "2026-10-20T12:30:00Z",
  "updatedAt": "2026-10-20T12:34:10Z"
}
```

## List login VCs of a customer

`GET /{tenantId}/verifiable-credentials?customerId={customerId}`

Returns all login VCs of a customer with their status, e.g. to find the active login VC before a [revocation](revoke-login-vc.md). Optional filter: `status` (e.g. `ISSUED`).

```json
{
  "items": [
    {
      "vcId": "b1e3a9f2-0c4d-4f61-8a2e-9d7c5e3f1a08",
      "status": "ISSUED",
      "issuedAt": "2026-10-20T12:34:10Z",
      "validUntil": "2027-10-20T12:34:10Z",
      "lastUsedAt": "2026-10-21T08:02:44Z"
    }
  ]
}
```

## Errors

<table><thead><tr><th width="120">HTTP</th><th width="280">Code</th><th>Description</th></tr></thead><tbody><tr><td>400</td><td><code>INVALID_REQUEST</code></td><td>Mandatory field missing or invalid.</td></tr><tr><td>400</td><td><code>INVALID_REDIRECT_URI</code></td><td><code>redirectUri</code> is not in the list of allowed redirect URIs.</td></tr><tr><td>401</td><td><code>UNAUTHORIZED</code></td><td>Invalid API credentials.</td></tr><tr><td>404</td><td><code>VC_PROFILE_NOT_FOUND</code></td><td><code>vcProfileId</code> unknown for this tenant.</td></tr><tr><td>404</td><td><code>NOT_FOUND</code></td><td>Login VC not found.</td></tr><tr><td>422</td><td><code>INVALID_CLAIMS</code></td><td><code>claims</code> do not match the claim schema of the VC profile.</td></tr></tbody></table>
