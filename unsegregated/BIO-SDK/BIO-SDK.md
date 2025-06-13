# Earlier Content

# Biometric SDK

## Overview

The BioSDK diagram illustrates how MOSIP integrates biometric devices for registration and authentication. It shows the flow of biometric data from capture devices through the Secure Biometric Interface (SBI) to the BioSDK, which performs quality checks, deduplication, and operator authentication.&#x20;

For registration, signed biometrics are sent to the MOSIP backend, where the BioSDK ensures data quality, ABIS handles deduplication, and ID Auth manages 1:1 matching. During authentication, encrypted biometrics are verified via the authentication app. Device management, including registration, deregistration, and key rotation, is handled by the Device Management Server.

![Biometric Data Flow](../../docs/.gitbook/assets/sdk.png)

## Applications

* Registration Client.
* Backend quality check.
* Biometric authentication during onboarding (internal auth).
* ID Authentications.

## Biometric SDK library

The library is used by the [Registration Client](../../docs/id-lifecycle-management/identity-issuance/registration-client) to perform 1:N match, segmentation, extraction, etc. For more information on integration with Registration Client, refer to the [Registration Client Biometric SDK Integration Guide](../../docs/registration-client-sdk-integration.md).

A simulation of this library is available as [Mock BioSDK](https://github.com/mosip/mosip-mock-services/tree/release-1.2.0/mock-sdk). The same is installed in the [MOSIP sandbox](../../docs/readme/technology/sandbox-details.md).

## Biometric SDK service

For 1:1 match and quality check of biometrics at the MOSIP backend, the BioSDK must be running as a service that can be accessed by [Registration Processor](../../docs/id-lifecycle-management/identity-issuance/registration-processor) and [IDA Internal Services](../../docs/id-lifecycle-management/identity-verification/id-authentication-services#internal-services). The service exposes REST APIs specified [here](../../docs/id-lifecycle-management/supporting-components/biometrics/biometric-sdk.md#api).

A [simulation (mock) service](https://github.com/mosip/biosdk-services/tree/release-1.2.0) has been provided. The mock service loads [mock BioSDK](https://github.com/mosip/mosip-mock-services/tree/release-1.2.0/mock-sdk) internally on the startup and exposes the endpoints to perform 1:N match, segmentation, and extraction as per [IBioAPI](https://github.com/mosip/commons/blob/release-1.2.0/kernel/kernel-biometrics-api/src/main/java/io/mosip/kernel/biometrics/spi/IBioApi.java).

The service may be packaged as a docker running inside the [MOSIP Kubernetes cluster](https://github.com/mosip/mosip-infra/blob/release-1.2.0/deployment/v3/cluster/README.md) or running separately on a server. The scalability of this service must be taken care of depending on the load on the system, i.e., the rate of enrolment and ID authentication.


## Testing kit

BioSDK server request/response may be tested using the [BioSDK testing kit](https://github.com/mosip/biosdk-testing-kit.git).

## Configuration

The following properties in [`application-default.properties`](../../docs/id-lifecycle-management/supporting-components/biometrics/biometric-sdk.md) needs to be updated to integrate the BioSDK library and service with MOSIP.

```properties
mosip.fingerprint.provider=io.mosip.kernel.bioapi.impl.BioApiImpl
mosip.face.provider=io.mosip.kernel.bioapi.impl.BioApiImpl
mosip.iris.provider=io.mosip.kernel.bioapi.impl.BioApiImpl
mosip.ida.biosdk-service.url=http://mock-biosdk-service.default:80
mosip.regproc.biosdk-service.url=http://mock-biosdk-service.default:80
mosip.idrepo.biosdk-service.url=http://mock-biosdk-service.default:80
```


## API

1. BioSDK library: [IBioAPIV2](https://github.com/mosip/bio-utils/blob/4a708ba24e9553dc187ecae468b07987744431c8/kernel-biometrics-api/src/main/java/io/mosip/kernel/biometrics/spi/IBioApiV2.java#L15)
2. BioSDK service: TBD.


---






## 2. API Overview

- **Endpoints:** Summarize key endpoints (e.g., `/capture`, `/extract`).
- **Authentication:** Note any authentication/authorization requirements.

## 3. Data Model

- **Main Objects:**  
    - `sample`, `segments`, `bdbInfo`, `quality`, etc.
- **Common Fields:**  
    - `level`, `purpose`, `quality`, `errors`
- **Descriptions:** Explain the role and usage of each field.



For the Structure, Parameter etc.. and their definition you can also refer to [CBEFF](To be linked). 




# 4. Scenarios & Sample Payloads

## 4.1 Packets with all biometric data; Eye (Left and Right), Finger (All or particular ones)


- **Description:** All biometric modalities captured successfully (ye (Left and Right), Finger (All or particular ones)
- **Sample Request:**  


### Both Eye Capture Request
Request - Packet with only left and rigth eye biometric data..

```json
{
	"sample": {
		"segments": [
			{
				"version": {
					"major": 1,
					"minor": 1
				},
				"cbeffversion": {
					"major": 1,
					"minor": 1
				},
				"birInfo": {
					"integrity": false
				},
				"bdbInfo": {
					"index": "d118f98b-1556-495b-9678-bd567bf3062e",
					"format": {
						"organization": "Mosip",
						"type": "9"
					},
					"creationDate": {
						"date": {
							"year": 2025,
							"month": 5,
							"day": 7
						},
						"time": {
							"hour": 11,
							"minute": 28,
							"second": 19,
							"nano": 371670900
						}
					},
					"type": [
						"IRIS"
					],
					"subtype": [
						"Left"
					],
					"level": "RAW",
					"purpose": "ENROLL",
					"quality": {
						"algorithm": {
							"organization": "HMAC",
							"type": "SHA-256"
						},
						"score": 100
					}
				},
				"bdb":[], // Sample data - 
				"sb": [], // Sample data
				"others": {
					"SPEC_VERSION": "0.9.5",
					"RETRIES": "1",
					"FORCE_CAPTURED": "false",
					"EXCEPTION": "false",
					"PAYLOAD": "{\n    \"digitalId\": \"ewogICAgImFsZyI6ICJSUzI1NiIsCiAgICAidHlwIjogIkpXVCIsCiAgICAieDVjIjogWwogICAgICAgICJNSUlEZERDQ0FseWdBd0lCQWdJRU1vWWRPekFOQmdrcWhraUc5dzBCQVFzRkFEQmtNUXN3Q1FZRFZRUUdFd0pWVXpFUk1BOEdBMVVFQ0F3SVZtbHlaMmx1YVdFeEVEQU9CZ05WQkFjTUIwWmhhWEptWVhneEV6QVJCZ05WQkFvTUNrbHlhVlJsWTJoSmJtTXhHekFaQmdOVkJBTU1Fa2x5YVZSbFkyaEpibU1nVTNSaFoybHVaekFlRncweU5UQTFNRGN4TVRFNU16WmFGdzB5TlRBMk1EWXhNVEU1TXpaYU1GQXhDekFKQmdOVkJBWVRBbFZUTVJNd0VRWURWUVFMRXdwTllXNWhaMlZ0Wlc1ME1STXdFUVlEVlFRS0V3cEpjbWxVWldOb1NXNWpNUmN3RlFZRFZRUURFdzVKY21sVVpXTm9JRVJsZG1salpUQ0NBU0l3RFFZSktvWklodmNOQVFFQkJRQURnZ0VQQURDQ0FRb0NnZ0VCQUtkQ1R5Mm9PTm5oWEZEOGl5Y3ZxVkF4dnY2MDJ6SW1mUFJUdzVpTWxTZDZDODlCOEI4SjVVVjA2d1JEVk9hOWN4TzFvcGQ4TktHdEtMdTlRV1dCazRTRGRjQTAxV2xZZjA4bURNVFlvOVVDOG8rNzk0azdKK2RuUDZBR3pTOEhrTkxGQW5vTklpUkpSeTVGR0pjRlFJMUtpVExqRHJKbTRzU3NoeERFZXZpVmt5NWtoNkRSYjMwbUJuakZ5TnBpMHN2ZHFpSUVvZDc3NXFxZEdLVUxNOUp3ay9FamwvVGxLZ29YL3dQMmFLeEJqdjcydWhtL3F5dGp0SkhiMkFSdndjeG9VMUNMVUk4czRsNUVVL0lKYzRsSW1oejc3QUZHS0NUTzg5YW1qYlZPbFZ6SnF0UHVCZUhsTnBaSjVxTnRuOFNzSjlSK3RINmdoUVpORk53VThZOENBd0VBQWFOQ01FQXdIUVlEVlIwT0JCWUVGTi93RzExNkFLOURlb0pSU0pSYTE4TjBmRTE3TUI4R0ExVWRJd1FZTUJhQUZDRU5pcE1NNjZkVHFSVXJBNHVXbG9udnlCUk9NQTBHQ1NxR1NJYjNEUUVCQ3dVQUE0SUJBUUJwVnVpS2YydG94TzBsTU1FNVNMYWhFcGxKQlpNU2kzNHdMcUZKZi92NWtsVkNMaHl0ZDBZa2dRVlB6dytXR0VaaWxIeFkzYkFjZTZDeVQzYjBQZzZFbTJKYjZmMmxwK0ttRUdkOVcrblByYVF2L0MrOFlwV3MrWDJkSm5WTGhsOHpDcWxPV09YQ3JwM1VFZTU5NktyZkxJbFRLUWJBZy9kZ3hPQy9NYzZxTTgvZWpzTFFlaytrdkwvWmVHMjJiSFlPcUFBc3ZTL1RlZ3FOa2E4d2xIYWtOVHNDMWw5aWppeUdUWVlSRytIMStlN3hvUzc2T3paZXFlajhpc1orNk9kZGV2NEs4eTBIaE8xeE1GMUhYUGZhUFdKak5hL0lxY1NTY1lpaWZnMGVobENrNVdDYVNtUlo1R0RhWDF0S3dKbUtydWVjUmFxOFgrRXdWaEgwc1dORiIKICAgIF0KfQ.ewogICAgInNlcmlhbE5vIjogIkRGMDA1MjAwMDAxOTg2OUEiLAogICAgIm1ha2UiOiAiSXJpU2hpZWxkIiwKICAgICJtb2RlbCI6ICJCSzIxMjFVIiwKICAgICJ0eXBlIjogIklyaXMiLAogICAgImRldmljZVN1YlR5cGUiOiAiRG91YmxlIiwKICAgICJkZXZpY2VQcm92aWRlciI6ICJJcmlUZWNoIiwKICAgICJkZXZpY2VQcm92aWRlcklkIjogIklyaVRlY2giLAogICAgImRhdGVUaW1lIjogIjIwMjUtMDUtMDdUMTE6Mjc6MTVaIgp9.pP0tNusUshMxSxFWVkZrDA_T3qk0p1g5ffFjQWp83U26Rk8DDp83btLoQP3n_5Su5g_T0vvbQa4Y9M0uuul4yroe6lArK5tcB4dPNmREs4NKPUlGOyel7w6RYQZ41Uw5vNk7MLyet7zLbwNxCzDj5Nafli1y4pSJMmXpB4Nmdk4VGupSN3vTnbAIlofPhxda1Fj-943Z06Km7ddBo7fHMDsIMlkIvckwmMcxuBE1rMKT6I7s1GzFlM2Ma9IK54qUrGmRUUJzP7HXiB8ga6J1UYSjNe0lofWYaWm0j_PhpqKrIW9JV3lVIX4A_W1ugUGcaHqKidTjNHaSGW6zx11bPA\",\n    \"deviceCode\": \"DF0052000019869A\",\n    \"deviceServiceVersion\": \"0.9.5\",\n    \"bioType\": \"Iris\",\n    \"bioSubType\": \"Left\",\n    \"purpose\": \"Registration\",\n    \"env\": \"Staging\",\n    \"bioValue\": \"<bioValue>\",\n    \"transactionId\": \"61133ef7-1d97-4208-a6fa-4fedca30ac86\",\n    \"timestamp\": \"2025-05-07T11:27:13Z\",\n    \"requestedScore\": \"80\",\n    \"qualityScore\": \"100\"\n}",
					"SDK_SCORE": "100.0"
				}
			},
			{
				"version": {
					"major": 1,
					"minor": 1
				},
				"cbeffversion": {
					"major": 1,
					"minor": 1
				},
				"birInfo": {
					"integrity": false
				},
				"bdbInfo": {
					"index": "34fdeea8-3376-498a-a7ba-e3c4a206b78d",
					"format": {
						"organization": "Mosip",
						"type": "9"
					},
					"creationDate": {
						"date": {
							"year": 2025,
							"month": 5,
							"day": 7
						},
						"time": {
							"hour": 11,
							"minute": 28,
							"second": 19,
							"nano": 371670900
						}
					},
					"type": [
						"IRIS"
					],
					"subtype": [
						"Right"
					],
					"level": "RAW",
					"purpose": "ENROLL",
					"quality": {
						"algorithm": {
							"organization": "HMAC",
							"type": "SHA-256"
						},
						"score": 100
					}
				},
				"bdb": [], // Sample data
				"sb": [], // Sample data
				"others": {
					"SPEC_VERSION": "0.9.5",
					"RETRIES": "1",
					"FORCE_CAPTURED": "false",
					"EXCEPTION": "false",
					"PAYLOAD": "{\n    \"digitalId\": \"ewogICAgImFsZyI6ICJSUzI1NiIsCiAgICAidHlwIjogIkpXVCIsCiAgICAieDVjIjogWwogICAgICAgICJNSUlEZERDQ0FseWdBd0lCQWdJRU1vWWRPekFOQmdrcWhraUc5dzBCQVFzRkFEQmtNUXN3Q1FZRFZRUUdFd0pWVXpFUk1BOEdBMVVFQ0F3SVZtbHlaMmx1YVdFeEVEQU9CZ05WQkFjTUIwWmhhWEptWVhneEV6QVJCZ05WQkFvTUNrbHlhVlJsWTJoSmJtTXhHekFaQmdOVkJBTU1Fa2x5YVZSbFkyaEpibU1nVTNSaFoybHVaekFlRncweU5UQTFNRGN4TVRFNU16WmFGdzB5TlRBMk1EWXhNVEU1TXpaYU1GQXhDekFKQmdOVkJBWVRBbFZUTVJNd0VRWURWUVFMRXdwTllXNWhaMlZ0Wlc1ME1STXdFUVlEVlFRS0V3cEpjbWxVWldOb1NXNWpNUmN3RlFZRFZRUURFdzVKY21sVVpXTm9JRVJsZG1salpUQ0NBU0l3RFFZSktvWklodmNOQVFFQkJRQURnZ0VQQURDQ0FRb0NnZ0VCQUtkQ1R5Mm9PTm5oWEZEOGl5Y3ZxVkF4dnY2MDJ6SW1mUFJUdzVpTWxTZDZDODlCOEI4SjVVVjA2d1JEVk9hOWN4TzFvcGQ4TktHdEtMdTlRV1dCazRTRGRjQTAxV2xZZjA4bURNVFlvOVVDOG8rNzk0azdKK2RuUDZBR3pTOEhrTkxGQW5vTklpUkpSeTVGR0pjRlFJMUtpVExqRHJKbTRzU3NoeERFZXZpVmt5NWtoNkRSYjMwbUJuakZ5TnBpMHN2ZHFpSUVvZDc3NXFxZEdLVUxNOUp3ay9FamwvVGxLZ29YL3dQMmFLeEJqdjcydWhtL3F5dGp0SkhiMkFSdndjeG9VMUNMVUk4czRsNUVVL0lKYzRsSW1oejc3QUZHS0NUTzg5YW1qYlZPbFZ6SnF0UHVCZUhsTnBaSjVxTnRuOFNzSjlSK3RINmdoUVpORk53VThZOENBd0VBQWFOQ01FQXdIUVlEVlIwT0JCWUVGTi93RzExNkFLOURlb0pSU0pSYTE4TjBmRTE3TUI4R0ExVWRJd1FZTUJhQUZDRU5pcE1NNjZkVHFSVXJBNHVXbG9udnlCUk9NQTBHQ1NxR1NJYjNEUUVCQ3dVQUE0SUJBUUJwVnVpS2YydG94TzBsTU1FNVNMYWhFcGxKQlpNU2kzNHdMcUZKZi92NWtsVkNMaHl0ZDBZa2dRVlB6dytXR0VaaWxIeFkzYkFjZTZDeVQzYjBQZzZFbTJKYjZmMmxwK0ttRUdkOVcrblByYVF2L0MrOFlwV3MrWDJkSm5WTGhsOHpDcWxPV09YQ3JwM1VFZTU5NktyZkxJbFRLUWJBZy9kZ3hPQy9NYzZxTTgvZWpzTFFlaytrdkwvWmVHMjJiSFlPcUFBc3ZTL1RlZ3FOa2E4d2xIYWtOVHNDMWw5aWppeUdUWVlSRytIMStlN3hvUzc2T3paZXFlajhpc1orNk9kZGV2NEs4eTBIaE8xeE1GMUhYUGZhUFdKak5hL0lxY1NTY1lpaWZnMGVobENrNVdDYVNtUlo1R0RhWDF0S3dKbUtydWVjUmFxOFgrRXdWaEgwc1dORiIKICAgIF0KfQ.ewogICAgInNlcmlhbE5vIjogIkRGMDA1MjAwMDAxOTg2OUEiLAogICAgIm1ha2UiOiAiSXJpU2hpZWxkIiwKICAgICJtb2RlbCI6ICJCSzIxMjFVIiwKICAgICJ0eXBlIjogIklyaXMiLAogICAgImRldmljZVN1YlR5cGUiOiAiRG91YmxlIiwKICAgICJkZXZpY2VQcm92aWRlciI6ICJJcmlUZWNoIiwKICAgICJkZXZpY2VQcm92aWRlcklkIjogIklyaVRlY2giLAogICAgImRhdGVUaW1lIjogIjIwMjUtMDUtMDdUMTE6Mjc6MTVaIgp9.pP0tNusUshMxSxFWVkZrDA_T3qk0p1g5ffFjQWp83U26Rk8DDp83btLoQP3n_5Su5g_T0vvbQa4Y9M0uuul4yroe6lArK5tcB4dPNmREs4NKPUlGOyel7w6RYQZ41Uw5vNk7MLyet7zLbwNxCzDj5Nafli1y4pSJMmXpB4Nmdk4VGupSN3vTnbAIlofPhxda1Fj-943Z06Km7ddBo7fHMDsIMlkIvckwmMcxuBE1rMKT6I7s1GzFlM2Ma9IK54qUrGmRUUJzP7HXiB8ga6J1UYSjNe0lofWYaWm0j_PhpqKrIW9JV3lVIX4A_W1ugUGcaHqKidTjNHaSGW6zx11bPA\",\n    \"deviceCode\": \"DF0052000019869A\",\n    \"deviceServiceVersion\": \"0.9.5\",\n    \"bioType\": \"Iris\",\n    \"bioSubType\": \"Right\",\n    \"purpose\": \"Registration\",\n    \"env\": \"Staging\",\n    \"bioValue\": \"<bioValue>\",\n    \"transactionId\": \"61133ef7-1d97-4208-a6fa-4fedca30ac86\",\n    \"timestamp\": \"2025-05-07T11:27:13Z\",\n    \"requestedScore\": \"80\",\n    \"qualityScore\": \"100\"\n}",
					"SDK_SCORE": "100.0"
				}
			}
		],
		"others": {}
	},
	"modalitiesToExtract": [
		"IRIS"
	],
	"flags": {
		"iris.format": "Iris"
	}
}
```
<!--
##### Description

This JSON sample demonstrates a "Both Eye Capture" scenario for iris biometrics using the SDK. The structure and key fields are as follows:

**Top-Level Fields**
- `sample`: Main object holding biometric data and metadata.
	- `segments`: Array containing one object per eye (left and right). Each segment represents a single iris capture.
	- `others`: Reserved for additional data (empty in this example).
- `modalitiesToExtract`: Specifies which biometric modalities to extract (here, `"IRIS"`).
- `flags`: Additional configuration, such as iris format.

**Inside Each `segment`**
*`version` / `cbeffversion`: Versioning for the segment and CBEFF standard.
* `birInfo`: Biometric Information Record metadata (e.g., `integrity` flag).
* `bdbInfo`: Metadata about the biometric data block, including:
	- `index`: Unique identifier for the capture.
	- `format`: Organization and type.
	- `creationDate`: Timestamp of capture.
	- `type` / `subtype`: Biometric type (always `"IRIS"`) and which eye (`"Left"` or `"Right"`).
	- `level`: Data level (e.g., `"RAW"`).
	- `purpose`: Purpose of capture (e.g., `"ENROLL"`).
	- `quality`: Quality assessment, including algorithm and score.
* `bdb`: Biometric data block (empty array here; would contain encoded iris data in real use).
* `sb`: Supplemental block (empty array here).
* `others`: Additional metadata, such as:
	- `SPEC_VERSION`: Specification version.
	- `RETRIES`: Number of capture attempts.
	- `FORCE_CAPTURED`: Whether capture was forced.
	- `EXCEPTION`: If an exception occurred.
	- `PAYLOAD`: Encoded payload with device and transaction details.
	- `SDK_SCORE`: SDK-calculated quality score.

**Usage**
This structure standardizes the representation of both-eye iris captures, including all necessary metadata for quality control, auditing, and further biometric processing.

-->


### Both Eyes (Left and Right eys) Response
  
    [`Botheyes_Response.json`](./SampleRequests&Responses/Botheyes_Response.json)

```json
{
	"version": "0.9",
	"responsetime": "2025-05-07T13:28:58.379Z",
	"response": {
		"statusCode": 200,
		"statusMessage": "OK",
		"response": {
			"version": null,
			"cbeffversion": null,
			"birInfo": null,
			"segments": [
				{
					"version": {
						"major": 1,
						"minor": 1
					},
					"cbeffversion": {
						"major": 1,
						"minor": 1
					},
					"birInfo": {
						"creator": null,
						"index": null,
						"payload": null,
						"integrity": false,
						"creationDate": null,
						"notValidBefore": null,
						"notValidAfter": null
					},
					"bdbInfo": {
						"challengeResponse": null,
						"index": "d118f98b-1556-495b-9678-bd567bf3062e",
						"format": {
							"organization": "Mosip",
							"type": "9"
						},
						"encryption": null,
						"creationDate": {
							"date": {
								"year": 2025,
								"month": 5,
								"day": 7
							},
							"time": {
								"hour": 11,
								"minute": 28,
								"second": 19,
								"nano": 371670900
							}
						},
						"notValidBefore": null,
						"notValidAfter": null,
						"type": [
							"IRIS"
						],
						"subtype": [
							"Left"
						],
						"level": "PROCESSED",
						"product": null,
						"captureDevice": null,
						"featureExtractionAlgorithm": null,
						"comparisonAlgorithm": null,
						"compressionAlgorithm": null,
						"purpose": "VERIFY",
						"quality": {
							"algorithm": {
								"organization": "HMAC",
								"type": "SHA-256"
							},
							"score": 100,
							"qualityCalculationFailed": null
						}
					},
					"bdb": [], // Sample data for Left Iris
					"sb": null,
					"birs": null,
					"sbInfo": null,
					"others": {}
				},
				{
					"version": {
						"major": 1,
						"minor": 1
					},
					"cbeffversion": {
						"major": 1,
						"minor": 1
					},
					"birInfo": {
						"creator": null,
						"index": null,
						"payload": null,
						"integrity": false,
						"creationDate": null,
						"notValidBefore": null,
						"notValidAfter": null
					},
					"bdbInfo": {
						"challengeResponse": null,
						"index": "34fdeea8-3376-498a-a7ba-e3c4a206b78d",
						"format": {
							"organization": "Mosip",
							"type": "9"
						},
						"encryption": null,
						"creationDate": {
							"date": {
								"year": 2025,
								"month": 5,
								"day": 7
							},
							"time": {
								"hour": 11,
								"minute": 28,
								"second": 19,
								"nano": 371670900
							}
						},
						"notValidBefore": null,
						"notValidAfter": null,
						"type": [
							"IRIS"
						],
						"subtype": [
							"Right"
						],
						"level": "PROCESSED",
						"product": null,
						"captureDevice": null,
						"featureExtractionAlgorithm": null,
						"comparisonAlgorithm": null,
						"compressionAlgorithm": null,
						"purpose": "VERIFY",
						"quality": {
							"algorithm": {
								"organization": "HMAC",
								"type": "SHA-256"
							},
							"score": 100,
							"qualityCalculationFailed": null
						}
					},
					"bdb": [], // Sample data for Right Iris
					"sb": null,
					"birs": null,
					"sbInfo": null,
					"others": {}
				}
			],
			"others": {}
		}
	},
	"errors": []
}

```

## Partial Exceptions

One or more modalities contain exceptions (e.g., only one eye captured).

- **Sample Request:**  
    [`Single_Eye_Exception_Capture.json`](./SampleRequests&Responses/Single_Eye_Exception_Capture.json)
```json

{
	"sample": {
		"segments": [
			{
				"version": {
					"major": 1,
					"minor": 1
				},
				"cbeffversion": {
					"major": 1,
					"minor": 1
				},
				"birInfo": {
					"integrity": false
				},
				"bdbInfo": {
					"index": "1964589a-8c67-4fa3-8f81-ccc5d9863ee9",
					"format": {
						"organization": "Mosip",
						"type": "9"
					},
					"creationDate": {
						"date": {
							"year": 2025,
							"month": 5,
							"day": 7
						},
						"time": {
							"hour": 12,
							"minute": 13,
							"second": 41,
							"nano": 831325700
						}
					},
					"type": [
						"IRIS"
					],
					"subtype": [
						"Left"
					],
					"level": "RAW",
					"purpose": "ENROLL",
					"quality": {
						"algorithm": {
							"organization": "HMAC",
							"type": "SHA-256"
						},
						"score": 100
					}
				},
				"bdb": [], //Sample data for the BDB field
				"sb": [], //Sample data for the SB field
				"others": {
					"SPEC_VERSION": "0.9.5",
					"RETRIES": "1",
					"FORCE_CAPTURED": "false",
					"EXCEPTION": "false",
					"PAYLOAD": "{\n    \"digitalId\": \"ewogICAgImFsZyI6ICJSUzI1NiIsCiAgICAidHlwIjogIkpXVCIsCiAgICAieDVjIjogWwogICAgICAgICJNSUlEZERDQ0FseWdBd0lCQWdJRU1vWWRPekFOQmdrcWhraUc5dzBCQVFzRkFEQmtNUXN3Q1FZRFZRUUdFd0pWVXpFUk1BOEdBMVVFQ0F3SVZtbHlaMmx1YVdFeEVEQU9CZ05WQkFjTUIwWmhhWEptWVhneEV6QVJCZ05WQkFvTUNrbHlhVlJsWTJoSmJtTXhHekFaQmdOVkJBTU1Fa2x5YVZSbFkyaEpibU1nVTNSaFoybHVaekFlRncweU5UQTFNRGN4TVRFNU16WmFGdzB5TlRBMk1EWXhNVEU1TXpaYU1GQXhDekFKQmdOVkJBWVRBbFZUTVJNd0VRWURWUVFMRXdwTllXNWhaMlZ0Wlc1ME1STXdFUVlEVlFRS0V3cEpjbWxVWldOb1NXNWpNUmN3RlFZRFZRUURFdzVKY21sVVpXTm9JRVJsZG1salpUQ0NBU0l3RFFZSktvWklodmNOQVFFQkJRQURnZ0VQQURDQ0FRb0NnZ0VCQUtkQ1R5Mm9PTm5oWEZEOGl5Y3ZxVkF4dnY2MDJ6SW1mUFJUdzVpTWxTZDZDODlCOEI4SjVVVjA2d1JEVk9hOWN4TzFvcGQ4TktHdEtMdTlRV1dCazRTRGRjQTAxV2xZZjA4bURNVFlvOVVDOG8rNzk0azdKK2RuUDZBR3pTOEhrTkxGQW5vTklpUkpSeTVGR0pjRlFJMUtpVExqRHJKbTRzU3NoeERFZXZpVmt5NWtoNkRSYjMwbUJuakZ5TnBpMHN2ZHFpSUVvZDc3NXFxZEdLVUxNOUp3ay9FamwvVGxLZ29YL3dQMmFLeEJqdjcydWhtL3F5dGp0SkhiMkFSdndjeG9VMUNMVUk4czRsNUVVL0lKYzRsSW1oejc3QUZHS0NUTzg5YW1qYlZPbFZ6SnF0UHVCZUhsTnBaSjVxTnRuOFNzSjlSK3RINmdoUVpORk53VThZOENBd0VBQWFOQ01FQXdIUVlEVlIwT0JCWUVGTi93RzExNkFLOURlb0pSU0pSYTE4TjBmRTE3TUI4R0ExVWRJd1FZTUJhQUZDRU5pcE1NNjZkVHFSVXJBNHVXbG9udnlCUk9NQTBHQ1NxR1NJYjNEUUVCQ3dVQUE0SUJBUUJwVnVpS2YydG94TzBsTU1FNVNMYWhFcGxKQlpNU2kzNHdMcUZKZi92NWtsVkNMaHl0ZDBZa2dRVlB6dytXR0VaaWxIeFkzYkFjZTZDeVQzYjBQZzZFbTJKYjZmMmxwK0ttRUdkOVcrblByYVF2L0MrOFlwV3MrWDJkSm5WTGhsOHpDcWxPV09YQ3JwM1VFZTU5NktyZkxJbFRLUWJBZy9kZ3hPQy9NYzZxTTgvZWpzTFFlaytrdkwvWmVHMjJiSFlPcUFBc3ZTL1RlZ3FOa2E4d2xIYWtOVHNDMWw5aWppeUdUWVlSRytIMStlN3hvUzc2T3paZXFlajhpc1orNk9kZGV2NEs4eTBIaE8xeE1GMUhYUGZhUFdKak5hL0lxY1NTY1lpaWZnMGVobENrNVdDYVNtUlo1R0RhWDF0S3dKbUtydWVjUmFxOFgrRXdWaEgwc1dORiIKICAgIF0KfQ.ewogICAgInNlcmlhbE5vIjogIkRGMDA1MjAwMDAxOTg2OUEiLAogICAgIm1ha2UiOiAiSXJpU2hpZWxkIiwKICAgICJtb2RlbCI6ICJCSzIxMjFVIiwKICAgICJ0eXBlIjogIklyaXMiLAogICAgImRldmljZVN1YlR5cGUiOiAiRG91YmxlIiwKICAgICJkZXZpY2VQcm92aWRlciI6ICJJcmlUZWNoIiwKICAgICJkZXZpY2VQcm92aWRlcklkIjogIklyaVRlY2giLAogICAgImRhdGVUaW1lIjogIjIwMjUtMDUtMDdUMTI6MTI6MzFaIgp9.n7KnKGjO0vNMyhuXBGMl_z-iarKadc1i0_auV7s7ehQY31TLAkKPmFEi8zEJlDh4f9KB-L7HescoYIM_MdEYwCpInema_K5bhpY_3QSj_5sKQmTnQSYIRAEp8twNblrDJYxaK6dY83DzOZyJV-mif7rWw4IGgA7OMH1qDLYrQ1eoFraSb2_YS9b0N_tHcZbkr-Q_q-ZzqdBylIpUuh3KuIMPzcgwFwRwn8yIrtONj-fg-wo03E5wfpNRmOiLxa5qdLp_5ZzG2MDfRtsSYLoh2-E-Ms_H50C6tLVNMB8FBJhVx0RLNXMI_M4a6p_EGmfUUNAKMsOI1XNZ8VJAv9H5YQ\",\n    \"deviceCode\": \"DF0052000019869A\",\n    \"deviceServiceVersion\": \"0.9.5\",\n    \"bioType\": \"Iris\",\n    \"bioSubType\": \"Left\",\n    \"purpose\": \"Registration\",\n    \"env\": \"Staging\",\n    \"bioValue\": \"<bioValue>\",\n    \"transactionId\": \"0ad321b6-18e4-4e0e-9b7a-c33a00e443da\",\n    \"timestamp\": \"2025-05-07T12:12:30Z\",\n    \"requestedScore\": \"80\",\n    \"qualityScore\": \"100\"\n}",
					"SDK_SCORE": "100.0"
				}
			},
			{
				"version": {
					"major": 1,
					"minor": 1
				},
				"cbeffversion": {
					"major": 1,
					"minor": 1
				},
				"birInfo": {
					"integrity": false
				},
				"bdbInfo": {
					"index": "9f9b9c04-99f2-4620-bbed-e2463e1678ca",
					"format": {
						"organization": "Mosip",
						"type": "9"
					},
					"creationDate": {
						"date": {
							"year": 2025,
							"month": 5,
							"day": 7
						},
						"time": {
							"hour": 12,
							"minute": 13,
							"second": 41,
							"nano": 831325700
						}
					},
					"type": [
						"IRIS"
					],
					"subtype": [
						"Right"
					],
					"level": "RAW",
					"purpose": "ENROLL",
					"quality": {
						"algorithm": {
							"organization": "HMAC",
							"type": "SHA-256"
						},
						"score": 0
					}
				},
				"others": {
					"SPEC_VERSION": "",
					"RETRIES": "0",
					"FORCE_CAPTURED": "false",
					"EXCEPTION": "true",
					"PAYLOAD": "",
					"SDK_SCORE": "0.0"
				}
			}
		],
		"others": {}
	},
	"modalitiesToExtract": [
		"IRIS"
	],
	"flags": {
		"iris.format": "Iris"
	}
}


```


- **Sample Response:**  
    [`Single_Eye_exception_Response.json`](./SampleRequests&Responses/Single_Eye_exception_Response.json)

```json

{
	"version": "0.9",
	"responsetime": "2025-05-07T13:48:27.596Z",
	"response": {
		"statusCode": 200,
		"statusMessage": "OK",
		"response": {
			"version": null,
			"cbeffversion": null,
			"birInfo": null,
			"segments": [
				{
					"version": {
						"major": 1,
						"minor": 1
					},
					"cbeffversion": {
						"major": 1,
						"minor": 1
					},
					"birInfo": {
						"creator": null,
						"index": null,
						"payload": null,
						"integrity": false,
						"creationDate": null,
						"notValidBefore": null,
						"notValidAfter": null
					},
					"bdbInfo": {
						"challengeResponse": null,
						"index": "1964589a-8c67-4fa3-8f81-ccc5d9863ee9",
						"format": {
							"organization": "Mosip",
							"type": "9"
						},
						"encryption": null,
						"creationDate": {
							"date": {
								"year": 2025,
								"month": 5,
								"day": 7
							},
							"time": {
								"hour": 12,
								"minute": 13,
								"second": 41,
								"nano": 831325700
							}
						},
						"notValidBefore": null,
						"notValidAfter": null,
						"type": [
							"IRIS"
						],
						"subtype": [
							"Left"
						],
						"level": "PROCESSED",
						"product": null,
						"captureDevice": null,
						"featureExtractionAlgorithm": null,
						"comparisonAlgorithm": null,
						"compressionAlgorithm": null,
						"purpose": "VERIFY",
						"quality": {
							"algorithm": {
								"organization": "HMAC",
								"type": "SHA-256"
							},
							"score": 100,
							"qualityCalculationFailed": null
						}
					},
					"bdb": [], // Sample data for bdb
					"sb": null,
					"birs": null,
					"sbInfo": null,
					"others": {}
				}
				{
				"version": {
					"major": 1,
					"minor": 1
				},
				"cbeffversion": {
					"major": 1,
					"minor": 1
				},
				"birInfo": {
					"integrity": false
				},
				"bdbInfo": {
					"index": "9f9b9c04-99f2-4620-bbed-e2463e1678ca",
					"format": {
						"organization": "Mosip",
						"type": "9"
					},
					"creationDate": {
						"date": {
							"year": 2025,
							"month": 5,
							"day": 7
						},
						"time": {
							"hour": 12,
							"minute": 13,
							"second": 41,
							"nano": 831325700
						}
					},
					"type": [
						"IRIS"
					],
					"subtype": [
						"Right"
					],
					"level": "RAW",
					"purpose": "ENROLL",
					"quality": {
						"algorithm": {
							"organization": "HMAC",
							"type": "SHA-256"
						},
						"score": 0
					}
				},
				"others": {
					"SPEC_VERSION": "",
					"RETRIES": "0",
					"FORCE_CAPTURED": "false",
					"EXCEPTION": "true",
					"PAYLOAD": "",
					"SDK_SCORE": "0.0"
				}
			}
			],
			"others": {}
		}
	},
	"errors": []
}

```

### 4.3 Total Exceptions

Entire modality marked as exception (Both eyes; left and right, All fingers)

#### Sample Request Payload

    [`Total_EyeException_Capture.json`](./SampleRequests&Responses/Total_EyeException_Capture.json)

```json

{
	"sample": {
		"segments": [
			{
				"version": {
					"major": 1,
					"minor": 1
				},
				"cbeffversion": {
					"major": 1,
					"minor": 1
				},
				"birInfo": {
					"integrity": false
				},
				"bdbInfo": {
					"index": "aa597540-0866-40fd-a303-6ea82ccb9ca0",
					"format": {
						"organization": "Mosip",
						"type": "9"
					},
					"creationDate": {
						"date": {
							"year": 2025,
							"month": 5,
							"day": 7
						},
						"time": {
							"hour": 11,
							"minute": 25,
							"second": 18,
							"nano": 450031100
						}
					},
					"type": [
						"IRIS"
					],
					"subtype": [
						"Left"
					],
					"level": "RAW",
					"purpose": "ENROLL",
					"quality": {
						"algorithm": {
							"organization": "HMAC",
							"type": "SHA-256"
						},
						"score": 0
					}
				},
				"others": {
					"SPEC_VERSION": "",
					"RETRIES": "0",
					"FORCE_CAPTURED": "false",
					"EXCEPTION": "true",
					"PAYLOAD": "",
					"SDK_SCORE": "0.0"
				}
			},
			{
				"version": {
					"major": 1,
					"minor": 1
				},
				"cbeffversion": {
					"major": 1,
					"minor": 1
				},
				"birInfo": {
					"integrity": false
				},
				"bdbInfo": {
					"index": "043518b0-2a13-4702-9327-4060b532d5ee",
					"format": {
						"organization": "Mosip",
						"type": "9"
					},
					"creationDate": {
						"date": {
							"year": 2025,
							"month": 5,
							"day": 7
						},
						"time": {
							"hour": 11,
							"minute": 25,
							"second": 18,
							"nano": 450031100
						}
					},
					"type": [
						"IRIS"
					],
					"subtype": [
						"Right"
					],
					"level": "RAW",
					"purpose": "ENROLL",
					"quality": {
						"algorithm": {
							"organization": "HMAC",
							"type": "SHA-256"
						},
						"score": 0
					}
				},
				"others": {
					"SPEC_VERSION": "",
					"RETRIES": "0",
					"FORCE_CAPTURED": "false",
					"EXCEPTION": "true",
					"PAYLOAD": "",
					"SDK_SCORE": "0.0"
				}
			}
		],
		"others": {}
	},
	"modalitiesToExtract": [
		"IRIS"
	],
	"flags": {
		"iris.format": "Iris"
	}
}

```


#### Sample Response Payload

    [`Total_EyeException_Response.json`](./SampleRequests&Responses/Total_EyeException_Response.json)

```json

{
	"sample": {
		"segments": [
			{
				"version": {
					"major": 1,
					"minor": 1
				},
				"cbeffversion": {
					"major": 1,
					"minor": 1
				},
				"birInfo": {
					"integrity": false
				},
				"bdbInfo": {
					"index": "aa597540-0866-40fd-a303-6ea82ccb9ca0",
					"format": {
						"organization": "Mosip",
						"type": "9"
					},
					"creationDate": {
						"date": {
							"year": 2025,
							"month": 5,
							"day": 7
						},
						"time": {
							"hour": 11,
							"minute": 25,
							"second": 18,
							"nano": 450031100
						}
					},
					"type": [
						"IRIS"
					],
					"subtype": [
						"Left"
					],
					"level": "RAW",
					"purpose": "ENROLL",
					"quality": {
						"algorithm": {
							"organization": "HMAC",
							"type": "SHA-256"
						},
						"score": 0
					}
				},
				"others": {
					"SPEC_VERSION": "",
					"RETRIES": "0",
					"FORCE_CAPTURED": "false",
					"EXCEPTION": "true",
					"PAYLOAD": "",
					"SDK_SCORE": "0.0"
				}
			},
			{
				"version": {
					"major": 1,
					"minor": 1
				},
				"cbeffversion": {
					"major": 1,
					"minor": 1
				},
				"birInfo": {
					"integrity": false
				},
				"bdbInfo": {
					"index": "043518b0-2a13-4702-9327-4060b532d5ee",
					"format": {
						"organization": "Mosip",
						"type": "9"
					},
					"creationDate": {
						"date": {
							"year": 2025,
							"month": 5,
							"day": 7
						},
						"time": {
							"hour": 11,
							"minute": 25,
							"second": 18,
							"nano": 450031100
						}
					},
					"type": [
						"IRIS"
					],
					"subtype": [
						"Right"
					],
					"level": "RAW",
					"purpose": "ENROLL",
					"quality": {
						"algorithm": {
							"organization": "HMAC",
							"type": "SHA-256"
						},
						"score": 0
					}
				},
				"others": {
					"SPEC_VERSION": "",
					"RETRIES": "0",
					"FORCE_CAPTURED": "false",
					"EXCEPTION": "true",
					"PAYLOAD": "",
					"SDK_SCORE": "0.0"
				}
			}
		],
		"others": {}
	},
	"modalitiesToExtract": [
		"IRIS"
	],
	"flags": {
		"iris.format": "Iris"
	}
}

```

## Other Modalities

### Fingers

#### Sample Request: Just One Finger Capture

    - [`Sample_Finger_JustOneFinger_Capture.json`](./SampleRequests&Responses/Sample_Finger_JustOneFinger_Capture.json)

```json

{
	"sample": {
		"segments": [
			{
				"version": {
					"major": 1,
					"minor": 1
				},
				"cbeffversion": {
					"major": 1,
					"minor": 1
				},
				"birInfo": {
					"integrity": false
				},
				"bdbInfo": {
					"index": "6aa4ee19-2106-4691-95e8-edbd0ad65f2f",
					"format": {
						"organization": "Mosip",
						"type": "7"
					},
					"creationDate": {
						"date": {
							"year": 2025,
							"month": 5,
							"day": 7
						},
						"time": {
							"hour": 11,
							"minute": 35,
							"second": 34,
							"nano": 247309200
						}
					},
					"type": [
						"FINGER"
					],
					"subtype": [
						"Right",
						"Thumb"
					],
					"level": "RAW",
					"purpose": "ENROLL",
					"quality": {
						"algorithm": {
							"organization": "HMAC",
							"type": "SHA-256"
						},
						"score": 90
					}
				},
				"bdb": [], // Sample BDB data
				"sb": [], // Sample SB data
				"others": {
					"SPEC_VERSION": "0.9.5",
					"RETRIES": "1",
					"FORCE_CAPTURED": "false",
					"EXCEPTION": "false",
					"PAYLOAD": "{\"digitalId\":\"eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsIng1YyI6WyJNSUlEdVRDQ0FxT2dBd0lCQWdJR0FaWkhVbENmTUFzR0NTcUdTSWIzRFFFQkN6Q0JrVEVMTUFrR0ExVUVCaE1DU1U0eEVqQVFCZ05WQkFnTUNWUmxiR0Z1WjJGdVlURU1NQW9HQTFVRUJ3d0RTRmxFTVN3d0tnWURWUVFLRENOT1VISnBiV1VnVkdWamFHNXZiRzluYVdWeklGQnlhWFpoZEdVZ1RHbHRhWFJsWkRFY01Cb0dBMVVFQ3d3VFRsQnlhVzFsSUZSbFkyaHViMnh2WjJsbGN6RVVNQklHQTFVRUF3d0xUbEJ5YVcxbFJGQkxUVk13SGhjTk1qVXdOREU0TURVeE5EVTVXaGNOTWpVd05qRTNNRFV4TkRVNVdqQ0JqVEVmTUIwR0ExVUVBeE1XT1RNNU5qVXpNREk1TGxKbFlXeFRZMkZ1TFVjeE1ERVRNQkVHQTFVRUN4TUtVM1Z3Y21WdFlTQkpSREVRTUE0R0ExVUVDaE1IVTNWd2NtVnRZVEVhTUJnR0ExVUVCeE1SVW1Wd2RXSnNhV01nYjJZZ1MyOXlaV0V4R2pBWUJnTlZCQWdURVZKbGNIVmliR2xqSUc5bUlFdHZjbVZoTVFzd0NRWURWUVFHRXdKTFVqQ0NBU0l3RFFZSktvWklodmNOQVFFQkJRQURnZ0VQQURDQ0FRb0NnZ0VCQUtlcUQ2S1FZbmYwQjcyb29sM1ZldTNSNUNReDI3endhK2JTUGdHQlNIR3NVSzBKWlZrU2dtZkQrVFBSUzNOd3pKTzI4OEgyUkxxNmk0MWVzbVJMUkdDUnlWZUh1MTZOZzhtWmk5Y0t0L1JhQTZLM3RUWUxEZEU3cUliK2NYMHJqYVhDZG9KQ1kvYm5jRGRQNGFwaWxDWmhZM216Yi96NXJQRS84S1UvdnVvNm1QVHEwZEZsL1FLRDl1RlRvbDlyM0ZWMVVYUklRZFlicUVUSW4rampoaHZIenhLUDZMRUxDVkJKd1JrQURDNEdCNHZraFF3ZkNTVDgwN3Rva1REVkRHeVRKY2h3MWR0Q1M3NExnRk9HaWpzdlJzM050Qk5DQXErait1Q3g4aHkxV0RxR0kwMVRXbisxS09EZ1EwSzVySUM4RFcyd2FKc09MUW1iMW5zb0xTRUNBd0VBQWFNZE1Cc3dEQVlEVlIwVEJBVXdBd0VCL3pBTEJnTlZIUThFQkFNQ0FZWXdDd1lKS29aSWh2Y05BUUVMQTRJQkFRQ25CRnZGNERyYVBZMldLbG5yNkt4elFyWUlHejZyZktiNlN6aTRIMkk5MFBrZnc4T3Y4bS8rZ28xTU5JeHBJUkFkTnZmeXZIVFk3QTExSlhsQitBeGZTMm15THl0Z3pmNVNDVjhnUGNtZTlLYlY0VXRjWVp1VlpqNHNBTUtQR2czMmo0aWdIc0dTRnJlZVovdmlqZWxNNnFNVlpVa3RQMkswNnhkdVJ4c2c4aDNXSGM5Z3BtRHpyeU5VWlNxa29LL25UcUN3aVFBNnBoMVpKUk9mNHFaY0xpQVAwTHF1SUs3aTBnbUwzTlNKU2JBLzdmaHduemVoM2ErajlXYXJZZmRjYlNORDN3V05XR1k3NEp2Wm9IMktpS1ZaRUI0bjZCbDRPbWtaME9FQ3cwYmpSdG1MbUthTWtGbVJzWFV0VDFadElCT2lnU1JwNmk1eWduNzhOQldqIl19.eyJkYXRlVGltZSI6IjIwMjUtMDUtMDdUMTE6MzA6MTJaIiwiZGV2aWNlU3ViVHlwZSI6IlNsYXAiLCJtb2RlbCI6IlJlYWxTY2FuLUcxMCIsInR5cGUiOiJGaW5nZXIiLCJtYWtlIjoiU3VwcmVtYSBJRCIsInNlcmlhbE5vIjoiOTM5NjUzMDI5IiwiZGV2aWNlUHJvdmlkZXIiOiJOUHJpbWUgVGVjaG5vbG9naWVzIFByaXZhdGUgTGltaXRlZCIsImRldmljZVByb3ZpZGVySWQiOiJOUHJpbWVEUEtNUyJ9.Pm_TEw9AGM8dmInCvsw8vZBbJQppHMFX6aCotMZovWqg2RaIn5poQpCctzdOsR5kDED5pvIlahEF0cal0xA9EWus0i-wPpB7p4WJPKHHN7XSPOzkb5bopDZSHbfQ1_RyK6v8LQDAQIAEvhuk1xtfdw5LjEMpk3K8D5uBx7GsfJSRdkrCRvjmyndtShn7g1XiRbpelkb5sFp91QKztkge_X5YQY1oq8hw2J-1sHPknUP4BDc1vVo62TQrY8OOYLMdt3zu1hl1iVfXJvXv0DYSwZ0v5t9pC_D9TvUcEEXg5iTsRXwDcCuOjX74mquea6bXQlcNEF0e8Dmw-53SR9Yceg\",\"deviceCode\":\"939653029\",\"deviceServiceVersion\":\"0.9.5.8\",\"bioSubType\":\"Right Thumb\",\"purpose\":\"Registration\",\"env\":\"Staging\",\"bioType\":\"Finger\",\"bioValue\":\"<bioValue>\",\"transactionId\":\"a9d50790-915e-4945-93a4-75493dfb3308\",\"timestamp\":\"2025-05-07T11:30:12Z\",\"requestedScore\":\"40\",\"qualityScore\":\"90\"}",
					"SDK_SCORE": "90.0"
				}
			},
		
		],
		"others": {}
	},
	"modalitiesToExtract": [
		"FINGER"
	],
	"flags": {
		"finger.format": "fingerprint"
	}
}


```

#### Sample Response - Just One Finger Response.

    - [`Sample_Finger_JustOneFinger_Response.json`](./SampleRequests&Responses/Sample_Finger_JustOneFinger_Response.json)

```json

{
	"version": "0.9",
	"responsetime": "2025-05-07T13:33:05.251Z",
	"response": {
		"statusCode": 200,
		"statusMessage": "OK",
		"response": {
			"version": null,
			"cbeffversion": null,
			"birInfo": null,
			"segments": [
				{
					"version": {
						"major": 1,
						"minor": 1
					},
					"cbeffversion": {
						"major": 1,
						"minor": 1
					},
					"birInfo": {
						"creator": null,
						"index": null,
						"payload": null,
						"integrity": false,
						"creationDate": null,
						"notValidBefore": null,
						"notValidAfter": null
					},
					"bdbInfo": {
						"challengeResponse": null,
						"index": "6aa4ee19-2106-4691-95e8-edbd0ad65f2f",
						"format": {
							"organization": "Mosip",
							"type": "7"
						},
						"encryption": null,
						"creationDate": {
							"date": {
								"year": 2025,
								"month": 5,
								"day": 7
							},
							"time": {
								"hour": 11,
								"minute": 35,
								"second": 34,
								"nano": 247309200
							}
						},
						"notValidBefore": null,
						"notValidAfter": null,
						"type": [
							"FINGER"
						],
						"subtype": [
							"Right",
							"Thumb"
						],
						"level": "PROCESSED",
						"product": null,
						"captureDevice": null,
						"featureExtractionAlgorithm": null,
						"comparisonAlgorithm": null,
						"compressionAlgorithm": null,
						"purpose": "VERIFY",
						"quality": {
							"algorithm": {
								"organization": "HMAC",
								"type": "SHA-256"
							},
							"score": 90,
							"qualityCalculationFailed": null
						}
					},
					"bdb": [], // Sample BDB data
					"sb": null,
					"birs": null,
					"sbInfo": null,
					"others": {}
				}
			],
			"others": {}
		}
	},
	"errors": []
}

```

### Face

#### Face Request

    - [`face_request.json`](./SampleRequests&Responses/face_request.json)

```json

{
	"sample": {
		"segments": [
			{
				"version": {
					"major": 1,
					"minor": 1
				},
				"cbeffversion": {
					"major": 1,
					"minor": 1
				},
				"birInfo": {
					"integrity": false
				},
				"bdbInfo": {
					"index": "b71dc768-0e49-4ea1-a71f-3d15637a87b2",
					"format": {
						"organization": "Mosip",
						"type": "8"
					},
					"creationDate": {
						"date": {
							"year": 2025,
							"month": 5,
							"day": 7
						},
						"time": {
							"hour": 11,
							"minute": 38,
							"second": 11,
							"nano": 683644600
						}
					},
					"type": [
						"FACE"
					],
					"subtype": [],
					"level": "RAW",
					"purpose": "ENROLL",
					"quality": {
						"algorithm": {
							"organization": "HMAC",
							"type": "SHA-256"
						},
						"score": 66
					}
				},
				"bdb": [], //Sample BDB data
				"sb": [], //Sample SB data
				"others": {
					"SPEC_VERSION": "0.9.5",
					"RETRIES": "1",
					"FORCE_CAPTURED": "false",
					"EXCEPTION": "false",
					"PAYLOAD": "{\"digitalId\":\"eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsIng1YyI6WyJNSUlEeURDQ0FyQ2dBd0lCQWdJR0FaWmM1NFVyTUEwR0NTcUdTSWIzRFFFQkN3VUFNSUdWTVFzd0NRWURWUVFHRXdKSlRqRVNNQkFHQTFVRUNBd0pWRVZNUVU1SFFVNUJNUkl3RUFZRFZRUUhEQWxJV1VSRlVrRkNRVVF4TERBcUJnTlZCQW9NSTA1UWNtbHRaU0JVWldOb2JtOXNiMmRwWlhNZ1VISnBkbUYwWlNCTWFXMXBkR1ZrTVJ3d0dnWURWUVFMREJOT1VISnBiV1VnVkdWamFHNXZiRzluYVdWek1SSXdFQVlEVlFRRERBbE9VSEpwYldWRVVGTXdIaGNOTWpVd05ESXlNRGswT1RVNFdoY05NalV3TmpJeE1EazBPVFU0V2pDQmxERXRNQ3NHQTFVRUF4TWtURVl6TURBdE1qZ3lSVGc1TVRnM1EwTXdMa3hsYm05MmJ5QkdTRVFnVjJWaVkyRnRNUkF3RGdZRFZRUUxFd2RPVUZKVVpXTm9NUnd3R2dZRFZRUUtFeE5PVUhKcGJXVWdWR1ZqYUc1dmJHOW5hV1Z6TVJJd0VBWURWUVFIRXdsSWVXUmxjbUZpWVdReEVqQVFCZ05WQkFnVENWUmxiR0Z1WjJGdVlURUxNQWtHQTFVRUJoTUNTVTR3Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3QXdnZ0VLQW9JQkFRQ3k5S0I5cFNxak1Dd1Y5eWl4NjBjQ2dlNTJvcjJkSDZrYms1bmFoQVo0SXNKeWpZZEc0VjlxR3F4N2Jrd2VGWnBHVjZKSjNaSHZXS2NwdVlFMTlxSXJ5S1ZMVHNlVXFaaUNyNjZiTWlKcU9DVzVhWTI5ajF2bi9jaVJVNlZzdzJWaHZLZ3dVaFArUHc4UmpKUFQ5dUVuNHdoUWVEeCtFUHhYcldTZTFqczhGWTg1OGNBMmNNVFJVbmwxYWJjS3NSUmpBd01RaVZSOTBwOVphWFh2WlZQVFBjbWViMnNXMi9oODJEbjhmRTJKMm10ZFFGdVhDQ1ZGWGxxSlIvS2NtdE9sWjJGZk9JdlI3OFRQUkIxcTlaVHVsQnVGMnRRVEdvMnk3MEZTMHpnSksveTBFcktSWUlua2M3YzM1T20wOTJWMXNmYTJHZ21VRmU5Q1ZKU1FYVDBQQWdNQkFBR2pIVEFiTUF3R0ExVWRFd1FGTUFNQkFmOHdDd1lEVlIwUEJBUURBZ0dHTUEwR0NTcUdTSWIzRFFFQkN3VUFBNElCQVFBZW9icFR6RGpJeXAyS041SW1pZXU1dmpoWnVXcTJJRHJVWGtIT1VRMzcvaFJES1JqZUhGdmJEbFJDNk5JaThUUjlEM1dzOUw2dUkydFNSOGY2RW80NkY3ajNwNTIyclpxcmtqZVdyYkNhUXM1RGNJTjBDblBFTWwrcnFvNUpPbit1bGF5M2dyMHVQaWEveVZyTkFHRjZqZzdMakZpODQzUWFCMzFHRDF1TDlhbE4zNWZqU1RYNXVkaDVhN05KL0ZSK2xpc2hYUVNPVlRKMmVBeUxseTRsOXR0ZG9aU3E5ME16NlNMOW05WmJRdkRlTDdOMC9JV1N2eUxUUDBMMHdoOE9ueGtVZU5sMkxKM2xLdk8rYnBIQUh6MnhzTjF5VjFsSEhoVVRFR2thYzRid0NuR1hIc3UwK2FPZEtDSFZDaGxYZ0kxemVYQmczWnZNZmNJU2hjMWoiXX0.eyJkYXRlVGltZSI6IjIwMjUtMDUtMDdUMTE6Mzc6NDJaIiwiZGV2aWNlU3ViVHlwZSI6IkZ1bGwgZmFjZSIsIm1vZGVsIjoiTGVub3ZvIEZIRCBXZWJjYW0iLCJ0eXBlIjoiRmFjZSIsIm1ha2UiOiJMZW5vdm8iLCJzZXJpYWxObyI6IkxGMzAwLTI4MkU4OTE4N0NDMCIsImRldmljZVByb3ZpZGVyIjoiTlByaW1lIFRlY2hub2xvZ2llcyBQdnQuIEx0ZC4iLCJkZXZpY2VQcm92aWRlcklkIjoiTlByaW1lRFBTIn0.Op8f1FlvrHkvtsM7Z5Cl3dQZ7twVfYowlQ_854lEQkrVbVNWMmef-2cRCTY8a6A4c0l-fkDFCXlR6nGG7KsYhettp3mEz0XRcIziGagR5xNePfOUw61M6qor4ImcD_HPd3mDGnBHkp1GsThVSenlePfbnCXuS7Y5M9dFwDG8PMVi8eh6VZQksZj8pdinpSgVIsv1A8OdXrLiY4ORpHO6sEJ2_nH5URmmKI8bws9RwIiFalE-GlJop6D4EP2zCVli8qe6elhVyvijO1Xfywqb5E623pEYFb95MeTohYb18TKSZCHFgjHXxLSSBtmZs8D4h2lvZAoAjfvVhF2Fh_35kg\",\"deviceCode\":\"LF300-282E89187CC0\",\"deviceServiceVersion\":\"0.9.5.2\",\"bioSubType\":\"null\",\"purpose\":\"Registration\",\"env\":\"Staging\",\"bioType\":\"Face\",\"bioValue\":\"<bioValue>\",\"transactionId\":\"0c669587-08e7-48ea-9bea-9b61438a4b5b\",\"timestamp\":\"2025-05-07T11:37:42Z\",\"requestedScore\":\"60\",\"qualityScore\":\"66\"}",
					"SDK_SCORE": "0.0"
				}
			}
		],
		"others": {}
	},
	"modalitiesToExtract": [
		"FACE"
	],
	"flags": {
		"face.format": "face"
	}
}


```

#### Face Response

    - [`face_response.json`](./SampleRequests&Responses/face_response.json)

```json

{
	"version": "0.9",
	"responsetime": "2025-05-07T13:36:33.796Z",
	"response": {
		"statusCode": 200,
		"statusMessage": "OK",
		"response": {
			"version": null,
			"cbeffversion": null,
			"birInfo": null,
			"segments": [
				{
					"version": {
						"major": 1,
						"minor": 1
					},
					"cbeffversion": {
						"major": 1,
						"minor": 1
					},
					"birInfo": {
						"creator": null,
						"index": null,
						"payload": null,
						"integrity": false,
						"creationDate": null,
						"notValidBefore": null,
						"notValidAfter": null
					},
					"bdbInfo": {
						"challengeResponse": null,
						"index": "b71dc768-0e49-4ea1-a71f-3d15637a87b2",
						"format": {
							"organization": "Mosip",
							"type": "8"
						},
						"encryption": null,
						"creationDate": {
							"date": {
								"year": 2025,
								"month": 5,
								"day": 7
							},
							"time": {
								"hour": 11,
								"minute": 38,
								"second": 11,
								"nano": 683644600
							}
						},
						"notValidBefore": null,
						"notValidAfter": null,
						"type": [
							"FACE"
						],
						"subtype": [],
						"level": "RAW",
						"product": null,
						"captureDevice": null,
						"featureExtractionAlgorithm": null,
						"comparisonAlgorithm": null,
						"compressionAlgorithm": null,
						"purpose": "ENROLL",
						"quality": {
							"algorithm": {
								"organization": "HMAC",
								"type": "SHA-256"
							},
							"score": 66,
							"qualityCalculationFailed": null
						}
					},
					"bdb": [], // Sample BDB data
					"sb": null,
					"birs": null,
					"sbInfo": null,
					"others": {}
				}
			],
			"others": {}
		}
	},
	"errors": []
}

```

---

## 5. Field Reference

- **Field Descriptions:** Table or list explaining each field in request/response JSON.
- **Special Notes:**  
    - `level: PROCESSED`
    - `purpose: VERIFY`
    - Error handling details

## 6. Error Handling

- **Error Structure:** Describe the structure of error responses.
- **Error Codes:** List possible error codes and their meanings.

## 7. Appendix

- **Sample Files:**  
    [SampleRequests&Responses Directory](./SampleRequests&Responses/)
- **Versioning & Changelog:** Track changes and versions.
- **Additional Tips:**  
    - For each scenario, show both request and response samples.
    - Use code blocks for JSON snippets and provide links to full files.
    - Clearly explain fields, especially those that change in exception scenarios.
    - Reference sample files directly for clarity.

---


Glossary: `bdb` stands for "Biometric Data Block". It contains the actual biometric data (such as the encoded iris, fingerprint, or face image/template) captured from the device. In the sample, it is shown as an empty array (`[]`) for illustration, but in real scenarios, this field will contain the binary or encoded biometric data specific to the modality and subtype (e.g., Left Iris). "sb": null