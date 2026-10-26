---
hidden: true
---

# Supported wallets

The DTP does not provide its own wallet app. The login VC and the EUDI Wallet authentication work with wallets that implement the EUDI Wallet standards listed below. The end user uses the wallet of their country.

## Wallet requirements

<table data-full-width="true"><thead><tr><th width="220">Capability</th><th width="280">Standard</th><th>Used for</th></tr></thead><tbody><tr><td><strong>Credential issuance</strong></td><td>OpenID for Verifiable Credential Issuance (OpenID4VCI), pre-authorized code flow with transaction code</td><td>Issuance of the login VC</td></tr><tr><td><strong>Credential presentation</strong></td><td>OpenID for Verifiable Presentations (OpenID4VP) with the High Assurance Interoperability Profile (HAIP), same-device and cross-device</td><td>Login with the login VC, PID presentation at enrolment</td></tr><tr><td><strong>Credential format</strong></td><td>SD-JWT VC (<code>dc+sd-jwt</code>)</td><td>Login VC. The PID is accepted as SD-JWT VC or ISO mdoc (<code>mso_mdoc</code>).</td></tr><tr><td><strong>Holder binding</strong></td><td>Key proof with a device-bound key</td><td>Ensures that only the enrolled wallet can present the login VC</td></tr><tr><td><strong>Revocation</strong></td><td>Token Status List</td><td>Revocation of the login VC</td></tr><tr><td><strong>Third-party attestations</strong></td><td>Acceptance of attestations (EAA) from issuers other than the PID provider</td><td>Storing the login VC in the wallet</td></tr></tbody></table>

{% hint style="warning" %}
**Third-party attestations.** Some national wallets will initially only accept attestations from registered or public issuers. A wallet can be suitable for the PID presentation but not (yet) for storing the login VC. In this case the [PID login](eudi-wallet-authentication-flow.md) can be used as fallback, if enabled in the authentication profile.
{% endhint %}

## Wallet overview

Status of the wallets based on public information as of **October 2026**. The EUDI Wallet landscape is changing quickly. Contact us for the current list of wallets tested with the DTP.

<table data-full-width="true"><thead><tr><th width="240">Wallet</th><th width="140">Country</th><th>Status</th></tr></thead><tbody><tr><td><strong>EUDI Wallet reference implementation</strong></td><td>EU</td><td>Open-source reference wallet of the European Commission (Android, iOS). Recommended for integration tests in the Sandbox environment.</td></tr><tr><td><strong>AltID</strong></td><td>Denmark</td><td>In production since 3 June 2026. First EUDI wallet in production.</td></tr><tr><td><strong>State EUDI Wallet</strong></td><td>Germany</td><td>Public sandbox since January 2026. Public rollout planned for early 2027.</td></tr><tr><td><strong>IT-Wallet</strong></td><td>Italy</td><td>Public sandbox available.</td></tr><tr><td><strong>France Identité</strong></td><td>France</td><td>National app in use, not yet certified as EUDI wallet. Public playground available.</td></tr><tr><td><strong>NL Wallet</strong></td><td>Netherlands</td><td>Developer resources available, no public sandbox yet.</td></tr><tr><td><strong>swiyu</strong></td><td>Switzerland</td><td>Not an EUDI wallet (Switzerland is not an EU member state). Wallet of the Swiss e-ID, based on OpenID4VC and SD-JWT VC. Launch of the e-ID postponed to the first half of 2027. Support by the DTP to be confirmed.</td></tr></tbody></table>

## Regulatory timeline

* By **end of 2026** every EU member state must provide at least one EUDI wallet to its citizens.
* By **end of 2027** regulated private services (e.g. banks) and very large online platforms must accept the EUDI wallet for strong user authentication.
