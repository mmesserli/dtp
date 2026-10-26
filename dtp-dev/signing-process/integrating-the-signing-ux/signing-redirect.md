# Signing redirect

Redirecting to the specific URL feature is available at certain points in the identification or signing process. This section will show how to integrate the redirect feature.

## Redirect for different signing scenarios

The signing redirect can be configured for the following scenarios of the signing process:

| Redirect URL       | Signer Status                                                                        | Reason                                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------- |
| success            | `SIGNED`                                                                             | The signing process is successfully completed and no manual verification by the agent is needed.                    |
| failed             | `FAILED`                                                                             | The signing process is unable to proceed.                                                                           |
|                    | `OUTSTANDING`                                                                        | The verification of identification case associated with the signer is rejected or user reset password for Swisscom. |
| redoIdentification | <p><code>OUTSTANDING</code><br>(case status: <code>IDENTIFICATION_FAILED</code>)</p> | A new identification is required for the signining process.                                                         |
| rejected           | `REJECTED`                                                                           | The signing process has been rejected by the end user.                                                              |
| timeout            | `TIMEOUT`                                                                            | The signing session has been timed out                                                                              |

## Customize signing redirect URLs

Creating document-basket with the new signer evidence

```
curl --location 'https://{{URL}}/{{TENANT_ID}}/document-baskets' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{ TOKEN }}' \
--data-raw '{
    "ubiSignProfileId": {{ signProfileId }},
    "ubiIdConfiguration": {
        "ubiIdProfileId": {{ profileId }},
        "doIdentification": "ALWAYS"
    },
    "signers": {
        "signersEvidence": [
            {
                "firstName": "Mustermann",
                "lastName": "Erika",
                "mobileCountryCode": "41",
                "mobilePhoneNumber": "791234567",
                "extPersonId": "0f8cda3a-e84a-4332-b64c-08c948bf27e2",
                "email": "erika@mail.com",
                "identificationRedirectUrls": {
                    "success": "https://redirecturl.com?any_thing=abcXYZ",
                    "verificationRejected": "https://redirecturl.com?any_thing=abcXYZ",
                    "failed": "https://redirecturl.com?any_thing=abcXYZ",
                    "timeout": "https://redirecturl.com?any_thing=abcXYZ"
                },
                "signingRedirectUrls": {
                    "success": "https://redirecturl.com?any_thing=abcXYZ",
                    "rejected": "https://redirecturl.com?any_thing=abcXYZ",
                    "failed": "https://redirecturl.com?any_thing=abcXYZ",
                    "timeout": "https://redirecturl.com?any_thing=abcXYZ",
                    "resetPassword": "https://redirecturl.com?any_thing=abcXYZ"
                }
            }
        ]
    }
}'
```

OR overwrite `signingRedirectUrls` when creating document-basket with existing identification cases

```
curl --location 'https://{{URL}}/{{TENANT_ID}}/document-baskets' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{ TOKEN }}' \
--data-raw '{
    "ubiSignProfileId": {{ signProfileId }},
    "ubiIdConfiguration": {
        "ubiIdProfileId": {{ profileId }},
        "doIdentification": "ALWAYS"
    },
    "signers": {
      "signersFromExistingCases": [
        {
          "idCaseId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "signingRedirectUrls": {
            "success": "https://redirecturl.com?any_thing=abcXYZ",
            "rejected": "https://redirecturl.com?any_thing=abcXYZ",
            "failed": "https://redirecturl.com?any_thing=abcXYZ",
            "timeout": "https://redirecturl.com?any_thing=abcXYZ",
            "resetPassword": "https://redirecturl.com?any_thing=abcXYZ"
          }
        }
      ]
    }
}'
```

Overwriting redirect in case of signing failed and timeout when creating document-basket

```
curl --location 'https://{{URL}}/{{TENANT_ID}}/document-baskets' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{ TOKEN }}' \
--data-raw '{
    "ubiSignProfileId": {{ signProfileId }},
    "ubiIdConfiguration": {
        "ubiIdProfileId": {{ profileId }},
        "doIdentification": "ALWAYS"
    },
    "signers": {
        "signersEvidence": [
            {
                "firstName": "Mustermann",
                "lastName": "Erika",
                "mobileCountryCode": "41",
                "mobilePhoneNumber": "791234567",
                "extPersonId": "0f8cda3a-e84a-4332-b64c-08c948bf27e2",
                "email": "erika@mail.com",
                "signingRedirectUrls": {
                    "failed": "https://redirecturl.com?any_thing=abcXYZ",
                    "timeout": "https://redirecturl.com?any_thing=abcXYZ"
                }
            }
        ]
    }
}'
```

The redirect URLs in each scenario

```
// Signing successful: redirect with default value
https://example.com/success?additional=abc123&signerId=f17febc9-0ca1-44e0-8bcd-762d127578c4

// Signing failed: redirect with overwritten value
https://redirecturl.com/failed?any_thing=abcXYZ&signerId=f17febc9-0ca1-44e0-8bcd-762d127578c4

// Signing rejected: redirect with default value
https://example.com/rejected?additional=abc123&signerId=f17febc9-0ca1-44e0-8bcd-762d127578c4

// Signing session timeout: redirect with overwritten value
https://redirecturl.com/timeout?any_thing=abcXYZ&signerId=f17febc9-0ca1-44e0-8bcd-762d127578c4

// Reset password: If we are using redoIdentification( user select reset password) with external application flow, we will redirect with overwritten value
https://redirecturl.com/reset-password?any_thing=abcXYZ&signerId=f17febc9-0ca1-44e0-8bcd-762d127578c4
```

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
If the parameter is configured in signing profile to add to the redirect URL, customized redirect URLs must not include the configured parameter
{% endhint %}

## Default configuration

The redirect after the signing process feature is disabled by default and could be enabled by the configuration in signing profile.

NOTE: The configuration belongs to the signing profile and will be configured by seal.ID admin.

A default redirect url can be defined for each of the scenarios in the signing profile. If the default URL value is not provided in the configuration, customize redirect URL will be required when creating the scheduled case.

## Automatic parameter attachment to redirect URLs

The following parameters can be configured to be attached as html parameters, to the redirect url.

* `signerId`: ID of signer
* `ubiIdCaseId`: ID of identification seal.ID case
* `extCaseId`: External ID reference to seal.ID case that can be set when the seal.ID case is created
* `extPersonId`: External ID reference to the person of seal.ID case that can be set when the seal.ID case is created
* `extApplication`: Name of external application that can be set when the seal.ID case is created
* `ubiIdCategory`: category of seal.ID case that can be set when the seal.ID case is created

IMPORTANT: If the any of the above parameters has no valid value it will be omitted in the URL when redirecting.

NOTE: Additional html parameters defined by the customer can be used in the defined url.

IMPORTANT: If the parameter is configured in id & signing profile to add to the redirect URL, default redirect URLs must not include the configured parameter.

## Security considerations for a secure redirect implementation

Using Redirect also means your software system will have to expose an HTTP endpoint to accept the requests containing the parameters from which MAY be used to fetch the information of the End Users.

Consider the following scenario: a tenant uses only `extCaseId` in the scenario of Identification Success. If the tenant's web application simply loads the corresponding information of `extCaseId` and responds back to any requests, such an endpoint may be exploited by malicious users to fetch others' data.

To mitigate that, the provider advises tenants to employ some tracking capability or entropy into the Redirect URL such that tenants can validate if a request at the redirect endpoint is legitimate.

A Tenant MAY employ an additional parameter in (each) Redirect URL, for example, state, such that the Redirect URL becomes 'https://example.org/redirect-frommideal?state=vMcIaRQy7dKCmDS'. The value of state may be a cryptographically generated one-time token or it could be a managed reference in the tenant’s database. Tenant SHOULD generate and associate the state with the End User’s session before redirecting them to application. The idea is that the application will always reserve the existing parameters in the URL to allow the tenant to validate the state upon redirect back.
