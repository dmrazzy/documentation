# Partner Management System Revamp 1.3.0-beta.3

Coming Soon

Note: Content below is under revision:

**Release Name**: Partner Management System Revamp

**Release Number**: 1.3.0-beta.3

**Release Date**: Coming Soon

#### **Overview**

We’re excited to announce the release of **PMS 1.3.0.beta3**, This brings **end-to-end support for** [**MISP Partners**](../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding) within the PMS (Partner Management System).
This release empowers Partner Admins to manage the complete MISP Partner lifecycle — from onboarding and certificate handling to policy association and license key management — all through a seamless and unified interface.

#### **Key Highlights**

PMS now supports complete lifecycle management of **MISP Partners**, including their onboarding, certificate management, policy association, and MISP License Key management.

**1.** [**MISP Partner Onboarding**](../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding#onboarding-misp-partners)

* Partner admins can create new MISP Partners directly using the **Partners** card on PMS dashboard.
* During creation, the 'Partner Admin' must ensure that the **Root and Intermediate CA Certificates** are already uploaded.
* Once created, a 'MISP Partner' can upload or re-upload their **CA-signed partner certificate** and link a **Policy Group** (one-time action).

**2.** [**Policy Linking for MISP Partners**](../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding#select-policy-group-one-time-assignment)

* Once a Policy Group is assigned, Partner Admins can **request, approve, or reject** MISP policy linkages.
* The interface provides a **tabular view** showing all partners and their linked policy details for easy management and tracking.

**3.** [**MISP Services and License Key Management**](../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding#generate-misp-license-key)

* A dedicated **MISP Services** card enables comprehensive management of 'MISP License Keys.
* Partner Admins can:
  * Generate a new MISP License Key (by specifying the validity period).
  * Regenerate a license key with updated validity.
  * View existing license keys and their details.
  * Deactivate active license keys.
* This new capability allows Partner Admins to **manage the complete MISP Partner lifecycle** directly within PMS.

**Note:**

* It is now possible to generate multiple MISP License Keys for the same Partner ID; however, only the latest key remains active in the **ID Authentication (IDA)** module, and older keys are automatically overwritten.
* The MISP license key **automatically deactivates when it expires**.

For a comprehensive and detailed description of all the features, refer to [Feature Documentation](../../id-lifecycle-management/support-systems/partner-management-services/overview/features.md).

#### **User Stories:**

| **Feature**                    | **Sub Feature**                                                                                                                  | **JIRA ID**                                                                                      |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Create and Manage MISP Partner | Create MISP Partner                                                                                                              | [https://mosip.atlassian.net/browse/MOSIP-33105](https://mosip.atlassian.net/browse/MOSIP-33105) |
|                                | MISP Partner: API POST /partners/{partnerId}/policy-group                                                                        | [https://mosip.atlassian.net/browse/MOSIP-42971](https://mosip.atlassian.net/browse/MOSIP-42971) |
|                                | MISP Partner: API GET /misp-licenses/{partnerId}                                                                                 | [https://mosip.atlassian.net/browse/MOSIP-42963](https://mosip.atlassian.net/browse/MOSIP-42963) |
|                                | MISP Partner: API PATCH /misp-licenses/{partnerId}                                                                               | [https://mosip.atlassian.net/browse/MOSIP-42962](https://mosip.atlassian.net/browse/MOSIP-42962) |
|                                | MISP Partner: API PUT /misp-licenses/{partnerId}                                                                                 | [https://mosip.atlassian.net/browse/MOSIP-42961](https://mosip.atlassian.net/browse/MOSIP-42961) |
|                                | MISP Partner: API POST /misp-licenses()                                                                                          | [https://mosip.atlassian.net/browse/MOSIP-42960](https://mosip.atlassian.net/browse/MOSIP-42960) |
|                                | MISP Partner onboarding issues related to keycloak                                                                               | [https://mosip.atlassian.net/browse/MOSIP-42932](https://mosip.atlassian.net/browse/MOSIP-42932) |
|                                | MISP Partner: API GET /misp-licenses()                                                                                           | [https://mosip.atlassian.net/browse/MOSIP-42922](https://mosip.atlassian.net/browse/MOSIP-42922) |
|                                | MISP Partner: API GET /partners/v3?status={status}\&policyGroupAvailable={policyGroupAvailable}\&partnerType=MISP\_Partner       | [https://mosip.atlassian.net/browse/MOSIP-42882](https://mosip.atlassian.net/browse/MOSIP-42882) |
|                                | MISP Partner: API POST /partners/v3()                                                                                            | [https://mosip.atlassian.net/browse/MOSIP-42880](https://mosip.atlassian.net/browse/MOSIP-42880) |
| Generate MISP License Key      | MISP Services: Regenerate MISP License Key                                                                                       | [https://mosip.atlassian.net/browse/MOSIP-42860](https://mosip.atlassian.net/browse/MOSIP-42860) |
|                                | MISP Services: Generate MISP License Key                                                                                         | [https://mosip.atlassian.net/browse/MOSIP-42478](https://mosip.atlassian.net/browse/MOSIP-42478) |
|                                | MISP Services: Tabular View of MISP License Keys                                                                                 | [https://mosip.atlassian.net/browse/MOSIP-42479](https://mosip.atlassian.net/browse/MOSIP-42479) |
|                                | Display of Important Note on Multiple License Keys                                                                               | [https://mosip.atlassian.net/browse/MOSIP-43205](https://mosip.atlassian.net/browse/MOSIP-43205) |
|                                | MISP Partner: API POST /partners/exists()                                                                                        | [https://mosip.atlassian.net/browse/MOSIP-43166](https://mosip.atlassian.net/browse/MOSIP-43166) |
|                                | MISP Services: Deactivate MISP License Key                                                                                       | [https://mosip.atlassian.net/browse/MOSIP-42487](https://mosip.atlassian.net/browse/MOSIP-42487) |
| MISP Partner Policy Management | Select Policy Group for MISP Partner                                                                                             | [https://mosip.atlassian.net/browse/MOSIP-42861](https://mosip.atlassian.net/browse/MOSIP-42861) |
|                                | MISP Services: Individual View of MISP License key details                                                                       | [https://mosip.atlassian.net/browse/MOSIP-42486](https://mosip.atlassian.net/browse/MOSIP-42486) |
|                                | Policy Manager: Create and Manage MISP Policies                                                                                  | [https://mosip.atlassian.net/browse/MOSIP-42463](https://mosip.atlassian.net/browse/MOSIP-42463) |
|                                | Request and Manage MISP Partner Policy Linking                                                                                   | [https://mosip.atlassian.net/browse/MOSIP-42461](https://mosip.atlassian.net/browse/MOSIP-42461) |
|                                | Partner Admin: MISP Partner Onboarding in PMS portal                                                                             | [https://mosip.atlassian.net/browse/MOSIP-42460](https://mosip.atlassian.net/browse/MOSIP-42460) |
| Legacy PMS Enhancement         | PMS Legacy: Database issues                                                                                                      | [https://mosip.atlassian.net/browse/MOSIP-41988](https://mosip.atlassian.net/browse/MOSIP-41988) |
| List View MISP Partner         | Additional features for MISP Partner Type in 'List of Partners' (Tabular view)                                                   | [https://mosip.atlassian.net/browse/MOSIP-41480](https://mosip.atlassian.net/browse/MOSIP-41480) |
| UI Label Update                | Update the Status label from 'Activated' to 'Active' across all  PMS UI pages for all partners, partner admins, policy managers. | [https://mosip.atlassian.net/browse/MOSIP-42462](https://mosip.atlassian.net/browse/MOSIP-42462) |

List of all user stories pertaining to this release are available [here](https://mosip.atlassian.net/issues/?filter=11933\&jql=project%20%3D%20MOSIP%20AND%20%22Epic%20Link%22%20%3D%20MOSIP-32075%20and%20status%20not%20in%20%28Canceled%29%20and%20type%3D%20story%20and%20%22Release%20Number%5BLabels%5D%22%20in%20%28PMS_Release_1.3.0-beta.2%29%20ORDER%20BY%20status%20ASC).

#### Known Issues

&#x20;

| **JIRA ID**                                                   | **Issue Description**                                                                                                                         |
| ------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| [MOSIP-43457](https://mosip.atlassian.net/browse/MOSIP-43457) | MISP License regeneration fails when Policy ID is empty, even though license generation with empty Policy ID succeeds.                        |
| [MOSIP-43405](https://mosip.atlassian.net/browse/MOSIP-43405) | PMS-Revamp - Tablet: All info tooltip texts overflow outside the all screen.                                                                  |
| [MOSIP-43516](https://mosip.atlassian.net/browse/MOSIP-43516) | PMS-Revamp - Tablet: “Upload MISP Partner Certificate” button is misaligned on the Partner Created Successfully screen.                       |
| [MOSIP-43389](https://mosip.atlassian.net/browse/MOSIP-43389) | PMS Revamp- MISP licence key name is not bold in the regenerate MISP licence key screen                                                       |
| [MOSIP-43465](https://mosip.atlassian.net/browse/MOSIP-43465) | PMS Revamp- Error message is not as per the story criteria on entering same key name                                                          |
| [MOSIP-43486](https://mosip.atlassian.net/browse/MOSIP-43486) | PMS-Revamp -The Title of "‘View MISP License Key" is not as per the story mentioned. "Details" word is missing.                               |
| [MOSIP-43487](https://mosip.atlassian.net/browse/MOSIP-43487) | PMS-Revamp - “Pending for approval” status label overlaps with the "linked devices" icon in French language on MacBook (Safari browser).      |
| [MOSIP-43490](https://mosip.atlassian.net/browse/MOSIP-43490) | PMS-Revamp - Highlighting square border around sort icon overlaps with adjacent icons when sorting is applied in the Macbook.                 |
| [MOSIP-43448](https://mosip.atlassian.net/browse/MOSIP-43448) | PMS-Revamp- Eye icon and "Deactivated" status are attached with no spacing in the list of MISP License Key tabular view on Safari MacBook.    |
| [MOSIP-43491](https://mosip.atlassian.net/browse/MOSIP-43491) | PMS-Revamp - Screen layout and border alignment issue in "View Partner Details" when enter a long email ID, across all three languages.       |
| [MOSIP-43493](https://mosip.atlassian.net/browse/MOSIP-43493) | PMS-Revamp - Action button (...) appears at the top when a long Policy Group is present in the List of Partners tabular view.                 |
| [MOSIP-43326](https://mosip.atlassian.net/browse/MOSIP-43326) | PMS Revamp - Success message alignment is not even for icon and text on confirmation popup in all 3 languages                                 |
| [MOSIP-43334](https://mosip.atlassian.net/browse/MOSIP-43334) | PMS Revamp- The asterisk symbol (\*) used to indicate mandatory fields is not uniformly aligned in Create MISP Licence Key in all 3 languages |
| [MOSIP-43483](https://mosip.atlassian.net/browse/MOSIP-43483) | PMS-Revamp - No spacing between page number and Next ( > ) button in pagination across all screens in Arabic language.                        |
| [MOSIP-43515](https://mosip.atlassian.net/browse/MOSIP-43515) | PMS - Revamp - Tablet: Row separator lines missing for some rows across all tables.                                                           |
| [MOSIP-43391](https://mosip.atlassian.net/browse/MOSIP-43391) | PMS Revamp- On entering existing username and email id , getting an error message for email id only                                           |
| [MOSIP-43514](https://mosip.atlassian.net/browse/MOSIP-43514) | PMS-Revamp - Tablet: Alert badge is not displayed correctly on the Dashboard screen in Arabic language.                                       |
| [MOSIP-43151](https://mosip.atlassian.net/browse/MOSIP-43151) | MISP License Key (IDA Issue): Inconsistent Primary Key Handling Between PMS and IDA for MISP License Keys                                     |

For more details on all the the open issues, please refer [here](https://mosip.atlassian.net/issues?filter=11933\&jql=project%20%3D%20MOSIP%20AND%20%22Epic%20Link%22%20%3D%20MOSIP-32075%20and%20status%20not%20in%20\(Canceled\)%20and%20type%3D%20story%20and%20%22Release%20Number%5BLabels%5D%22%20in%20\(PMS_Release_1.3.0-beta.3\)%20ORDER%20BY%20status%20ASC).

&#x20;

#### **Repositories Released**

| **Repository Released**     | **Branch name** | **Tags**      |
| --------------------------- | --------------- | ------------- |
| partner-management-services | release-1.3.x   | v1.3.0-beta.3 |
| partner-management-portal   | release-1.3.x   | v1.3.0-beta.3 |
| mosip-data                  | release-1.3.x   | v1.3.0-beta.3 |

#### Compatible Modules:

The following table outlines the tested and certified compatibility of PMS 1.3.0-beta.1 with other modules.

| **Module/ Repo**    | **Tags**                                                                         |
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

#### Services

For code and implementation of Partner Management Services, refer [here](https://github.com/mosip/partner-management-services/tree/release-1.3.x).

#### Partner Management Portal UI

For code and implementation of Partner Management Portal (revamp) , refer [here](https://github.com/mosip/partner-management-portal/tree/release-1.3.x).

To get started with the user interface of Partner Management Portal, refer to the [Partner Management Portal End User Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/functional-overview).

#### Documentation

* Features: [https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/overview/features](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/overview/features)
* End User Guide on Notifications feature: [https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/functional-overview/pms-notification](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/functional-overview/pms-notification)
* Technical Guide: [https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/develop](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/develop)

#### Test Report

For details on the test results, refer [here](https://github.com/mosip/test-management/tree/master/PMS%20Revamp/1.3.0-beta.2).

