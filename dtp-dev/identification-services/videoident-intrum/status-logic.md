# VideoIdent (Intrum) status logic

```mermaid
stateDiagram-v2
     [*] --> SCHEDULED
SCHEDULED --> INITIALIZED: User starts
INITIALIZED --> IDENTIFICATION_FAILED: NOK
INITIALIZED --> IDENTIFICATION_SUCCCESSFUL: OK
    IDENTIFICATION_SUCCCESSFUL--> DOWNLOADED: Download OK
    IDENTIFICATION_SUCCCESSFUL--> CORRUPTED: Download NOK
    IDENTIFICATION_FAILED --> INITIALIZED: Retry
    VERIFICATION_REJECTED--> DOWNLOADED: Download OK
    VERIFICATION_REJECTED--> CORRUPTED: Download NOK
    CORRUPTED --> DOWNLOADED: OK
    INITIALIZED  --> VERIFICATION_REJECTED:  NOK Fraud
SCHEDULED --> DELETED_MARKED
INITIALIZED --> DELETED_MARKED
IDENTIFICATION_FAILED --> DELETED_MARKED
VERIFICATION_REJECTED --> DELETED_MARKED
DOWNLOADED --> DELETED_MARKED
CORRUPTED --> DELETED_MARKED
DELETED_MARKED --> DELETED

```

On every status update a notification is sent to the external application. With the provided API's a case can be scheduled and the details of a identification case and the files can be downloaded.

<table data-full-width="true"><thead><tr><th width="317">Status</th><th>Description</th></tr></thead><tbody><tr><td>SCHEDULED</td><td><ul><li>The identification case is scheduled. the available personal data is posted to the identification application.</li><li>A startlink is created and a unique identification case id created.</li></ul></td></tr><tr><td>INITIALIZED</td><td><ul><li>The user has started the video sequence</li><li>If the user clicks on the "Retry" button, the INITIALIZED notification is sent again after the video sequence is restarted.</li></ul></td></tr><tr><td>IDENTIFICATION_FAILED</td><td><ul><li>The user could not complete the video sequence.</li><li>The user is taken to an error page and can start the video sequence again.</li><li>This notification is sent for each failed identification. If the same case also fails after a retry, IDENTIFICATION_FAILED is sent again.</li></ul></td></tr><tr><td>VERIFICATION_REJECTED</td><td><ul><li>The agent has rejected the identification due to fraud.</li></ul></td></tr><tr><td>IDENTIFICATION_FAILED</td><td>The status can be updated to IDENTIFICATION_FAILED again after status INITIALIZED if a problem occurred in the identification.</td></tr><tr><td>DOWNLOADED / CORRUPTED</td><td><p>GetCase details and download identification zip file</p><ul><li>By an API the case details can be get and a zip file with all identification files including an identification report downloaded</li><li>For the file download a notification is sent if the provided zip file could be generated successfully (status DOWNLOADED) or the zip file could not be generated completely (status CORRUPTED)</li><li><p>Behavior when the download could be done successful</p><ul><li>the status is DOWNLOADED</li><li>the zip file contains the video file and the identification report as well</li></ul></li><li><p>Behavior when the provided zip file does not contain all information</p><ul><li>the status is CORRUPTED</li><li>status is set when the video file is missing or the identification report could not be generated successfully</li><li>when the external application receives the status CORRUPTED the download must be triggered again (B). when now the zip could be generated completely the status will be set to DOWNLOADED</li></ul></li></ul></td></tr><tr><td>DELETED_MARKED</td><td><ul><li>A case will be set to status DELETED_MARKED once the configured days for deletion have passed.</li></ul></td></tr><tr><td>DELETED</td><td><ul><li>A case will be set to status DELETED after the final deletion of a case, which is triggered 30 days after marking it as DELETED MARKED.</li></ul></td></tr></tbody></table>
