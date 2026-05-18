# Partner Management System Revamp 1.3.0-beta.5

<mark style="color:red;">**Coming Soon**</mark>!

**Release Number:** 1.3.0-beta.5\
**Release Date:** <mark style="color:red;">**Coming Soon**</mark>!

### Overview

Partner Management System (PMS) Revamp 1.3.0-beta.5 introduces support for onboarding new partner types and includes enhancements to partner-policy validation behavior in the DataShare service. This release extends the PMS onboarding framework to support Credential Partners and Manual Adjudication Partners through complete end-to-end onboarding flows.

## Key Features & Enhancements

### 1. Support for Credential Partner Onboarding

This release introduces support for a new partner type called **Credential Partner** with [complete end-to-end onboarding](https://mosip.atlassian.net/wiki/spaces/PMS/pages/2582773932) capabilities in PMS.

#### Added support for:

* Partner self-registration support for Credential Partners.
* Policy request creation support for Partner Admins.
* Root CA upload support as part of Credential Partner onboarding.
* Policy request approval workflow support for Partner Admins.
* Biometric Extractor configuration creation support for Partner Admins.
* End-to-end onboarding lifecycle support for Credential Partners across PMS workflows.

**Note** : Credential Partner onboarding requires mandatory mapping of **Biometric Extractors** and **Credential Types** as part of the policy request workflow. To support this, two additional steps have been introduced in the policy request process for configuring these mappings.

The Partner Admin will not be able to approve the policy request if either of these mappings is missing or incomplete.

Please refer to the [User Guide](https://mosip.atlassian.net/wiki/spaces/PMS/pages/2582773932/New+-+Credential+Partner+Onboarding+End+User+Guide#Step-3%3A-Partner-requests-Policy) for detailed instructions and configuration steps.

**Note**:As part of the current release, editing an existing policy request is not supported. Once a policy request has been created and approved, the associated **Biometric Extractor** and **Credential Type** mappings cannot be modified for that policy.

If changes to these mappings are required, a new policy request must be created with the updated configuration.

**Note**: For a given partner, mapping the same **Credential Type** multiple times is not supported. Each Credential Type can be associated only once per partner.

### 2. Support for Manual Adjudication Partner Onboarding

This release introduces support for a new partner type called **Manual Adjudication Partner** with [full onboarding support](https://mosip.atlassian.net/wiki/spaces/PMS/pages/2582839443).

With this partner admin can:

* Create new Manual Adjudication partner
* Upload and re-upload partner certificates
* Link relevant policies to the Manual Adjudication partner

### 3. Enhancement to Policy-to-Partner Validation in DataShare Service

The behavior of the following endpoint has been corrected in the DataShare service:

/v1/policymanager/policies/{policyId}/partner/{partnerId}

#### Enhancement Details

The endpoint validation logic has been updated to validate policy-to-partner mappings using the partner\_policy\_request table instead of the partner\_policy table.

## Deprecated APIs

<table data-header-hidden><thead><tr><th width="141.203125"></th><th width="106.44921875"></th><th width="316.91015625"></th><th></th></tr></thead><tbody><tr><td>API Endpoint</td><td>Method</td><td>Deprecation Description</td><td>Replacement Endpoint</td></tr><tr><td>/{partnerId}/credentialtype/{credentialType}/policies/{policyName}</td><td>POST</td><td><ul><li>It <strong>directly creates the partner–policy–credentialType mapping immediately</strong>, which bypasses the partner-policy request workflow and makes it difficult to support controlled <strong>approval/reject</strong> handling and consistent validations tied to a policy-request lifecycle. We introduced POST /partners/{partnerId}/policies/{policyId}/credential-types-request to align credential-type association with the <strong>existing partner policy mapping request flow</strong>: it validates the parent request (must exist and be <strong>IN_PROGRESS</strong>), enforces <strong>single-request-per-policy-request</strong>, performs duplicate checks against existing mappings, and persists the request in pms.partner_policy_credential_type_request so that the mapping can be <strong>approved or rejected</strong> reliably as part of the overall workflow.</li></ul></td><td><ul><li>POST<br>/partners/{partnerId}/policies/{policyId}/credential-types-request</li></ul></td></tr><tr><td>/{partnerId}/bioextractors/{policyId}</td><td>POST</td><td>It <strong>directly creates/updates the final extractor configuration</strong> for a partner-policy (writes into the extractor provider table) and therefore cannot reliably support the partner-policy mapping workflow’s <strong>approval/reject</strong> lifecycle. We introduced POST /partners/{partnerId}/policies/{policyId}/bio-extractors-request to align bio-extractor submissions with the <strong>existing partner policy request flow</strong>: it validates that the referenced partner-policy request exists and is <strong>IN_PROGRESS</strong>, prevents duplicate/overlapping extractor configurations (both within the request and against existing configuration), and persists the submission as request rows so the system can <strong>approve or reject</strong> them consistently and audibly.</td><td>POST<br>/partners/{partnerId}/policies/{policyId}/bio-extractors-request</td></tr></tbody></table>

## User Stories

| **Feature**                   | Jira Issue ID                                                 | Summary                                                                                                      |
| ----------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| Credential Partner Onboarding | [MOSIP-44515](https://mosip.atlassian.net/browse/MOSIP-44515) | Credential Partner – Login & Dashboard Display with Service Cards                                            |
|                               | [MOSIP-44519](https://mosip.atlassian.net/browse/MOSIP-44519) | Credential Partner – View Policy Request Details                                                             |
|                               | [MOSIP-44518](https://mosip.atlassian.net/browse/MOSIP-44518) | Credential Partner – View Submitted Policies (Tabular View)                                                  |
|                               | [MOSIP-44662](https://mosip.atlassian.net/browse/MOSIP-44662) | Partner Admin: Approve/Reject Partner Policy Request for Credential Partner                                  |
|                               | [MOSIP-44660](https://mosip.atlassian.net/browse/MOSIP-44660) | Credential Partner: Map Credential Type                                                                      |
|                               | [MOSIP-44659](https://mosip.atlassian.net/browse/MOSIP-44659) | Credential Partner: Map Biometric Extractor Provider while requesting policy                                 |
|                               | [MOSIP-44656](https://mosip.atlassian.net/browse/MOSIP-44656) | Partner Admin : Biometric Provider Configuration Dashboard Card in PMS                                       |
|                               | [MOSIP-44597](https://mosip.atlassian.net/browse/MOSIP-44597) | Partner Admin: View Biometric Extractor Provider Configuration Details                                       |
|                               | [MOSIP-44596](https://mosip.atlassian.net/browse/MOSIP-44596) | Partner Admin : Display Biometric Extractor Provider Configuration Listing for Credential partner            |
|                               | [MOSIP-44595](https://mosip.atlassian.net/browse/MOSIP-44595) | Partner Admin: Create Biometric Extractor Provider Configuration                                             |
|                               | [MOSIP-44517](https://mosip.atlassian.net/browse/MOSIP-44517) | Credential Partner – Request Policy                                                                          |
|                               | [MOSIP-44836](https://mosip.atlassian.net/browse/MOSIP-44836) | API Service: Update endpoint PUT /partners/policy/{mappingkey} for Approve / Reject a Partner Policy Request |
|                               | [MOSIP-44835](https://mosip.atlassian.net/browse/MOSIP-44835) | API Service: Add new endpoint GET /partner-policy-requests/{requestId}/credential-types-request              |
|                               | [MOSIP-44834](https://mosip.atlassian.net/browse/MOSIP-44834) | API Service: Add new endpoint GET /partner-policy-requests/{requestId}/bio-extractors-request                |
|                               | [MOSIP-44832](https://mosip.atlassian.net/browse/MOSIP-44832) | API Service: Add new endpoint POST /partners/{partnerId}/policies/{policyId}/credential-types-request        |
|                               | [MOSIP-44827](https://mosip.atlassian.net/browse/MOSIP-44827) | API Service: Add new endpoint POST /partners/{partnerId}/policies/{policyId}/bio-extractors-request          |
|                               | [MOSIP-44792](https://mosip.atlassian.net/browse/MOSIP-44792) | API Service: Correct Policy-Partner Mapping Validation in DataShare Service by Updating Endpoint Logic       |
|                               | [MOSIP-44676](https://mosip.atlassian.net/browse/MOSIP-44676) | API Service: Add new endpoint GET /bio-extractor-configurations/{bio-extractor-configurations-id}            |
|                               | [MOSIP-44674](https://mosip.atlassian.net/browse/MOSIP-44674) | API Service: Add new endpoint GET /bio-extractor-configurations                                              |
|                               | [MOSIP-44673](https://mosip.atlassian.net/browse/MOSIP-44673) | API Service: Add new endpoint POST /bio-extractor-configurations                                             |
| Manual Adjudication Partner   | [MOSIP-44348](https://mosip.atlassian.net/browse/MOSIP-44348) | Create Manual Adjudication Partner                                                                           |
|                               | [MOSIP-44350](https://mosip.atlassian.net/browse/MOSIP-44350) | Policy Linking Management for Manual Adjudication Partner                                                    |
|                               | [MOSIP-44410](https://mosip.atlassian.net/browse/MOSIP-44410) | Partner Admin: View Manual Adjudication Partner Details                                                      |
|                               | [MOSIP-44411](https://mosip.atlassian.net/browse/MOSIP-44411) | Partner Admin: Deactivate Manual Adjudication Partner                                                        |
|                               | [MOSIP-44349](https://mosip.atlassian.net/browse/MOSIP-44349) | Upload / Re-Upload Certificate for Manual Adjudication Partner                                               |
|                               | [MOSIP-44443](https://mosip.atlassian.net/browse/MOSIP-44443) | API: Update GET /partners/v3 endpoint to support Manual Adjudication partner type                            |
|                               | [MOSIP-44438](https://mosip.atlassian.net/browse/MOSIP-44438) | API: Update GET /partner-api-keys/v2 endpoint to include partnerType in request filter and response          |
|                               | [MOSIP-44793](https://mosip.atlassian.net/browse/MOSIP-44793) | Manual Adjudication Partner: Remove API Key related UI Features and Manual Adjudication Services Card        |
| Enchancements                 | [MOSIP-42447](https://mosip.atlassian.net/browse/MOSIP-42447) | Data length validation for all textboxes in Filter section                                                   |
|                               | [MOSIP-38412](https://mosip.atlassian.net/browse/MOSIP-38412) | Side panel should cover the entire screen height \<in progress>                                              |
| Automation                    | [MOSIP-44921](https://mosip.atlassian.net/browse/MOSIP-44921) | PMS UI Automation: push the code from develop to release 1.3.x branch for PMS 1.3.0-beta.5 release.          |

## Known Issues

Please refer here for the full list of known issues.

| Issue ID | Summary |
| -------- | ------- |
|          |         |
|          |         |
|          |         |
|          |         |

## Repositories Released

| Repository                        | Branch        | Release Tag   |
| --------------------------------- | ------------- | ------------- |
| mosip-partner-management-services | release-1.3.x | v1.3.0-beta.5 |
| mosip-partner-management-ui       | release-1.3.x | v1.3.0-beta.5 |

## Compatible Modules

| **Module/ Repo**    | **Tags**      |
| ------------------- | ------------- |
| Key Manager         | v1.3.0-beta.3 |
| mosip-openid-bridge | v1.3.0-beta.2 |
| artifactory         | v1.2.0.2      |
| IDA                 | v1.2.1.0      |
| eSignet             | v1.6.2        |
| Reg Proc            | v1.2.0.2      |
| Notifier (Kernel)   | v1.2.0.1      |
| Audit manager       | v1.2.0.1      |
| ID Repo             | v1.2.2.0      |
| datashare           | v1.2.0.1      |
| Keycloak            | v1.2.0.1      |
| config-server       | v1.1.2        |
| Websub              | v1.2.0.1      |

## Learn More

* [Services](https://github.com/mosip/partner-management-services/tree/release-1.3.x)
* [Partner Management Portal](https://github.com/mosip/partner-management-portal/tree/release-1.3.x): For code and implementation of Partner Management Portal (revamp),
* [Features](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/overview/features)
* [End User Guide](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding)
* [Technical Guide](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/develop)

#### Test Report

For details on the test results, refer here.

