# AutoIdent (Intrum) testing

## Available testbots for automated case handling

In the Sandbox environment, there are a number of testbots available for testing purposes. They can be triggered by adding the corresponding keywords in the fields for "firstName" or "lastName". Capitalization is not being regarded which means that the trigger can be set either through "xtesthappy", "Xtesthappy" or "XTESTHAPPY".

Using testbots also allows testers to use fabricated identification documents to go through the process.

{% hint style="info" %}
Testbots are only available for the standard seal.ID process. If an additional step of scanning the **residence permit** or scanning the **signature page of the Swiss Passport** is active, the testbots are not supported. Please open a support ticket to arrange a test session in which agents will review your test cases.
{% endhint %}

<table data-full-width="true"><thead><tr><th width="197">Keyword</th><th width="215">Effect on the verification</th><th width="244">Identification status</th><th>Status reason</th></tr></thead><tbody><tr><td>xtesthappy</td><td>Automatical verification</td><td>VERIFICATION_CONFIRMED</td><td>SUCCESS_DATA_CHANGED</td></tr><tr><td>xtestfraud</td><td>Fraud case confirmed</td><td>VERIFICATION_REJECTED</td><td>FRAUD_SUSPICION_CONFIRMED</td></tr><tr><td>xtestcanceled</td><td>Reject – Cancel flow</td><td>IDENTIFICATION_FAILED</td><td>CANCELED</td></tr></tbody></table>

#### xtesthappy

Triggers the confirmation of the verification and can be used to go through the identification/verification process and proceed to the signing.

Identification status-flow:

* SCHEDULED
* INITIALIZED
* VERIFICATION\_PENDING
* VERIFICATION\_CONFIRMED

#### xtestfraud

Triggers the rejection of the verification and can be used to simulate a potential fraud case. A rejected case cannot be restarted, the status "VERIFICATION\_REJECTED" is final.

Identification status-flow:

* SCHEDULED
* INITIALIZED
* VERIFICATION\_PENDING
* VERIFICATION\_REJECTED
  * ```json
    "ubiIdStatusDetails": "WARNING_MANIPULATED_PHOTO"
    ```

#### xtestcanceled

Triggers the cancellation of the identification with the reason "ID\_BLURRY" and can be used go through the identification/verification process and proceed to the signing.

{% hint style="info" %}
In case that the Bot for the cancellation flow is being set through "xtestcanceled", the Status will switch to IDENTIFICATION\_FAILED at which point the enduser is being presented with the option to retry the flow. If a retry is being done, the case will end up in IDENTIFICATION\_FAILED again since the initial values will not change and the cancellation bot is triggered for each retry.
{% endhint %}

Identification status-flow:

* SCHEDULED
* INITIALIZED
* VERIFICATION\_PENDING
* IDENTIFICATION\_FAILED
  * ```json
        "ubiIdStatusReason": "CANCELED",
        "ubiIdStatusDetails": "ID_BLURRY"
    ```
