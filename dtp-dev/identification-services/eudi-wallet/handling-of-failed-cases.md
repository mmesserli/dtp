# Handling of failed cases

### Error handling during identification

The start page guides the end user to open the request in the EUDI Wallet (app link on mobile, QR code on desktop). In the EUDI Wallet the user sees which data is requested and by whom, and can approve or decline the request. If the end user can not proceed for some reason, the identification can be aborted on the start page.

Typical reasons for a failed identification:

* **No EUDI PID available** – the user has no EUDI Wallet installed or no PID credential in the wallet.
* **User declines the request** – the user does not approve sharing the requested data in the EUDI Wallet.
* **Invalid presentation** – the shared credential is expired, revoked or issued by an untrusted issuer.

### Retry page

If the identification failed, the user is shown the identification failed page and the status of the case is updated to IDENTIFICATION\_FAILED. The end user can click on a restart button on this page, to start over. In this case the status is updated to INITIALIZED again and the request is opened again in the EUDI Wallet.

In the webhook notification and in the API to retrieve the case details, in the fields ubiIdStatusReason and ubiIdStatusDetails additional details about the current status and why the identification failed can be found. More detail can be found in [EUDI Wallet status reason and details](eudi-wallet-status-reason-and-details.md).

### Errors to be handled by external application

* For each error code of ubiIdStatusReason "ABORTED" or "CANCELED" it can be configured in the profile on request:
  * if the end user should be navigated to the retry screen with a retry button (default behaviour)
  * if the end user should be navigated to the defined IDENTIFICATION\_FAILED redirect URL (can be setup on request), e.g. to offer an alternative identification method such as AutoIdent when no EUDI PID is available
  * if the end user should see a loading spinner so that the external application can close the iframe in which the identification is running (can be setup on request)
