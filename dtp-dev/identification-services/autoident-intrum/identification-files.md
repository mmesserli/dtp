# AutoIdent (Intrum) files

## Get identification case files

By sending a `GET` to the API `/{tenantId}/id-cases/{caseId}/files.zip` a zip file is downloaded containing the files. Depending on the used identification document and the identification level some of the files are optional:

<table data-full-width="true"><thead><tr><th width="339">Filename</th><th>Explanation</th><th>Note</th><th>Available for</th></tr></thead><tbody><tr><td><code>front_{caseId}.jpg</code></td><td>Front side of identification document</td><td>Only available for documents with two sides, for example the Swiss identity card</td><td>All use cases</td></tr><tr><td><code>back_{caseId}.jpg</code></td><td>Back side of identification document</td><td>only available for documents with two sides, for example the Swiss identity card</td><td>All use cases</td></tr><tr><td><code>security[123]_{caseId}.jpg</code></td><td>The images taken from the security features of the document (holograms, etc.)</td><td>Optional, since in some cases, no photo of security features can be taken during scan process</td><td>All use cases except ID-Scan without security feature</td></tr><tr><td><code>face_{caseId}.jpg</code></td><td>Photo of taken selfie</td><td></td><td>All use cases</td></tr><tr><td><code>liveness_left_{caseId}.jpg</code></td><td>photo taken during liveness check</td><td></td><td>All use cases except ID-Scan</td></tr><tr><td><code>residence_permit_{caseId}.jpg</code></td><td>photo of additional document if activated in the profile</td><td>E.g.: Photo of residence permit for seal.ID non-ch flow</td><td>All use cases with residence permit capture</td></tr><tr><td><code>idData_{caseId}.json</code></td><td>json containing all information gathered during the auto identification process</td><td></td><td>All use cases</td></tr><tr><td><code>idData_{caseId}.xml</code></td><td>idData in xml format</td><td></td><td>All use cases</td></tr><tr><td><code>verification_{caseId}.json</code></td><td>JSON contains all information gathered during the verification process</td><td>Only available for seal.ID when the verification already took place</td><td>All use cases with verification</td></tr><tr><td><code>verification_{caseId}.xml</code></td><td>Verification data in xml format</td><td>Only available for seal.ID when the verification already took place</td><td>All use cases with verification</td></tr><tr><td><code>ubiIdReport.pdf</code></td><td>Pdf report containing all relevant information of identification and verification steps</td><td>This pdf is suitable for archiving purpose</td><td>All use cases</td></tr><tr><td><code>{caseId}.mp4</code></td><td>The videolog of the identification process</td><td></td><td>VideoIdent only</td></tr><tr><td><code>{caseId}.mp3</code></td><td>The audiolog of the identification process</td><td></td><td>VideoIdent only</td></tr><tr><td><code>intrumReport.pdf</code></td><td>Intrum pdf report containing the identification data</td><td></td><td>All use cases</td></tr><tr><td><code>original_front_{caseId}.jpg</code></td><td>Original front side of identification document that aren't cropped.</td><td></td><td>All use cases</td></tr><tr><td><code>original_back_{caseId}.jpg</code></td><td>Original back side of identification document that aren't cropped</td><td></td><td>All use cases</td></tr></tbody></table>

Note: For details, please refer to the OpenAPI specification included for Scheduled Cases

* `front_{caseId}.jpg`: front side of identification document (only available for documents with two sides, for example the Swiss identity card)
* `back_{caseId}.jpg`: back side of identification document (only available for documents with two sides, for example the Swiss identity card)
* `security[123]_{caseId}.jpg`: The images taken from the security features of the document (holograms, etc.). Optional, since in some cases, no photo of security features can be taken during scan process
* `face_{caseId}.jpg`: photo of taken selfie
* `liveness_left_{caseId}.jpg`: photo taken during liveness check
* `residence_permit_{caseId}.jpg`: photo of additional document if activated in the profile (e.g. photo of residence permit for seal.ID non-ch flow)
* `idData_{caseId}.json`: json contains all information gathered during the auto identification process
* `idData_{caseId}.xml`: idData in xml format
* `verification_{caseId}.json`: json contains all information gathered during the verification process (only available for seal.ID when the verification already took place)
* `verification_{caseId}.xml`: verification in xml format (only available for seal.ID when the verification already took place)
* `ubiIdReport.pdf`: pdf report containing all relevant information of identification and verification steps. pdf is suitable for archiving purpose
* `{caseId}.mp4`: The videolog of the identification process to {caseId}.mp4: The videolog of the identification process (videoIdent only)
* `{caseId}.mp3`: The audiolog of the identification process. (videoIdent only)
* `intrumReport.pdf`: intrum pdf report containing the identification data
* `original_front_{caseId}.jpg`: original front side of identification document that aren't cropped.
* `original_back_{caseId}.jpg`: original back side of identification document that aren't cropped.

NOTE: The available response MIME types include `application/octet-stream` and `application/json`, these can be obtained by using `Accept` or `Accept-Override` request headers with the appropriate MIME type. For details, please refer to the OpenAPI specification included for Scheduled Cases.
