# AutoIdent (Intrum) verification canceled flow

After the identification an agent performs a manual review of the identification.

If the identification verification is canceled by the agent because the quality of the scanned result is not sufficient, the case will be updated to IDENTIFICATION\_FAILED. In this case the enduser is shown the identification failed page. The enduser can click on a restart button on this page, to start over. In his case the status is updated to INITIALIZED again.

<figure><img src="../../.gitbook/assets/screen2.png" alt="" width="215"><figcaption></figcaption></figure>

If the identification is rejected by the agent due to fraud reasons, the case status will be updated to VERIFICATION\_REJECTED. In this scenario the enduser will not be able to start the identification case again.

Example verification is canceled flow when the end-user using smart-link to start identification and signing process

```mermaid
graph TD
    A[Start by smart-link] --> B[Do Identification]
    B --> C[Identification scan successful?]
    C -->|Yes| D[Manual verification by agent?]
    C -->|No| E[IDENTIFICATION_FAILED] 
    D -->|No| F[IDENTIFICATION_SUCCESSFUL] --> G[Proceed signing documents] --> END((END))
    D -->|Yes| H[VERIFICATION_PENDING]
    E --> O[Restart Identification?]
    H --> I[Verification successful]
    I -->|Yes| J[VERIFICATION_CONFIRMED] --> K[Sign documents] --> END2((END))
    I -->|No| L[Verification is cancelled and end-user is allowed to retry] 
    L -->|No| N[VERIFICATION_REJECTED] --> END3((END))
    L -->|Yes| M[IDENTIFICATION_FAILED]
    M --> O
    O --> B
```
