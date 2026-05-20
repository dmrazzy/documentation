# Partner Management System Revamp 1.3.0-beta.5

**Release Name**: Partner Management System Revamp

**Release Number**: 1.3.0-beta.5

**Release Date**: 20th May, 2026&#x20;

### Overview

Partner Management System (PMS) Revamp 1.3.0-beta.5 introduces support for onboarding new partner types and includes enhancements to partner-policy validation behavior in the DataShare service. This release extends the PMS onboarding framework to support Credential Partners and Manual Adjudication Partners through complete end-to-end onboarding flows.

## Key Features & Enhancements

### 1. Support for Credential Partner Onboarding

This release introduces support for a new partner type called **Credential Partner** with [complete end-to-end onboarding](../../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/onboard-credential-partner.md) capabilities in PMS.

#### Added support for:

* Partner self-registration support for Credential Partners.
* Policy request creation support for Partner Admins.
* Root CA upload support as part of Credential Partner onboarding.
* Policy request approval workflow support for Partner Admins.
* Biometric Extractor configuration creation support for Partner Admins.
* End-to-end onboarding lifecycle support for Credential Partners across PMS workflows.

{% hint style="info" %}
**Note** : Credential Partner onboarding requires mandatory mapping of **Biometric Extractors** and **Credential Types** as part of the policy request workflow. To support this, two additional steps have been introduced in the policy request process for configuring these mappings.

The Partner Admin will not be able to approve the policy request if either of these mappings is missing or incomplete.
{% endhint %}

Please refer to the [User Guide](../../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/onboard-credential-partner.md) for detailed instructions and configuration steps.

{% hint style="info" %}
**Note**:As part of the current release, editing an existing policy request is not supported. Once a policy request has been created and approved, the associated **Biometric Extractor** and **Credential Type** mappings cannot be modified for that policy.

If changes to these mappings are required, a new policy request must be created with the updated configuration.
{% endhint %}

{% hint style="info" %}
**Note**: For a given partner, mapping the same **Credential Type** multiple times is not supported. Each Credential Type can be associated only once per partner.
{% endhint %}

### 2. Support for Manual Adjudication Partner Onboarding

This release introduces support for a new partner type called **Manual Adjudication Partner** with [full onboarding](../../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/onboard-manual-adjudication-partner.md) support.

With this partner admin can:

* Create new Manual Adjudication partner
* Upload and re-upload partner certificates
* Link relevant policies to the Manual Adjudication partner

### 3. Enhancement to Policy-to-Partner Validation in DataShare Service

The behavior of the following endpoint has been corrected in the DataShare service:

`/v1/policymanager/policies/{policyId}/partner/{partnerId}`

#### Enhancement Details

The endpoint validation logic has been updated to validate policy-to-partner mappings using the `partner_policy_request table` instead of the `partner_policy table`.

## Deprecated APIs

<table><thead><tr><th width="141.203125">API Endpoint</th><th width="106.44921875">Method</th><th width="316.91015625">Deprecation Description</th><th>Replacement Endpoint</th></tr></thead><tbody><tr><td><code>/{partnerId}/credentialtype/{credentialType}/policies/{policyName}</code></td><td><strong>POST</strong></td><td>Creates mappings directly and bypasses the partner-policy request approval workflow.</td><td><br><code>/partners/{partnerId}/policies/{policyId}/credential-types-request</code></td></tr><tr><td><code>/{partnerId}/bioextractors/{policyId}</code></td><td><strong>POST</strong></td><td>Direct biometric extractor configuration creation/update without workflow-based approval handling.</td><td><br><code>/partners/{partnerId}/policies/{policyId}/bio-extractors-request</code></td></tr></tbody></table>

## User Stories

<table><thead><tr><th width="168.32421875">Feature</th><th width="173.5625">Jira Issue ID</th><th>Summary</th></tr></thead><tbody><tr><td>Credential Partner Onboarding</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44515">MOSIP-44515</a></td><td>Credential Partner – Login &#x26; Dashboard Display with Service Cards</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44519">MOSIP-44519</a></td><td>Credential Partner – View Policy Request Details</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44518">MOSIP-44518</a></td><td>Credential Partner – View Submitted Policies (Tabular View)</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44662">MOSIP-44662</a></td><td>Partner Admin: Approve/Reject Partner Policy Request for Credential Partner</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44660">MOSIP-44660</a></td><td>Credential Partner: Map Credential Type</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44659">MOSIP-44659</a></td><td>Credential Partner: Map Biometric Extractor Provider while requesting policy</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44656">MOSIP-44656</a></td><td>Partner Admin : Biometric Provider Configuration Dashboard Card in PMS</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44597">MOSIP-44597</a></td><td>Partner Admin: View Biometric Extractor Provider Configuration Details</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44596">MOSIP-44596</a></td><td>Partner Admin : Display Biometric Extractor Provider Configuration Listing for Credential partner</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44595">MOSIP-44595</a></td><td>Partner Admin: Create Biometric Extractor Provider Configuration</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44517">MOSIP-44517</a></td><td>Credential Partner – Request Policy</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44836">MOSIP-44836</a></td><td>API Service: Update endpoint PUT /partners/policy/{mappingkey} for Approve / Reject a Partner Policy Request</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44835">MOSIP-44835</a></td><td>API Service: Add new endpoint GET /partner-policy-requests/{requestId}/credential-types-request</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44834">MOSIP-44834</a></td><td>API Service: Add new endpoint GET /partner-policy-requests/{requestId}/bio-extractors-request</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44832">MOSIP-44832</a></td><td>API Service: Add new endpoint POST /partners/{partnerId}/policies/{policyId}/credential-types-request</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44827">MOSIP-44827</a></td><td>API Service: Add new endpoint POST /partners/{partnerId}/policies/{policyId}/bio-extractors-request</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44792">MOSIP-44792</a></td><td>API Service: Correct Policy-Partner Mapping Validation in DataShare Service by Updating Endpoint Logic</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44676">MOSIP-44676</a></td><td>API Service: Add new endpoint GET /bio-extractor-configurations/{bio-extractor-configurations-id}</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44674">MOSIP-44674</a></td><td>API Service: Add new endpoint GET /bio-extractor-configurations</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44673">MOSIP-44673</a></td><td>API Service: Add new endpoint POST /bio-extractor-configurations</td></tr><tr><td>Manual Adjudication Partner</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44348">MOSIP-44348</a></td><td>Create Manual Adjudication Partner</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44350">MOSIP-44350</a></td><td>Policy Linking Management for Manual Adjudication Partner</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44410">MOSIP-44410</a></td><td>Partner Admin: View Manual Adjudication Partner Details</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44411">MOSIP-44411</a></td><td>Partner Admin: Deactivate Manual Adjudication Partner</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44349">MOSIP-44349</a></td><td>Upload / Re-Upload Certificate for Manual Adjudication Partner</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44443">MOSIP-44443</a></td><td>API: Update GET /partners/v3 endpoint to support Manual Adjudication partner type</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44438">MOSIP-44438</a></td><td>API: Update GET /partner-api-keys/v2 endpoint to include partnerType in request filter and response</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44793">MOSIP-44793</a></td><td>Manual Adjudication Partner: Remove API Key related UI Features and Manual Adjudication Services Card</td></tr><tr><td>Enchancements</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-42447">MOSIP-42447</a></td><td>Data length validation for all textboxes in Filter section</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38412">MOSIP-38412</a></td><td>Side panel should cover the entire screen height &#x3C;in progress></td></tr><tr><td>Automation</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44921">MOSIP-44921</a></td><td>PMS UI Automation: push the code from develop to release 1.3.x branch for PMS 1.3.0-beta.5 release.</td></tr></tbody></table>

## Known Issues

Please refer [here](https://mosip.atlassian.net/issues?jql=project%20%3D%20MOSIP%20AND%20issuetype%20%3D%20Bug%20AND%20%22epic%20link%22%20%3D%20MOSIP-32075%20AND%20status%20NOT%20IN%20\(Canceled%2C%20Closed\)%20AND%20affectedVersion%20%3D%20%221.3.0-Beta.5%22%20ORDER%20BY%20created%20DESC) for the full list of known issues.

<table data-first-column-sticky><thead><tr><th>Issue ID</th><th>Summary</th></tr></thead><tbody><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-45014">MOSIP-45014</a></td><td>PMS Revamp-Email notifications are not received in Arabic and French languages despite user language registration</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-45000">MOSIP-45000</a></td><td>PMS Revamp- The acknowledgement Title and subtitle popping up in the UI is not as per the requirement mentioned in the story criteria</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-44976">MOSIP-44976</a></td><td>PMS Revamp- Unable to create configuration with same name and different modality – uniqueness constraint unclear in Create Biometric Extractor Provider Configuration screen</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-44940">MOSIP-44940</a></td><td>PMS Revamp- On providing invalid attribute name/extractor provider/biometric/extractorproviderversion user is still able to submit the request for bio extract request</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-44815">MOSIP-44815</a></td><td>PMS-Revamp - Policy Group Selection Fails for Authentication partner with long Organization name and username.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-44783">MOSIP-44783</a></td><td>PMS-Revamp - Language code mismatch between Keycloak and DB During New Partner Registration for Notification Preference language.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-44680">MOSIP-44680</a></td><td>PMS Revamp- Partner ID/Username allows more than 36 characters during registration but fails at login with validation error</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-44091">MOSIP-44091</a></td><td>PMS Revamp UI Automation: Fix all commented-out issues and remove them from UI automation.</td></tr></tbody></table>

## Repositories Released

| Repository                        | Release Tag                                                                              |
| --------------------------------- | ---------------------------------------------------------------------------------------- |
| mosip-partner-management-services | [v1.3.0-beta.5](https://github.com/mosip/partner-management-services/tree/v1.3.0-beta.5) |
| mosip-partner-management-ui       | [v1.3.0-beta.5](https://github.com/mosip/partner-management-portal/tree/v1.3.0-beta.5)   |

## Compatible Modules

| Module/ Repo        | Tags                                                                             |
| ------------------- | -------------------------------------------------------------------------------- |
| Key Manager         | [v1.3.0-beta.3](https://github.com/mosip/keymanager/tree/v1.3.0-beta.3)          |
| mosip-openid-bridge | [v1.3.0-beta.2](https://github.com/mosip/mosip-openid-bridge/tree/v1.3.0-beta.2) |
| artifactory         | [v1.2.0.2](https://github.com/mosip/artifactory-ref-impl/tree/v1.2.0.2)          |
| IDA                 | [v1.2.1.0](https://github.com/mosip/id-authentication/tree/v1.2.1.0)             |
| eSignet             | [v1.6.2](https://github.com/mosip/esignet/tree/v1.6.2)                           |
| Reg Proc            | [v1.2.0.2](https://github.com/mosip/registration/tree/v1.2.0.2)                  |
| Notifier (Kernel)   | [v1.2.0.1](https://github.com/mosip/commons/tree/v1.2.0.1/kernel)                |
| Audit manager       | [v1.2.0.1](https://github.com/mosip/audit-manager/tree/v1.2.0.1)                 |
| ID Repo             | [v1.2.2.0](https://github.com/mosip/id-repository/tree/v1.2.2.0)                 |
| datashare           | [v1.2.0.1](https://github.com/mosip/durian/tree/v1.2.0.1)                        |
| Keycloak            | [v1.2.0.1](https://github.com/mosip/keycloak/tree/v1.2.0.1)                      |
| config-server       | [v1.1.2](https://github.com/mosip/mosip-config/tree/v1.1.2)                      |
| Websub              | [v1.2.0.1](https://github.com/mosip/websub/tree/v1.2.0.1)                        |

## Learn More

* [Services](https://github.com/mosip/partner-management-services/tree/v1.3.0-beta.5)
* [Partner Management Portal](https://github.com/mosip/partner-management-portal/tree/v1.3.0-beta.5): For code and implementation of Partner Management Portal (revamp),
* [Features](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/overview/features)
* [End User Guide](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/functional-overview)
* [Technical Guide](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/develop)

#### Test Report

For details on the test results, [refer here](test-report.md).

