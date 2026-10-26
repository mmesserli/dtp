# EUDI Wallet

To identify a person with the **EUDI Wallet** (European Digital Identity Wallet the identification case is scheduled via API, the end user opens the startlink and the external application is informed about every status change via webhook.

In the EUDI Wallet the end user reviews the requested Person Identification Data (PID) and approves or declines the request. The case status is then directly updated to:

* `IDENTIFICATION_SUCCESSFUL` – the PID was shared and successfully validated
* `IDENTIFICATION_FAILED` – no EUDI PID is available, or the user declines access to the request

## EUDI Wallet specific integration documents

{% hint style="success" %}
All specific integration information for EUDI Wallet you can find in the following documents:
{% endhint %}

{% content-ref url="eudi-wallet-status-logic.md" %}
[eudi-wallet-status-logic.md](eudi-wallet-status-logic.md)
{% endcontent-ref %}

{% content-ref url="eudi-wallet-status-reason-and-details.md" %}
[eudi-wallet-status-reason-and-details.md](eudi-wallet-status-reason-and-details.md)
{% endcontent-ref %}

{% content-ref url="eudi-wallet-files.md" %}
[eudi-wallet-files.md](eudi-wallet-files.md)
{% endcontent-ref %}

{% content-ref url="handling-of-failed-cases.md" %}
[handling-of-failed-cases.md](handling-of-failed-cases.md)
{% endcontent-ref %}

## General integration guide for identification

{% hint style="success" %}
Please check the general integration documents to understand how to integrate the identification process.
{% endhint %}

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>General Information</strong></td><td></td><td><a href="../../general-information/api-authentication.md">api-authentication.md</a></td><td><a href="../../.gitbook/assets/integration.png">integration.png</a></td></tr><tr><td><strong>Integrating the identification process</strong></td><td></td><td><a href="../../identification-process/flow-of-integrating-the-identification-process.md">flow-of-integrating-the-identification-process.md</a></td><td><a href="../../.gitbook/assets/integration.png">integration.png</a></td></tr></tbody></table>

## Webhook notifications

The external application will be notified about all status updates on 2 separate webhook URLs, one for the identification case, one for the document basket. There are separate APIs for identification and signing to collect data and files.

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>Integrating webhook notifications</strong></td><td><a href="../../general-information/webhook-notifications.md">webhook-notifications.md</a></td><td><a href="../../.gitbook/assets/integration.png">integration.png</a></td></tr></tbody></table>
