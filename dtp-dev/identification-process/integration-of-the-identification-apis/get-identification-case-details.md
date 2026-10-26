# Get identification case details

The external application can get the details of a identification case by calling a REST API. An API to get the identification data is available as well as an API to get a zip file which includes all files and gathered information of the identification process.

## Get identification case data

By sending a `GET` to the API `/{tenantId}/id-cases/{caseId}` the details of a identification case can be retrieved.

The response of the API will include data as

* general identification case information: general information as caseId and status
* initial person data: data entered by user during data gathering respectively posted by API `/{tenantId}/id-cases/schedule`
* autoID data: identification data as extracted information from document, document features
* verification data: data from manual verification by an Agent

NOTE: For details, please refer to the OpenAPI specification included: api-scheduled-cases.

In the case data collected per API the response includes the element "identificationDocument" which is an object containing data of identification process.

```
"identificationDocument": {
		"classification": "IDCARD",
		"type": null,
		"issuer": null,
		"issuingDate": "2010-10-12",
		"expiryDate": "2020-10-11",
		"documentNumber": "123456789",
		"primaryName": "TEST",
		"secondaryName": "X-MANUALTEST-HAPPYPATH",
		"birthdate": "1995-02-06",
		"sex": null,
		"nationality": "CHE",
		"personalNumber": null,
		"issuerCountry": "DEU",
		"issuedby": "ISSUER",
		"driverlicenses": null
	}
```
