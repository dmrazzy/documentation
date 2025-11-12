# Partner Management System Revamp 1.3.0-beta.3

**Release Name**: Partner Management System Revamp

**Release Number**: 1.3.0-beta.3

**Release Date**: 7th November, 2025

#### **Overview**

We’re excited to announce the release of **PMS 1.3.0.beta3**, This brings **end-to-end support for** [**MISP Partners**](../../../id-lifecycle-management/support-systems/partner-management-services/overview/) within the PMS (Partner Management System). This release empowers Partner Admins to manage the complete MISP Partner lifecycle — from onboarding and certificate handling to policy association and license key management — all through a seamless and unified interface.

#### **Key Highlights**

PMS now supports complete lifecycle management of **MISP Partners**, including their onboarding, certificate management, policy association, and MISP License Key management.

1. **MISP Partner Onboarding**: Partner Admins can [**Onboard a MISP Partner**](../../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding.md#onboarding-misp-partners) from the dashboard by first creating a partner (after uploading Root and Intermediate CA certificates) and then uploading the CA-signed certificate & linking a Policy Group.

* You can [**Create new MISP Partners**](../../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding.md#create-a-misp-partner) directly using the **Partners** card on PMS dashboard.
* During creation, the 'Partner Admin' must ensure that the **Root and Intermediate CA Certificates** are already uploaded.
* Once created, a 'MISP Partner' can upload or re-upload their **CA-signed partner certificate** and link a **Policy Group** (one-time action).

2. **Policy Linking for MISP Partners**: You can [**Manage MISP policy linkages**](../../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding.md#select-policy-group-one-time-assignment) (request, approve, reject) and view all linked policies in a single table for streamlined tracking.

* Once a Policy Group is assigned, you can **request, approve, or reject** MISP policy linkages.
* The interface provides a **tabular view** showing all partners and their linked policy details for easy management and tracking.

3. **MISP Services and License Key Management**: You can now [generate, regenerate, view, and deactivate MISP License Keys](../../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding.md#generate-misp-license-key) directly in PMS, enabling full lifecycle management from a dedicated MISP Services card.

* A dedicated **MISP Services** card enables comprehensive management of 'MISP License Keys.
* Partner Admins can:
  * Generate a new MISP License Key (by specifying the validity period).
  * Regenerate a license key with updated validity.
  * View existing license keys and their details.
  * Deactivate active license keys.
* This new capability allows Partner Admins to **manage the complete MISP Partner lifecycle** directly within PMS.

{% hint style="success" %}
**Note:**

* It is now possible to generate multiple MISP License Keys for the same Partner ID; however, only the latest key remains active in the **ID Authentication (IDA)** module, and older keys are automatically overwritten.
* The MISP license key **automatically deactivates when it expires**.
{% endhint %}

For a comprehensive and detailed description of all the features, refer to [Features Documentation](../../../id-lifecycle-management/support-systems/partner-management-services/overview/features.md).

#### **User Stories**

List of all user stories pertaining to this release are available [here](https://mosip.atlassian.net/issues/?filter=11933\&jql=project%20%3D%20MOSIP%20AND%20%22Epic%20Link%22%20%3D%20MOSIP-32075%20and%20status%20not%20in%20%28Canceled%29%20and%20type%3D%20story%20and%20%22Release%20Number%5BLabels%5D%22%20in%20%28PMS_Release_1.3.0-beta.2%29%20ORDER%20BY%20status%20ASC).

<table><thead><tr><th width="175.15234375">Feature</th><th width="182.52734375" align="center">JIRA ID</th><th>Sub Feature</th></tr></thead><tbody><tr><td>Create and Manage MISP Partner</td><td align="center"></td><td></td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-33105">MOSIP-33105</a></td><td>Create MISP Partner</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42971">MOSIP-42971</a></td><td>MISP Partner: API POST /partners/{partnerId}/policy-group</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42963">MOSIP-42963</a></td><td>MISP Partner: API GET /misp-licenses/{partnerId}</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42962">MOSIP-42962</a></td><td>MISP Partner: API PATCH /misp-licenses/{partnerId}</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42961">MOSIP-42961</a></td><td>MISP Partner: API PUT /misp-licenses/{partnerId}</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42960">MOSIP-42960</a></td><td>MISP Partner: API POST /misp-licenses()</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42932">MOSIP-42932</a></td><td>MISP Partner onboarding issues related to keycloak</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42922">MOSIP-42922</a></td><td>MISP Partner: API GET /misp-licenses()</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42882">MOSIP-42882</a></td><td>MISP Partner: API GET /partners/v3?status={status}&#x26;policyGroupAvailable={policyGroupAvailable}&#x26;partnerType=MISP_Partner</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42880">MOSIP-42880</a></td><td>MISP Partner: API POST /partners/v3()</td></tr><tr><td>Generate MISP License Key</td><td align="center"></td><td></td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42860">MOSIP-42860</a></td><td>MISP Services: Regenerate MISP License Key</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42478">MOSIP-42478</a></td><td>MISP Services: Generate MISP License Key</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42479">MOSIP-42479</a></td><td>MISP Services: Tabular View of MISP License Keys</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43205">MOSIP-43205</a></td><td>Display of Important Note on Multiple License Keys</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43166">MOSIP-43166</a></td><td>MISP Partner: API POST /partners/exists()</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42487">MOSIP-42487</a></td><td>MISP Services: Deactivate MISP License Key</td></tr><tr><td>MISP Partner Policy Management</td><td align="center"></td><td></td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42861">MOSIP-42861</a></td><td>Select Policy Group for MISP Partner</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42486">MOSIP-42486</a></td><td>MISP Services: Individual View of MISP License key details</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42463">MOSIP-42463</a></td><td>Policy Manager: Create and Manage MISP Policies</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42461">MOSIP-42461</a></td><td>Request and Manage MISP Partner Policy Linking</td></tr><tr><td></td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42460">MOSIP-42460</a></td><td>Partner Admin: MISP Partner Onboarding in PMS portal</td></tr><tr><td>Legacy PMS Enhancement</td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-41988">MOSIP-41988</a></td><td>PMS Legacy: Database issues</td></tr><tr><td>List View MISP Partner</td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-41480">MOSIP-41480</a></td><td>Additional features for MISP Partner Type in 'List of Partners' (Tabular view)</td></tr><tr><td>UI Label Update</td><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-42462">MOSIP-42462</a></td><td>Update the Status label from 'Activated' to 'Active' across all PMS UI pages for all partners, partner admins, policy managers.</td></tr></tbody></table>

#### Known Issues

For more details on all the the open issues, please refer [here](https://mosip.atlassian.net/issues?filter=11933\&jql=project%20%3D%20MOSIP%20AND%20%22Epic%20Link%22%20%3D%20MOSIP-32075%20and%20status%20not%20in%20\(Canceled\)%20and%20type%3D%20story%20and%20%22Release%20Number%5BLabels%5D%22%20in%20\(PMS_Release_1.3.0-beta.3\)%20ORDER%20BY%20status%20ASC).

<table><thead><tr><th width="209.52734375" align="center">JIRA ID</th><th>Issue Description</th></tr></thead><tbody><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43457">MOSIP-43457</a></td><td>MISP License regeneration fails when Policy ID is empty, even though license generation with empty Policy ID succeeds.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43405">MOSIP-43405</a></td><td>PMS-Revamp - Tablet: All info tooltip texts overflow outside the all screen.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43516">MOSIP-43516</a></td><td>PMS-Revamp - Tablet: “Upload MISP Partner Certificate” button is misaligned on the Partner Created Successfully screen.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43389">MOSIP-43389</a></td><td>PMS Revamp- MISP licence key name is not bold in the regenerate MISP licence key screen.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43465">MOSIP-43465</a></td><td>PMS Revamp- Error message is not as per the story criteria on entering same key name.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43486">MOSIP-43486</a></td><td>PMS-Revamp -The Title of "‘View MISP License Key" is not as per the story mentioned. "Details" word is missing.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43487">MOSIP-43487</a></td><td>PMS-Revamp - “Pending for approval” status label overlaps with the "linked devices" icon in French language on MacBook (Safari browser).</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43490">MOSIP-43490</a></td><td>PMS-Revamp - Highlighting square border around sort icon overlaps with adjacent icons when sorting is applied in the MacBook.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43448">MOSIP-43448</a></td><td>PMS-Revamp- Eye icon and "Deactivated" status are attached with no spacing in the list of MISP License Key tabular view on Safari MacBook.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43491">MOSIP-43491</a></td><td>PMS-Revamp - Screen layout and border alignment issue in "View Partner Details" when enter a long email ID, across all three languages.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43493">MOSIP-43493</a></td><td>PMS-Revamp - Action button (...) appears at the top when a long Policy Group is present in the List of Partners tabular view.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43326">MOSIP-43326</a></td><td>PMS Revamp - Success message alignment is not even for icon and text on confirmation popup in all 3 languages.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43334">MOSIP-43334</a></td><td>PMS Revamp- The asterisk symbol (*) used to indicate mandatory fields is not uniformly aligned in Create MISP Licence Key in all 3 languages.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43483">MOSIP-43483</a></td><td>PMS-Revamp - No spacing between page number and Next ( > ) button in pagination across all screens in Arabic language.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43515">MOSIP-43515</a></td><td>PMS - Revamp - Tablet: Row separator lines missing for some rows across all tables.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43391">MOSIP-43391</a></td><td>PMS Revamp- On entering existing username and email id , getting an error message for email id only.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43514">MOSIP-43514</a></td><td>PMS-Revamp - Tablet: Alert badge is not displayed correctly on the Dashboard screen in Arabic language.</td></tr><tr><td align="center"><a href="https://mosip.atlassian.net/browse/MOSIP-43151">MOSIP-43151</a></td><td>MISP License Key (IDA Issue): Inconsistent Primary Key Handling Between PMS and IDA for MISP License Keys.</td></tr></tbody></table>

#### **Repositories Released**

| Repository Released         | Branch name   | Tags          |
| --------------------------- | ------------- | ------------- |
| partner-management-services | release-1.3.x | v1.3.0-beta.3 |
| partner-management-portal   | release-1.3.x | v1.3.0-beta.3 |
| mosip-data                  | release-1.3.x | v1.3.0-beta.3 |

#### Compatible Modules

The following table outlines the tested and certified compatibility of PMS 1.3.0-beta.3 with other modules.

| Module/ Repo        | Tags                                                                             |
| ------------------- | -------------------------------------------------------------------------------- |
| Key Manager         | [v1.3.0-beta.3](https://github.com/mosip/keymanager/tree/v1.3.0-beta.3)          |
| mosip-openid-bridge | [v1.3.0-beta.2](https://github.com/mosip/mosip-openid-bridge/tree/v1.3.0-beta.2) |
| artifactory         | [v1.2.0.2](https://github.com/mosip/artifactory-ref-impl/tree/v1.2.0.2)          |
| IDA                 | [v1.2.1.0](https://github.com/mosip/id-authentication/tree/v1.2.1.0)             |
| eSignet             | [v1.4.1](https://github.com/mosip/esignet/tree/v1.4.1)                           |
| Reg Proc            | [v1.2.0.2](https://github.com/mosip/registration/tree/v1.2.0.2)                  |
| Notifier (Kernel)   | [v1.2.0.1](https://github.com/mosip/commons/tree/v1.2.0.1/kernel)                |
| Audit manager       | [v1.2.0.1](https://github.com/mosip/audit-manager/tree/v1.2.0.1)                 |
| ID Repo             | [v1.2.2.0](https://github.com/mosip/id-repository/tree/v1.2.2.0)                 |
| datashare           | [v1.2.0.1](https://github.com/mosip/durian/tree/v1.2.0.1)                        |
| Keycloak            | [v1.2.0.1](https://github.com/mosip/keycloak/tree/v1.2.0.1)                      |
| config-server       | [v1.1.2](https://github.com/mosip/mosip-config/tree/v1.1.2)                      |
| Websub              | [v1.2.0.1](https://github.com/mosip/websub/tree/v1.2.0.1)                        |

### Learn More

* [Services](https://github.com/mosip/partner-management-services/tree/release-1.3.x)
* [Partner Management Portal](https://github.com/mosip/partner-management-portal/tree/release-1.3.x): For code and implementation of Partner Management Portal (revamp),
* [Features](../../../id-lifecycle-management/support-systems/partner-management-services/overview/features.md)
* [End User Guide](../../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding.md)
* [Technical Guide](../../../id-lifecycle-management/support-systems/partner-management-services/develop/)

### Test Report

For details on the test results, refer [here](./).
