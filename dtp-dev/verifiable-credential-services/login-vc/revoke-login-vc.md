# Revoke login VC

A revoked login VC can no longer be used to log in. The revocation is effective immediately for all logins via the DTP and cannot be undone. To enable the login again, a new login VC must be issued (enrolment).

## When to revoke

* The customer reports a lost or stolen device.
* The customer relationship or contract ends.
* A new login VC is issued to the same customer (only one active login VC per wallet).
* Suspicion of misuse or a security incident.

## Revoke a login VC

`PUT /{tenantId}/verifiable-credentials/{vcId}/revoke`

Revokes an issued login VC, or withdraws an offer that has not been accepted yet. Use [List login VCs of a customer](login-vc-api.md) to find the `vcId`.

### Request

```json
{
  "reason": "DEVICE_LOST",
  "comment": "Reported by phone, ticket 4711"
}
```

<table data-full-width="true"><thead><tr><th width="180">Field</th><th width="120">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>reason</code></td><td>yes</td><td>Reason of the revocation, see table below. Returned as <code>statusDetails</code>.</td></tr><tr><td><code>comment</code></td><td>no</td><td>Free text for support purposes (max. 255 characters). Not visible to the end user.</td></tr></tbody></table>

<table><thead><tr><th width="260">reason</th><th>Description</th></tr></thead><tbody><tr><td><code>CUSTOMER_REQUEST</code></td><td>Requested by the customer.</td></tr><tr><td><code>DEVICE_LOST</code></td><td>Device lost or stolen.</td></tr><tr><td><code>CONTRACT_TERMINATED</code></td><td>Customer relationship or contract ended.</td></tr><tr><td><code>REPLACED</code></td><td>Replaced by a new login VC.</td></tr><tr><td><code>SECURITY_INCIDENT</code></td><td>Suspicion of misuse or security incident.</td></tr><tr><td><code>OTHER</code></td><td>Other reason, see <code>comment</code>.</td></tr></tbody></table>

### Response `200 OK`

```json
{
  "vcId": "b1e3a9f2-0c4d-4f61-8a2e-9d7c5e3f1a08",
  "customerId": "CUST-000123",
  "status": "REVOKED",
  "statusReason": "REVOKED_BY_TENANT",
  "statusDetails": "DEVICE_LOST",
  "revokedAt": "2026-10-22T09:15:00Z"
}
```

The call is idempotent: revoking an already revoked login VC returns `200 OK` with the existing revocation data.

## Revoke all login VCs of a customer

`PUT /{tenantId}/customers/{customerId}/verifiable-credentials/revoke`

Revokes all issued login VCs and withdraws all open offers of a customer, e.g. when the customer relationship ends. Same request body as above.

### Response `200 OK`

```json
{
  "customerId": "CUST-000123",
  "revoked": [
    { "vcId": "b1e3a9f2-0c4d-4f61-8a2e-9d7c5e3f1a08", "revokedAt": "2026-10-22T09:15:00Z" }
  ]
}
```

## Effect of the revocation

* The status of the login VC changes to `REVOKED` and a webhook notification is sent.
* The status list is updated. On the next login the DTP rejects the login VC with `CREDENTIAL_REVOKED`, see [EUDI Wallet authentication status and errors](../../authentication-services/eudi-wallet/eudi-wallet-authentication-status-and-errors.md).
* The login VC may still be displayed in the end user's wallet, marked as invalid depending on the wallet.

## Errors

<table><thead><tr><th width="120">HTTP</th><th width="280">Code</th><th>Description</th></tr></thead><tbody><tr><td>400</td><td><code>INVALID_REQUEST</code></td><td><code>reason</code> missing or invalid.</td></tr><tr><td>401</td><td><code>UNAUTHORIZED</code></td><td>Invalid API credentials.</td></tr><tr><td>404</td><td><code>NOT_FOUND</code></td><td>Login VC or customer not found.</td></tr><tr><td>409</td><td><code>VC_NOT_REVOCABLE</code></td><td>The login VC is in a final status that cannot be revoked (<code>EXPIRED</code>, <code>ISSUANCE_FAILED</code>).</td></tr></tbody></table>
