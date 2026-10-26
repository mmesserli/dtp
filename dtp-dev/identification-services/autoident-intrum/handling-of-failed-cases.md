# Handling of failed cases

### Error handling and instructions during identification

During the identification the AutoIdent will give instructions to the end user on how to scan the document and do the selfie. If a problem occurs the end user will be guided on how to resolve it. If the end user can not proceed for some reason, an X on the top left can be clicked and the end user can select an abort reason.

{% tabs %}
{% tab title="Allow camera access" %}
<div><figure><img src="../../.gitbook/assets/image (3).png" alt="" width="213"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (5) (1).png" alt="" width="171"><figcaption></figcaption></figure></div>
{% endtab %}

{% tab title="Camera not accessible" %}
<figure><img src="../../.gitbook/assets/image (2).png" alt="" width="215"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Document not readible" %}
<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="211"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Document not allowed" %}
<figure><img src="../../.gitbook/assets/image (5).png" alt="" width="306"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="User Aborts" %}
<figure><img src="../../.gitbook/assets/image (6).png" alt="" width="213"><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### Retry page

During the identification hints are given to the end user how to successfully finish the identification. For some cases the user is shown the identification failed page and the status of the case is updated to IDENTIFICATION\_FAILED. The end user can click on a restart button on this page, to start over. In his case the status is updated to INITIALIZED again.

<figure><img src="../../.gitbook/assets/screen1 (1).png" alt="" width="211"><figcaption></figcaption></figure>

In the webhook notification and in the API to retrieve the case details, in the fields ubiIdStatusReason and ubiIdStatusDetails additional details about the current status and why the end user aborted can be found. More detail can be found in [AutoIdent (Intrum) status reason and details](status-reason-and-details.md).

### Errors to be handled by external application

* For each error code of ubiIdStatusReason "ABORTED" (see [AutoIdent (Intrum) status logic](status-logic.md)) it can be configured in the profile on request:
  * if the end user should be navigated to the retry screen with a retry button (default behaviour)
  * if the end user should be navigated to the defined IDENTIFICATION\_FAILED redirect URL (can be setup on request)
  * if the end user should see a loading spinner so that the external application can close the iframe in which the identification is running (can be setup on request)
