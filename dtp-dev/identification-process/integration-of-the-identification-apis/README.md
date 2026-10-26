# Integration of the Identification APIs

## Schedule an identification case

The external application will schedule the identification process by calling a REST API. The response will return a caseId.

An identification case can be scheduled by sending a POST to the API /{tenantId}/id-cases/schedule. The request requires some mandatory data like personal information, the type of identity document, profileId, etc. Additional properties are also supported so that the tenant can have reference values inside the identification case.

For details, please refer to the OpenAPI specification included for Scheduled Cases. The response includes a scheduled case id so that the external application can use it to resume the scheduled case.

A scheduled case has an expiration date (can be specified). Once expired, it is not possible for the end user to do the identification process. By default, if isn’t specified, a scheduled case is valid for 6 months from its creation date.

### **Identity profile**

Upon the registration of the tenant, a default configuration of the identification process will be initialized.

It is also possible for Tenant to add additional configurations to meet with various use cases. Such additional configurations are called `profiles`. They can only be added by the provider and the tenant will be given the `id`s so that they can be specified when creating a scheduled identification case.

## Get identification case data and files of an identification case

Afterwards, the tenant application retrieves the full details of an identification case by calling the API `GET` `/{tenantId}/id-cases/{caseId}` as well as the artifacts (photos, videos, etc) by calling `GET` `/{tenantId}/id-cases/{caseId}/files.zip` The content of the identification case data and files is described in more detail in the chapter <\<case\_details>>.
