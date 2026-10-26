# EUDI Wallet files

## Get identification case files

By sending a `GET` to the API `/{tenantId}/id-cases/{caseId}/files.zip` a zip file is downloaded containing the files. Depending on the data shared from the EUDI Wallet some of the files are optional:

<table data-full-width="true"><thead><tr><th width="339">Filename</th><th>Explanation</th><th>Note</th></tr></thead><tbody><tr><td><code>face_{caseId}.jpg</code></td><td>Portrait of the user as contained in the PID</td><td>Optional, only available if the portrait attribute was requested and shared</td></tr><tr><td><code>presentation_{caseId}.json</code></td><td>The verifiable presentation as received from the EUDI Wallet (SD-JWT VC or mdoc) including validation result</td><td>Suitable as technical evidence</td></tr><tr><td><code>idData_{caseId}.json</code></td><td>JSON containing all PID attributes gathered during the identification process</td><td></td></tr><tr><td><code>idData_{caseId}.xml</code></td><td>idData in xml format</td><td></td></tr><tr><td><code>ubiIdReport.pdf</code></td><td>PDF report containing all relevant information of the identification</td><td>This pdf is suitable for archiving purpose</td></tr></tbody></table>

NOTE: The available response MIME types include `application/octet-stream` and `application/json`, these can be obtained by using `Accept` or `Accept-Override` request headers with the appropriate MIME type. For details, please refer to the OpenAPI specification included for Scheduled Cases.
