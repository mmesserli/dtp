# AutoIdent (Intrum)

## AutoIdent specific integration documents

{% hint style="success" %}
All specific integration information for AutoIdent (Intrum) you can find in the following documents:
{% endhint %}

{% content-ref url="identification-services/autoident-intrum/status-logic.md" %}
[status-logic.md](identification-services/autoident-intrum/status-logic.md)
{% endcontent-ref %}

{% content-ref url="identification-services/autoident-intrum/status-reason-and-details.md" %}
[status-reason-and-details.md](identification-services/autoident-intrum/status-reason-and-details.md)
{% endcontent-ref %}

{% content-ref url="identification-services/autoident-intrum/identification-files.md" %}
[identification-files.md](identification-services/autoident-intrum/identification-files.md)
{% endcontent-ref %}

{% content-ref url="identification-services/autoident-intrum/handling-of-failed-cases.md" %}
[handling-of-failed-cases.md](identification-services/autoident-intrum/handling-of-failed-cases.md)
{% endcontent-ref %}

{% content-ref url="identification-services/autoident-intrum/verification-canceled-flow.md" %}
[verification-canceled-flow.md](identification-services/autoident-intrum/verification-canceled-flow.md)
{% endcontent-ref %}

{% content-ref url="identification-services/autoident-intrum/identification-with-intrum-sdk.md" %}
[identification-with-intrum-sdk.md](identification-services/autoident-intrum/identification-with-intrum-sdk.md)
{% endcontent-ref %}

{% content-ref url="identification-services/autoident-intrum/testing.md" %}
[testing.md](identification-services/autoident-intrum/testing.md)
{% endcontent-ref %}

## General integration guide for identification

{% hint style="success" %}
Please check the general integration documents to understand how to integrate the identification process.
{% endhint %}

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>General Information</strong></td><td></td><td><a href="general-information/api-authentication.md">api-authentication.md</a></td><td><a href=".gitbook/assets/integration.png">integration.png</a></td></tr><tr><td><strong>Integrating the identification process</strong></td><td></td><td><a href="identification-process/flow-of-integrating-the-identification-process.md">flow-of-integrating-the-identification-process.md</a></td><td><a href=".gitbook/assets/integration.png">integration.png</a></td></tr></tbody></table>

## OpenAPI specification

There is an option in the API to create a document-basket and the connected identification case(s) in one call by including the identification data in the "signersEvidence" object. Please check the OpenAPI specification for more details.

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>Open-API specification</strong></td><td><a href="/broken/pages/tfSiXZlgUxdLydV0a0XC">Broken link</a></td><td><a href=".gitbook/assets/API.png">API.png</a></td></tr></tbody></table>

## Webhook notifications

The external application will be notified about all status updates on 2 separate webhook URLs, one for the identification case, one for the document basket. There are separate APIs for identification and signing to collect data and files.

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>Integrating webhook notifications</strong></td><td><a href="general-information/webhook-notifications.md">webhook-notifications.md</a></td><td><a href=".gitbook/assets/integration.png">integration.png</a></td></tr><tr><td><strong>Identification webhook API specification</strong></td><td><a href="/broken/pages/nLdKaNW4aWu6p6RoZLSV">Broken link</a></td><td><a href=".gitbook/assets/API.png">API.png</a></td></tr></tbody></table>
