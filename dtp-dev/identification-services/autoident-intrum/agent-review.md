# AutoIdent (Intrum) agent review

## Main identification document

{% hint style="info" %}
For the identification for seal.ID the enduser has to identify with a [supported document](https://docs.mideal.io/service-description/m_ideal-service-overview/onboarding-services/seal.id/seal.id-supported-countries-and-id-documents). In a manual verification an agent verifies below listed points.
{% endhint %}

### Comparison of transmitted data <a href="#comparison-of-transmitted-data" id="comparison-of-transmitted-data"></a>

* Except for spelling mistakes, nothing may be corrected
* Additional names may be added
* Date of birth must match

### Photo quality <a href="#photo-quality" id="photo-quality"></a>

#### Selfie <a href="#selfie.1" id="selfie.1"></a>

* Biometric security features visible
* No shadow on the face
* No other person in the picture

#### ID document <a href="#id-document" id="id-document"></a>

* Document is supported
* Entire document visible
* No reflections
* All data is legible
* Valid
* Not damaged

#### Photo comparison (selfie and ID) <a href="#photo-comparison-selfie-and-id" id="photo-comparison-selfie-and-id"></a>

* Clearly the same person
* Security features
* Clearly recognizable

## Additional document: Residence permit

{% hint style="info" %}
seal.ID allow to activate an additional step to scan / upload the front page of the residence permit. If this step is activated an agent verifies below listed points. The review result of the residence permit has no impact on the overall case status.
{% endhint %}

### Acceptance of residence permit

* ID card type: B, C or Ci
* Same person as in AutoIdent (name and person are the same)
* Person on photo is clearly recognizable
* All fields can be filled in

### Returned fields

* expiryDate:
  * Expiration date of residence permit
* status:
  * OK: Residence permit was accepted
  * NOK: Residence permit was rejected
* subtype :
  * subtype of ID-Card

Sample of returned result:

```
{
	"additionalDocuments": [
		{
			"type": "RESIDENCE_PERMIT",
			"status": "OK",
			"subType": "B",
			"expiryDate": "2029-10-09"
		}
	]
}
```
