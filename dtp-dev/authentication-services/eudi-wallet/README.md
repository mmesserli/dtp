# EUDI Wallet

## How it works

With EUDI Wallet authentication, the external application logs in its customers with their European Digital Identity Wallet instead of a password. The DTP handles the communication with the wallet and the validation of the presented credentials, and returns the result to the external application in OpenID Connect format.

The login is based on a **login VC**: a verifiable credential that the DTP issues on behalf of the tenant into the customer's wallet, bound to the tenant's unique customer identifier (`customerId`). The login VC is issued only for a customer who is already identified or logged in at the integrating customer.

<table data-full-width="true"><thead><tr><th width="160">Phase</th><th width="200">When</th><th>What happens</th></tr></thead><tbody><tr><td><strong>Enrolment</strong></td><td>Once per customer and wallet</td><td>In the authenticated session of a known customer, the external application requests a login VC for this customer's <code>customerId</code>. The DTP issues the login VC into the wallet. See <a href="../../verifiable-credential-services/login-vc/">Login VC</a>.</td></tr><tr><td><strong>Login</strong></td><td>Every login</td><td>The external application creates an authentication request with the <code>customerId</code>. The end user presents the login VC. The DTP validates it and redirects back with an authorization code, which the external application exchanges for an ID token. See <a href="eudi-wallet-authentication-flow.md">EUDI Wallet authentication flow</a>.</td></tr></tbody></table>

**Why a login VC instead of the PID on every login**

* **Data minimisation:** only the `customerId` is shared on login, no personal data.
* **Control by the tenant:** the tenant can revoke the login VC at any time, e.g. when the device is lost or the customer relationship ends.
* **Binding once:** wallet and customer are bound at enrolment, not matched again on every login.
* **Independent of the PID:** the login keeps working when the PID is renewed in the wallet.

{% hint style="info" %}
**PID login (optional).** The authentication profile can also accept the PID directly, e.g. for customers whose wallet does not (yet) accept the login VC. See [PID login as fallback](eudi-wallet-authentication-flow.md). The login VC is recommended.
{% endhint %}

## Prerequisites

<table data-full-width="true"><thead><tr><th width="260">Prerequisite</th><th>Description</th></tr></thead><tbody><tr><td><strong>Authentication profile</strong></td><td>Configured by the DTP admin on request. Defines the accepted credentials (login VC and/or PID), the redirect URLs, the allowed redirect URIs and the attributes returned in the ID token.</td></tr><tr><td><strong>VC profile for the login VC</strong></td><td>Configured by the DTP admin on request. Defines the claims, validity, offer expiry and transaction code. The tenant receives the <code>vcProfileId</code>.</td></tr><tr><td><strong>Known customer</strong></td><td>The integrating customer identifies or authenticates the customer before requesting a login VC. See <a href="../../verifiable-credential-services/login-vc/login-vc-issuance-flow.md">Responsibility of the integrating customer</a>.</td></tr><tr><td><strong>Supported wallet</strong></td><td>The end user needs a wallet that supports the required standards. See <a href="supported-wallets.md">Supported wallets</a>.</td></tr><tr><td><strong>API credentials and webhook</strong></td><td>See <a href="../../general-information/api-authentication.md">API Authentication</a> and <a href="../../general-information/webhook-notifications.md">Webhook notifications</a>.</td></tr></tbody></table>

## EUDI Wallet authentication documents

{% content-ref url="eudi-wallet-authentication-flow.md" %}
[eudi-wallet-authentication-flow.md](eudi-wallet-authentication-flow.md)
{% endcontent-ref %}

{% content-ref url="../../verifiable-credential-services/login-vc/" %}
[login-vc](../../verifiable-credential-services/login-vc/)
{% endcontent-ref %}

{% content-ref url="authenticate-api.md" %}
[authenticate-api.md](authenticate-api.md)
{% endcontent-ref %}

{% content-ref url="token-exchange-and-id-token.md" %}
[token-exchange-and-id-token.md](token-exchange-and-id-token.md)
{% endcontent-ref %}

{% content-ref url="eudi-wallet-authentication-status-and-errors.md" %}
[eudi-wallet-authentication-status-and-errors.md](eudi-wallet-authentication-status-and-errors.md)
{% endcontent-ref %}

## General information

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>API Authentication</strong></td><td></td><td><a href="../../general-information/api-authentication.md">api-authentication.md</a></td><td><a href="../../.gitbook/assets/integration.png">integration.png</a></td></tr><tr><td><strong>Webhook notifications</strong></td><td></td><td><a href="../../general-information/webhook-notifications.md">webhook-notifications.md</a></td><td><a href="../../.gitbook/assets/integration.png">integration.png</a></td></tr></tbody></table>
