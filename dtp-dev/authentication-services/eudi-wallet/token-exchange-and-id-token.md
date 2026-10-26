# Token exchange and ID token

After a successful authentication the end user is redirected to the external application with an authorization `code`. The external application exchanges this code server-to-server for an ID token (JWT), following the OpenID Connect authorization code flow.

{% hint style="info" %}
**Why a code and not a JWT in the redirect URL?** Data in a redirect URL ends up in browser history, proxy and server logs and referrer headers. Passing the JWT directly (OIDC implicit flow) is discouraged by current OAuth security best practice. The code is short-lived, single-use and worthless without the API credentials of the external application, and the JWT is only transferred over the authenticated back channel.
{% endhint %}

## Exchange code for ID token

`POST /{tenantId}/auth/token`

Authenticated with the API credentials of the external application (see [API Authentication](../../general-information/api-authentication.md)).

### Request

```http
POST /{tenantId}/auth/token
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&code=SplxlOBeZQQYbYS6WxSbIA
&redirect_uri=https%3A%2F%2Fapp.example.com%2Flogin%2Fcallback
```

* `code` is single-use and valid for 60 seconds.
* `redirect_uri` must be identical to the redirect URI used for the authentication request (static or dynamic).

### Response `200 OK`

```json
{
  "id_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6ImR0cC0yMDI2In0...",
  "token_type": "Bearer",
  "expires_in": 300
}
```

### Errors

<table><thead><tr><th width="120">HTTP</th><th width="220">error</th><th>Description</th></tr></thead><tbody><tr><td>400</td><td><code>invalid_grant</code></td><td>Code is unknown, expired, already used or the <code>redirect_uri</code> does not match.</td></tr><tr><td>400</td><td><code>unsupported_grant_type</code></td><td>Only <code>authorization_code</code> is supported.</td></tr><tr><td>401</td><td><code>invalid_client</code></td><td>Invalid API credentials.</td></tr></tbody></table>

## ID token

The ID token is a JWT signed by the DTP (RS256).

### Login with login VC

```json
{
  "iss": "https://{dtp-host}/{tenantId}",
  "sub": "CUST-000123",
  "aud": "{tenantId}",
  "iat": 1760962872,
  "exp": 1760963172,
  "auth_time": 1760962870,
  "nonce": "n-0S6_WzA2Mj",
  "acr": "urn:dtp:acr:login-vc",
  "amr": ["login_vc"],
  "auth_request_id": "7f3c2a9e-5b1d-4c8e-9a6f-2d4b8e1c0f57",
  "vc_id": "b1e3a9f2-0c4d-4f61-8a2e-9d7c5e3f1a08"
}
```

### PID login (fallback)

```json
{
  "iss": "https://{dtp-host}/{tenantId}",
  "sub": "CUST-000123",
  "aud": "{tenantId}",
  "iat": 1760962872,
  "exp": 1760963172,
  "auth_time": 1760962870,
  "nonce": "n-0S6_WzA2Mj",
  "acr": "urn:dtp:acr:eudi-pid",
  "amr": ["eudi_pid"],
  "auth_request_id": "7f3c2a9e-5b1d-4c8e-9a6f-2d4b8e1c0f57",
  "given_name": "Erika",
  "family_name": "Mustermann",
  "birthdate": "1984-01-26"
}
```

<table data-full-width="true"><thead><tr><th width="180">Claim</th><th>Description</th></tr></thead><tbody><tr><td><code>sub</code></td><td>The <code>customerId</code> sent in the authenticate request.</td></tr><tr><td><code>nonce</code></td><td>The <code>nonce</code> sent in the authenticate request.</td></tr><tr><td><code>acr</code> / <code>amr</code></td><td>Authentication method: <code>login_vc</code> (login VC) or <code>eudi_pid</code> (PID login as fallback).</td></tr><tr><td><code>vc_id</code></td><td>ID of the presented login VC. Only for login VC. Can be used to <a href="../../verifiable-credential-services/login-vc/revoke-login-vc.md">revoke</a> the login VC.</td></tr><tr><td>Additional claims</td><td>Login VC: only claims defined in the VC profile, no personal data by default. PID login: PID attributes configured in the authentication profile (e.g. <code>given_name</code>, <code>family_name</code>, <code>birthdate</code>).</td></tr></tbody></table>

## Validate the ID token

The external application must validate the ID token before logging the user in:

1. Verify the signature with the public keys from `GET /{tenantId}/.well-known/jwks.json`.
2. Check `iss`, `aud` and that `exp` is in the future.
3. Check that `nonce` matches the value sent in the authenticate request.
4. Check that `sub` matches the customer that should be logged in.

The OpenID configuration with all endpoints is available at `GET /{tenantId}/.well-known/openid-configuration`.
