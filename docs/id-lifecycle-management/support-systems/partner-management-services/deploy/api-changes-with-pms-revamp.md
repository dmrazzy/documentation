# API changes with PMS Revamp

## API changes with PMS Revamp

This document captures all the changes that have been made in the API endpoints during the PMS Revamp Releases. These changes include addition of new endpoints, deprecation of a few endpoints and also some other changes.

## PMS API Endpoints Documentation

### /oauth/client (GET)

**Description:** This endpoint retrieves a list of all OAuth clients created by the Auth Partners. It supports pagination, sorting, and and filtering based on optional query parameters. If the token used to access this endpoint, does not have the PARTNER\_ADMIN role, then it will fetch all the OAuth clients created by all the partners associated with the logged in user only. If the token used to access this endpoint, has PARTNER\_ADMIN role, then it will fetch all the OAuth clients created by all the partners. It is configured for **PARTNER\_ADMIN** and **AUTH\_PARTNER** roles.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: sortFieldName, sortType, partnerId, orgName, policyGroupName, policyName, clientName, status

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /oauth/client (POST)

**Description:** This endpoint is used for creating OIDC Client.

**Changes done in release 1.2.2.0:**

1. Added validation to check the partner id in the request body belongs to the user who's token is being used to access this endpoint. This will ensure that PMS user can create OIDC client only for the partner id which belongs to the user. This validation is skipped if the user's role is **PARTNER\_ADMIN**.
2. Added validation to check if the **Partner ID** used in the request body is active. This will ensure that OIDC client cannot be created for an inactive partner. ([MOSIP-34276](https://mosip.atlassian.net/browse/MOSIP-34276))
3. If multiple policy requests were created by the partner for a policy, then while creating the OIDC client, this endpoint was checking the status of only the first policy request. So even if there was an approved policy request, it was still throwing an error. Fixed this bug ([MOSIP-34599](https://mosip.atlassian.net/browse/MOSIP-34599))
4. Improved JWK validation for the public key by adding validation that n value (modulus value) of the JWK must be unique ([MOSIP-36219](https://mosip.atlassian.net/browse/MOSIP-36219))
5. Updated client name to be a JSON string to support client name language map ([ES-836](https://mosip.atlassian.net/browse/ES-836))

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /oauth/client/{client\_id} (GET)

**Description:** This endpoint retrieves the OIDC client details by client id

**Changes done in release 1.2.2.0:**

1. Added validation to check the partner id in the request belongs to the user who's token is being used to access this endpoint. This will ensure that PMS user can access OIDC client only for the partner id which belongs to the user. This validation is skipped if the user's role is **PARTNER\_ADMIN**.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /oauth/client/{client\_id} (PUT)

**Description:** This endpoint is used for updating OIDC Client based on client id

**Changes done in release 1.2.2.0:**

1. Added validation to check the partner id in the request body belongs to the user who's token is being used to access this endpoint. This will ensure that PMS user can update OIDC client only for the partner id which belongs to the user. This validation is skipped if the user's role is **PARTNER\_ADMIN**.
2. Added validation to check if the **Partner ID** used in the request body is active. This will ensure that OIDC client cannot be updated for an inactive partner. This validation is skipped if the user's role is **PARTNER\_ADMIN** and status in the request is changed to **INACTIVE**. ([MOSIP-34276](https://mosip.atlassian.net/browse/MOSIP-34276))
3. If the status in the request is changed to **INACTIVE**, only the status is updated in the database other fields remain unchanged. This will ensure that PUT endpoint can be used to deactivate the OIDC client.
4. Added a bypass for a user with **PARTNER\_ADMIN** role. If the user with **PARTNER\_ADMIN** role is used to access this endpoint, then it will deactivate the OIDC client for any partner ID, even if the partner ID is deactivated.
5. Added a validation to check if the OIDC client is already deactivated.([MOSIP-34108](https://mosip.atlassian.net/browse/MOSIP-34108))
6. Updated client name to be a JSON string to support client name language map ([ES-836](https://mosip.atlassian.net/browse/ES-836))

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /devicedetail (GET)

**Description:** This endpoint retrieves a list of all the Devices across all the Device Providers in PMS. It supports pagination, sorting, and filtering. It is configured for the role **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: sortFieldName, sortType, partnerId, orgName, deviceType, deviceSubType, status, make, model, sbiId, sbiVersion, deviceId

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /devicedetail (PUT)

**Description:** Service to update Device Detail

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /devicedetail (POST)

**Description:** Service to save Device Detail

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new POST /securebiometricinterface/{sbiId}/devices endpoint

This ensures that a device will always be created for a SBI and not without one.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3**: Fixed the Random ID generator issue by introducing the `mosip.pms.id.generation.max.retries` property, which allows the system to retry ID generation until a unique value is found in the database, thereby preventing conflicts. \[MOSIP-42232]

### /devicedetail (PATCH)

**Description:** Service to approve/reject Device Detail

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new POST devicedetail/{id}/approval endpoint

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /devicedetail/{deviceId} (PATCH)

**Description:** This endpoint deactivates a Device based on the Device Id. It is configured for the roles **DEVICE\_PROVIDER** or **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: deviceId, status

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /devicedetail/{id}/approval (POST)

**Description:** This endpoint is for the Partner Admin user to approve or reject a Device and activate the mapping between the Device and the SBI. It is configured for the role **PARTNER\_ADMIN**

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: sbiId, partnerId, deviceId, status

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /devicedetail/deviceSubType/filtervalues (POST)

**Description:** Service to filter Device Sub Types

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /devicedetail/deviceType/filtervalues (POST)

**Description:** Service to filter Device Types

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /devicedetail/deviceType/search (POST)

**Description:** Service to search Device Types

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /devicedetail/filtervalues (POST)

**Description:** Service to filter Device Detail

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /devicedetail endpoint

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /devicedetail/search (POST)

**Description:** Service to search Device Detail

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /devicedetail endpoint

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /ftpchipdetail (GET)

**Description:** This endpoint retrieves a list of all FTM Chip details created by all the FTM Providers associated with the logged in user. It is configured for the roles **FTM\_PROVIDER** or **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added an optional query parameter expiryPeriod

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /ftpchipdetail (PUT)

**Description:** Service to update ftp chip detail

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /ftpchipdetail (POST)

**Description:** Service to save ftp chip detail

**Changes done in release 1.2.2.0:**

1. Improved the validation check by trimming extra spaces in make and model to prevent duplicate entries. ([MOSIP-35788](https://mosip.atlassian.net/browse/MOSIP-35788))

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3**:

Fixed the Random ID generator issue by introducing the `mosip.pms.id.generation.max.retries` property, which allows the system to retry ID generation until a unique value is found in the database, thereby preventing conflicts. \[MOSIP-42232]

### /ftpchipdetail (PATCH)

**Description:** Service to approve/reject ftp chip detail

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /ftpchipdetail/{ftmId} (PATCH)

**Description:** This endpoint deactivates the ftp chip detail based on the ftp chip detail Id. It is configured for the roles **FTM\_PROVIDER** or **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /ftpchipdetail/{ftmId}/certificate-data (GET)

**Description:** This endpoint fetches both the CA signed certificate uploaded by the FTM Chip Provider and the MOSIP signed certificate generated by PMS. It is configured for the roles **FTM\_PROVIDER** or **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following field: ftmId

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /ftpchipdetail/getPartnerCertificate/{ftpChipDetailId} (GET)

**Description:** Service to get certificate of ftp chip

**Changes done in release 1.2.2.0:** Improved Key Manager error handling, to capture the correct error code from Key Manager and send it in the endpoint's response.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /ftpchipdetail/search (POST)

**Description:** Service to search ftp chip details

**Changes done in release 1.2.2.0:**

1. This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /ftpchipdetail/v2 endpoint.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /ftpchipdetail/uploadcertificate (POST)

**Description:** Service to upload certificate of ftp chip

**Changes done in release 1.2.2.0:**

1. Added validation to allow certificate upload only if the FTM chip details certificate status is **APPROVED** or **PENDING\_CERT\_UPLOAD**.([MOSIP-36283](https://mosip.atlassian.net/browse/MOSIP-36283)). So for Rejected or Deactivated FTM, a certificate cannot be uploaded.
2. Improved Key Manager error handling, to capture the correct error code from Key Manager and send it in the endpoint's response.
3. Set isActive to false after certificate re-upload. This will ensure that after cert is reuploaded, partner admin will have to approve the FTM again. ([MOSIP-36285](https://mosip.atlassian.net/browse/MOSIP-36285))

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /ftpchipdetail/v2 (GET)

**Description:** This endpoint retrieves a list of all FTM Chip details created by all the FTM Providers. Also supports pagination, sorting, and filtering. It is configured for the role **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: ftmId, status

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /admin-partners (GET)

**Description:** This endpoint retrieves a list of all Partners. Also supports pagination, sorting, and filtering. It is configured for the role **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

* Disabled sorting on the email\_id column due to encryption-related limitations.
* Modified filtering behavior for the email\_id column: now supports only exact match filtering; partial or "contains" search is no longer supported.

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: sortFieldName, sortType, partnerId, partnerType, orgName, emailAddress, certificateUploadStatus, policyGroupName

**Changes done in release 1.3.0-beta.3**:

A new query parameter called `status` has been introduced, enabling filtering based on the partner's approval status. Supported values for this parameter are: `active`, `deactivated`, and `inactive`.

### /admin-partners/{partnerId} (GET)

**Description:** This endpoint retrieves all the details of the Partner based on Partner Id. It is configured for the role **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following field: partnerId

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partner-api-keys (GET)

**Description:** This endpoint retrieves a list of all the API keys created by the Auth Partners. Also supports pagination, sorting, and and filtering based on optional query parameters. If the token used to access this endpoint, does not have the **PARTNER\_ADMIN** role, then it will fetch all the API keys created by all the partners associated with the logged in user only. If the token used to access this endpoint, has **PARTNER\_ADMIN** role, then it will fetch all the API keys created by all the partners.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:**

1. This endpoint has been deprecated since the release-1.3.0-beta.2. It has been replaced by the new GET /partner-api-keys/v2 endpoint.
2. Also added input regex validation for the following fields: sortFieldName, sortType, partnerId, apiKeyLabel, orgName, status, policyName, policyGroupName

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partner-api-keys/v2 (GET)

**Description:** This endpoint retrieves a list of all the API keys created by the Auth Partners. Also supports pagination, sorting, and and filtering based on optional query parameters. If the token used to access this endpoint, does not have the **PARTNER\_ADMIN** role, then it will fetch all the API keys created by all the partners associated with the logged in user only. If the token used to access this endpoint, has **PARTNER\_ADMIN** role, then it will fetch all the API keys created by all the partners.

**Changes done in release 1.2.2.0:** -

**Changes done in release 1.3.0-beta.1:** -

**Changes done in release 1.3.0-beta.2:**

1. Newly added in release 1.3.0-beta.2
2. Also added input regex validation for the following fields: sortFieldName, sortType, partnerId, apiKeyLabel, orgName, status, policyName, policyGroupName

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partner-policy-requests (GET)

**Description:** This endpoint fetches list of all the policy requests made by the partners. Also supports pagination, sorting, and filtering based on optional query parameters. If the token used to access this endpoint, does not have the **PARTNER\_ADMIN** role, then it will fetch all the policy requests made by all the partners associated with the logged in user only.If the token used to access this endpoint, has **PARTNER\_ADMIN** role, then it will fetch all the policy requests made by all the partners.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: sortFieldName, sortType, partnerId, partnerComment, orgName, status, policyId, policyName, policyGroupName, partnerType

**Changes done in release 1.3.0-beta.3**: A new optional query parameter `partnerIdSearchType` has been introduced to specify the search type for `partnerId`. This parameter is applicable only when `partnerId` is provided. Supported values are `contains` (default) and `equals`.

### /partners (GET)

**Description:** Service to get partner details

**Changes done in release 1.2.2.0:**

1. This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /partners/v3 endpoint.

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/{partnerId} (PATCH)

**Description:** Service to activate/de-activate partner

**Changes done in release 1.2.2.0:**

1. Added a check to verify if the **partner** is already deactivated. If yes, partner cannot be deactivated again. ([MOSIP-37017](https://mosip.atlassian.net/browse/MOSIP-37017))

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/{partnerId}/apikey/{apikey} (GET)

**Description:** Service to get policy for given API key

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/{partnerId}/apikey/{apikey}/policies (PUT)

**Description:** Service to update policies against to API key

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/{partnerId}/policy/{policyId}/apikey/status (PATCH)

**Description:** Service to activate/de-activate partner API key

**Changes done in release 1.2.2.0:**

1. If the API key is already deactivated, it cannot be deactivated again.([MOSIP-34430](https://mosip.atlassian.net/browse/MOSIP-34430))
2. Added a validation to check if the **Partner ID** used in the request body is active. This will ensure that API cannot be deactivated if partner has been deactivated. This validation is skipped if the user's role is **PARTNER\_ADMIN**. ([MOSIP-34430](https://mosip.atlassian.net/browse/MOSIP-34430))

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/apikey (GET)

**Description:** Service to get API key requests

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /partner-policy-requests endpoint.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/apikey/{apikey} (GET)

**Description:** Service to get API key request

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/policy/{mappingkey} (PUT)

**Description:** Service to approve/reject partner policy mapping

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/v2 (GET)

**Description:** Service to get partner details

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /partners/v3 endpoint.

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /trust-chain-certificates (GET)

**Description:** This endpoint retrieves a list of all the Trust Certificates uploaded by the Partner Admin. Also supports pagination, sorting, and filtering. It is configured for the role **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: sortFieldName, sortType, caCertificateType, certificateId, partnerDomain, issuedTo, issuedBy

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /trust-chain-certificates/{certificateId}/certificateFile (GET)

**Description:** This endpoint will download p7b file for a CA / Intermediate CA certificate along with the trust chain based on Certificate Id. It is configured for the role **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following field: certificateId

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners (POST)

**Description:** partner self registration

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3**: This endpoint has been deprecated since release 1.3.0-beta.3 and replaced by the new POST `/partners/v3` endpoint.

### /partners/v3 (POST)

This endpoint is used for partner self registration. Newly added in release-1.3.0-beta.3

### /partners/exists (PUT)

This endpoint checks whether a partner already exists in PMS by validating duplicates based on the provided email and partner ID, and returns a conflict if either is already registered. Newly added in release-1.3.0-beta.3

### partners/{partnerId}/policy-group (POST)

This endpoint is used to link a policy group to a MISP partner. It is configured for users with the PARTNER\_ADMIN role. Newly added in release-1.3.0-beta.3

### /partners/{partnerId} (GET)

**Description:** Service to get details of partner

**Changes done in release 1.2.2.0:** Corrected the version in the response body

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/{partnerId} (PUT)

**Description:** Service to update details of partner

**Changes done in release 1.2.2.0:**

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/{partnerId}/apikey/request (GET)

**Description:** Service to get API key requests of partner

**Changes done in release 1.2.2.0:** Corrected the version in the response body

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/{partnerId}/certificate (GET)

**Description:** Service to get partner certificate

**Changes done in release 1.2.2.0:**

1. Added a check to verify if the partner id in the request exists in the database.([MOSIP-37017](https://mosip.atlassian.net/browse/MOSIP-37017))
2. Added validation to check if the certificate has been uploaded previously.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/{partnerId}/certificate-data (GET)

**Description:** This endpoint retrieves both the CA signed certificate uploaded by the partner and the MOSIP-signed certificate generated by PMS. It is configured for role any of the partner type or **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following field: certificateId

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/{partnerId}/contact/add (POST)

**Description:** Service to add additional contact deatils of partner

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3**: Fixed the Random ID generator issue by introducing the `mosip.pms.id.generation.max.retries` property, which allows the system to retry ID generation until a unique value is found in the database, thereby preventing conflicts. \[MOSIP-42232]

### /partners/{partnerId}/generate/apikey (PATCH)

**Description:** To generate API Key for approved policies

**Changes done in release 1.2.2.0:** Added a check to remove extra spaces in the API key label before saving to the database, preventing the creation of duplicate API key labels with extra spaces.([MOSIP-35788](https://mosip.atlassian.net/browse/MOSIP-35788))

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3**: Fixed the Random ID generator issue by introducing the `mosip.pms.id.generation.max.retries` property, which allows the system to retry ID generation until a unique value is found in the database, thereby preventing conflicts. \[MOSIP-42232]

### /partners/{partnerId}/policy/map (POST)

**Description:** To request for policy mapping

**Changes done in release 1.2.2.0:** Updated error messages to indicate if the policy is already mapped and its status is **Approved** or **In Progress**.([MOSIP-33803](https://mosip.atlassian.net/browse/MOSIP-33803))

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3**: Fixed the Random ID generator issue by introducing the `mosip.pms.id.generation.max.retries` property, which allows the system to retry ID generation until a unique value is found in the database, thereby preventing conflicts. \[MOSIP-42232]

### /partners/{partnerId}/policygroup/{policygroupName} (PUT)

**Description:** Service to update the policy group for partner

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/apikey/request/filtervalues (POST)

**Description:** Service to filter API key requests

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /partner-policy-requests endpoint.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/apikey/request/search (POST)

**Description:** Service to search API key requests

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /partner-policy-requests endpoint.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/apikey/search (POST)

**Description:** Service to search API key

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /partner-api-keys endpoint.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/certificate/ca/upload (POST)

**Description:** Service to upload ca certificate

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/certificate/upload (POST)

**Description:** Service to upload partner certificate

**Changes done in release 1.2.2.0:**

1. Added a validation to check if the **Partner ID** used in the request body is active. This will ensure that certificate cannot be uploaded if partner has been deactivated. ([MOSIP-34498](https://mosip.atlassian.net/browse/MOSIP-34498))
2. If domain is **FTM**, do not call the uploadOtherDomainCertificate endpoint of KeyManager.([MOSIP-35797](https://mosip.atlassian.net/browse/MOSIP-35797))

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/email/verify (PUT)

**Description:** Service to verify partner email

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/filtervalues (POST)

**Description:** Service to filter partner details

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /partners/v3 endpoint.

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

* Support for email\_id\*\*, contact\_no\*\*, and address fields has been removed due to encryption constraints.

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/partner-certificates-details (GET)

**Description:** This endpoint retrieves a list of all Partner Certicates uploaded by the logged in user

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /partners/partnerType/search (POST)

**Description:** Service to search partner types

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### partners/search (POST)

**Description:** Service to search partner details

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /partners/v3 endpoint.

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

* Support for email\_id\*\*, contact\_no\*\*, and address columns has been removed due to encryption constraints.

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### partners/v2 (POST)

**Description:** Registers partner details

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3**: This endpoint has been deprecated since release 1.3.0-beta.3 and replaced by the new POST `/partners/v3` endpoint.

### partners/v2/{partnerId} (PUT)

**Description:** Service to update details of partner

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** Handled encryption and decryption for PII columns([MOSIP-38061](https://mosip.atlassian.net/browse/MOSIP-38061))

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### partners/v3 (GET)

**Description:** This endpoint retrieves a list of Partners associated with the logged in user, based on the query parameters. It is configured for role any of the partner type or **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: status, partnerType

**Changes done in release 1.3.0-beta.3**: If the `partnerType` query parameter is set to `MISP_Partner`, this endpoint returns all MISP partners in the system, regardless of user association. In this scenario, access is restricted to users with the `PARTNER_ADMIN` role.

### /roles (GET)

**Description:** Service to get required roles

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /securebiometricinterface (GET)

**Description:** This endpoint retrieves a list of all SBIs created by the Device Providers. Also supports pagination, sorting, and and filtering based on optional query parameters. If the token used to access this endpoint, does not have the **PARTNER\_ADMIN** role, then it will fetch all SBIs created by all the partners associated with the logged in user only. If the token used to access this endpoint, has **PARTNER\_ADMIN** role, then it will fetch all the SBIs created by all the partners.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:**

1. Added input regex validation for the following fields: sortFieldName, sortType, partnerId, orgName, sbiId, sbiVersion, status, sbiExpiryStatus
2. Included an optional query parameter expiryPeriod

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /securebiometricinterface (PUT)

**Description:** Service to update SecureBiometricInterface

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /securebiometricinterface (POST)

**Description:** Service to save SecureBiometricInterface details

**Changes done in release 1.2.2.0:** Added a check to remove extra spaces in the SBI version before saving to the database, preventing the creation of duplicate SBI versions with extra spaces.([MOSIP-35788](https://mosip.atlassian.net/browse/MOSIP-35788))

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /securebiometricinterface (PATCH)

**Description:** Service to approve/reject SecureBiometricInterface

**Changes done in release 1.2.2.0:** Added separate error codes for cases when SBI is already **approved** or **rejected**.([MOSIP-38973](https://mosip.atlassian.net/browse/MOSIP-38973))

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /securebiometricinterface/{sbiId} (PATCH)

**Description:** This endpoint deactivates an SBI along with associated Devices. It is configured for the roles **DEVICE\_PROVIDER** or **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: sbiId, status

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /securebiometricinterface/{sbiId}/devices (GET)

**Description:** This endpoint fetches the list of Devices associated with a given SBI Id. It is configured for the roles **DEVICE\_PROVIDER** or **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following field: sbiId

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /securebiometricinterface/{sbiId}/devices (POST)

**Description:** This endpoint adds a new Device and creates an inactive mapping between the device and the given SBI. It is configured for the roles **DEVICE\_PROVIDER** or **PARTNER\_ADMIN**.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: sbiId, id, make, model, deviceTypeCode, deviceSubTypeCode

**Changes done in release 1.3.0-beta.3**: Fixed the Random ID generator issue by introducing the `mosip.pms.id.generation.max.retries` property, which allows the system to retry ID generation until a unique value is found in the database, thereby preventing conflicts. \[MOSIP-42232]

### /securebiometricinterface/devicedetails/map (PUT)

**Description:** Service to map device details with sbi

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new POST /securebiometricinterface/{sbiId}/devices endpoint.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /securebiometricinterface/devicedetails/map/remove (PUT)

**Description:** Service to remove mapped device details with sbi

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /securebiometricinterface/devicedetails/map/search (POST)

**Description:** Service to search mapped device details and SecureBiometricInterface details

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /securebiometricinterface/{sbiId}/devices endpoint.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /securebiometricinterface/filtervalues (POST)

**Description:** Service to filter SBI's

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /securebiometricinterface endpoint.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /securebiometricinterface/search (POST)

**Description:** Service to search SecureBiometricInterface details

**Changes done in release 1.2.2.0:** This endpoint has been deprecated since the release-1.2.2.0. It has been replaced by the new GET /securebiometricinterface endpoint.

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /system-config (GET)

**Description:** This endpoint fetches the configurations for PMS and sends them to the UI. No roles are required for access.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /users (POST)

**Description:** Service to register user

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /users/user-consent (GET)

**Description:** This endpoint fetches the user's consent related to the data captured by PMS. The consent is requested only once after the user's first login, and won't be asked again if already given. It is configured for all Partner Type roles.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /users/user-consent (POST)

**Description:** This endpoint saves the user's consent related to data captured by the PMS portal, which is requested only once after the user's first login. Once provided, the consent will not be asked again. It is configured for all Partner Type roles.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /users/{userId}/notifications-seen-timestamp (GET)

**Description:** This endpoint which will get the status and timestamp of when the notifications were last viewed by the user in the PMS portal.

**Changes done in release 1.2.2.0:** -

**Changes done in release 1.3.0-beta.1:** Newly added in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /users/{userId}/notifications-seen-timestamp (PUT)

**Description:** This endpoint which will update the status and timestamp of when the notifications were last viewed by the user in the PMS portal.

**Changes done in release 1.2.2.0:** -

**Changes done in release 1.3.0-beta.1:** Newly added in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /notifications (GET)

**Description:** This endpoint will get all the notifications from the pms.notifications table. Also supports pagination, sorting, and and filtering based on optional query parameters.

**Changes done in release 1.2.2.0:** -

**Changes done in release 1.3.0-beta.1:** Newly added in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: notificationStatus, notificationType, certificateId, expiryDate, issuedBy, issuedTo, partnerDomain, createdFromDate, createdToDate, ftmId, make, model, apiKeyName, policyName, sbiId, sbiVersion

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /notifications/{notificationId} (PATCH)

**Description:** This endpoint will be used to handle the dismiss action by the user.

**Changes done in release 1.2.2.0:** -

**Changes done in release 1.3.0-beta.1:** Newly added in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: notificationId, notificationStatus

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /misp-licenses (GET)

This endpoint retrieves all MISP license keys with support for pagination, sorting, and filtering. It is configured for users with the PARTNER\_ADMIN role. Newly added in release-1.3.0-beta.3

### /misp-licenses (POST)

This endpoint generates a MISP license key for a specified MISP partner. It supports creating multiple license keys for the same partner, allows setting an expiry date, and includes the licenseKeyName field to distinguish between different keys. It is configured for users with the PARTNER\_ADMIN role. Newly added in release-1.3.0-beta.3

### /misp-licenses/{partnerId} GET

This endpoint retrieves MISP license key details for a specified partner using the partnerId. It supports optional query parameters to filter or refine the results and is configured for users with the PARTNER\_ADMIN role. Newly added in release-1.3.0-beta.3

### /misp-licenses/{partnerId} (PUT)

This endpoint re-generates a MISP license key for a specified partner using the partnerId. It allows setting a new expiry date and issues a new license key for the requested partner. It is configured for users with the PARTNER\_ADMIN role. Newly added in release-1.3.0-beta.3

### /misp-licenses/{partnerId} (PATCH)

This endpoint deactivates the MISP license key for a specified partner using the partnerId. It is configured for users with the PARTNER\_ADMIN role. Newly added in release-1.3.0-beta.3

### /misps POST

This endpoint is used to generate a MISP license key for a specified MISP partner.

No changes made in release 1.2.2.0

No changes made in release 1.3.0-beta.1

No changes made in release 1.3.0-beta.2

This endpoint has been deprecated since the release-1.3.0-beta.3 and replaced by the POST /misp-licenses endpoint.

### /misps PUT

This endpoint is used to update the details of a MISP license key.

No changes made in release 1.2.2.0

No changes made in release 1.3.0-beta.1

No changes made in release 1.3.0-beta.2

This endpoint has been deprecated since the release-1.3.0-beta.3 and replaced by the PATCH /misp-licenses/{partnerId} endpoint.

### /misps GET

This endpoint is used to retrieve the details of a MISP license key.

No changes made in release 1.2.2.0

No changes made in release 1.3.0-beta.1

No changes made in release 1.3.0-beta.2

This endpoint has been deprecated since the release-1.3.0-beta.3 and replaced by the GET /misp-licenses endpoint.

### /misps/{mispId}/licenseKey  GET

This endpoint re-generates a MISP license key for the specified partner.

No changes made in release 1.2.2.0

No changes made in release 1.3.0-beta.1

No changes made in release 1.3.0-beta.2

This endpoint has been deprecated since the release-1.3.0-beta.3 and replaced by the PUT /misp-licenses/{partnerId} endpoint.

### /misps/filtervalues POST

This endpoint is used to filter misp details.

No changes made in release 1.2.2.0

No changes made in release 1.3.0-beta.1

No changes made in release 1.3.0-beta.2

This endpoint has been deprecated since the release-1.3.0-beta.3\
and replaced by the GET /misp-licenses endpoint.

### /misps/search POST

This endpoint is used to search misp details

No changes made in release 1.2.2.0

No changes made in release 1.3.0-beta.1

No changes made in release 1.3.0-beta.2

This endpoint has been deprecated since the release-1.3.0-beta.3\
and replaced by the GET /misp-licenses endpoint.



***

## Policy Management Service Endpoints

### /policies (GET)

**Description:** Service to get policies

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /policies (POST)

**Description:** Service to create a new authentication, data sharing, or credential policy.

**Changes done in release 1.2.2.0:** Handled missing 'Empty Array and Empty String' Schema Validation

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3**: Fixed the Random ID generator issue by introducing the `mosip.pms.id.generation.max.retries` property, which allows the system to retry ID generation until a unique value is found in the database, thereby preventing conflicts. \[MOSIP-42232]

### /policies/{policyId} (GET)

**Description:** Service to retrieve the details of a specific policy by its ID.

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /policies/{policyId} (PUT)

**Description:** Service to update policy details

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /policies/{policyId} (PATCH)

**Description:** This endpoint deactivates a policy based on the Policy Id. It checks if any policy requests are associated with the policy: it can be deactivated if there are no requests or if there are rejected requests. It cannot be deactivated if there are approved or pending requests, returning error codes PMS\_POL\_063 or PMS\_POL\_064, respectively. This endpoint is configured for the **POLICYMANAGER** or **PARTNER\_ADMIN** roles.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: policyId, status

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /policies/{policyId}/group/{policygroupId}/publish (POST)

**Description:** Service to publish policy

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /policies/active/group/{groupName} (GET)

**Description:** Service to get active policy details for policy group name

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /policies/group/{policygroupId} (GET)

**Description:** Service to get policy group

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /policies/group/{policyGroupId} (PATCH)

**Description:** Service for Partner Admin users to deactivate a Policy Group based on the Policy Group Id. It is configured for the **POLICYMANAGER** or **PARTNER\_ADMIN** roles.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: policyGroupId, status

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /policies/group/new (POST)

**Description:** Service to create a new policy group

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3**: Fixed the Random ID generator issue by introducing the `mosip.pms.id.generation.max.retries` property, which allows the system to retry ID generation until a unique value is found in the database, thereby preventing conflicts. \[MOSIP-42232]

### /policies/group/search (POST)

**Description:** Service to search policy group

**Changes done in release 1.2.2.0:** No changes made in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /policies/policy-groups (GET)

**Description:** Service to retrieve details about all active Policy Groups

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** No changes made in release 1.3.0-beta.2

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3

### /policies/v2 (GET)

**Description:** Service to retrieve the list of all Policies. It is configured for the **POLICYMANAGER** or **PARTNER\_ADMIN** roles.

**Changes done in release 1.2.2.0:** Newly added in release 1.2.2.0

**Changes done in release 1.3.0-beta.1:** No changes made in release 1.3.0-beta.1

**Changes done in release 1.3.0-beta.2:** Added input regex validation for the following fields: sortFieldName, sortType, policyType, policyId, policyName, policyDescription, policyGroupName, status

**Changes done in release 1.3.0-beta.3:** No changes made in release 1.3.0-beta.3
