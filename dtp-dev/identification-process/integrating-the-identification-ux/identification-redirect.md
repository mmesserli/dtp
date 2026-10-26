# Identification redirect

Redirecting to the specific URL feature is available at certain points in the identification or signing process. This section will show how to integrate the redirect feature.

## Redirect for different identification scenarios

The identification redirect can be configured for the following scenarios of the identification process:

| Redirec URL                    | Identification Case Status  | Reason                                                                                                                          |
| ------------------------------ | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| success                        | `IDENTIFICATION_SUCCESSFUL` | The identification process is successfully completed and no manual verification by the agent is needed.                         |
|                                | `VERIFICATION_PENDING`      | The identification process is successfully completed and a manual verification is pending which will be performed by the agent. |
|                                | `VERIFICATION_CONFIRMED`    | The identification process is successfully completed and verified by the agent.                                                 |
| failed                         | `IDENTIFICATION_FAILED`     | The user aborted the identification process manually.                                                                           |
| verificationRejected           | `VERIFICATION_REJECTED`     | The identification result has been rejected by the agent.                                                                       |
| Identification session timeout | `TIMEOUT`                   | The identification session has been timed out. Per default the timeout feature is deactivated.                                  |

NOTE: If the `smart link` feature is used to associate the signing process seamlessly, end user will be redirected to the signing flow instead of the external application when identification is successful.

## Customize redirect URLs

The default redirect URL of the profile configuration can be overwritten when creating an identification case or when creating a document-basket.

`identificationRedirectUrls` & `signingRedirectUrls` properties when creating the document basket using the API `POST /{tenantId}/document-baskets`

{% hint style="warning" %}
The redirect URL if provided must comply with [RFC-3986.](https://www.rfc-editor.org/rfc/rfc3986) The maximum total length of the redirect URL value is 1024 characters.
{% endhint %}

Example of valid redirect URL value:

```
"url": "https://example.com/success"
"url": "https://example.com/success?additional=abc123&other=abcXYZ"
```

Example of invalid redirect URL value:

<pre><code>"url": "www.example.com/main"
"url": "https://example.com/ invalid-%%$\ zzz"
<strong>"url": "https://example.com/success?ubiIdCaseId=abc123"
</strong></code></pre>

{% hint style="warning" %}
The redirect URL overwriting only allow when the configuration on a specific scenario must be enabled. If overwriting the redirect URL is included in the request body but the configuration of the specific scenario is disabled, the error 400 Bad Request will be returned.
{% endhint %}

{% hint style="info" %}
Redirect URL value will be built and returned in the response based on configurations and overwriting value.
{% endhint %}

{% hint style="warning" %}
If the parameter is configured in identification or signing profile to be added to the redirect URL, customized redirect URLs must not include the configured parameter
{% endhint %}

## Automatic parameter attachment to redirect URLs

The following parameters can be configured to be attached as html parameters, to the redirect URLs.

* `ubiIdCaseId`: ID of identification seal.ID case
* `extCaseId`: External ID reference to seal.ID case that can be set when the seal.ID case is created
* `extPersonId`: External ID reference to the person of seal.ID case that can be set when the seal.ID case is created
* `extApplication`: Name of external application that can be set when the seal.ID case is created
* `ubiIdCategory`: category of seal.ID case that can be set when the seal.ID case is created

## Default configuration

The redirect after the identification process feature is disabled by default and could be enabled by the configuration in id profile.

NOTE: The configuration belongs to the identity profile and will be configured by seal.ID admin.

A default redirect url can be defined for each of the scenarios in the identification profile. If the default URL value is not provided in the configuration, customize redirect URL will be required when creating the scheduled case.

## Security considerations for a secure redirect implementation

Using Redirect also means your software system will have to expose an HTTP endpoint to accept the requests containing the parameters from which MAY be used to fetch the information of the End Users.

Consider the following scenario: a tenant uses only `extCaseId` in the scenario of Identification Success. If the tenant's web application simply loads the corresponding information of `extCaseId` and responds back to any requests, such an endpoint may be exploited by malicious users to fetch others' data.

To mitigate that, the provider advises tenants to employ some tracking capability or entropy into the Redirect URL such that tenants can validate if a request at the redirect endpoint is legitimate.

A Tenant MAY employ an additional parameter in (each) Redirect URL, for example, state, such that the Redirect URL becomes 'https://example.org/redirect-frommideal?state=vMcIaRQy7dKCmDS'. The value of state may be a cryptographically generated one-time token or it could be a managed reference in the tenant’s database. Tenant SHOULD generate and associate the state with the End User’s session before redirecting them to application. The idea is that application will always reserve the existing parameters in the URL to allow the tenant to validate the state upon redirect back.
