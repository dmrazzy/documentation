# Partner Management System Revamp 1.3.0-beta.4

**Release Name**: Partner Management System Revamp

**Release Number**: 1.3.0-beta.4

**Release Date**: Coming Soon!

### Overview

PMS Revamp v1.3.0-Beta4 introduces key enhancements focused on partner onboarding expansion, role registration simplification, security configuration flexibility, and proactive notifications. This release adds support for a new ABIS partner type, streamlines Partner Admin and Policy Manager onboarding, enhances OIDC client configuration, improves API key lifecycle management, and introduces MISP license expiry notifications for better operational visibility.

### Key Features & Enhancements

#### 1. ABIS Partner Onboarding Support

PMS now supports onboarding of a new partner type — **ABIS (Automated Biometric Identification System) Partner** — through the Partner Admin role.

With this enhancement, a **Partner Admin** can:

* Create new ABIS partner entries
* Upload and re-upload partner certificates
* Link relevant policies to the ABIS partner

**Note:**\
Before onboarding **any partner** (including ABIS partners), the **upload of Root CA and Intermediate CA certificates is a mandatory prerequisite**. Partner onboarding cannot proceed unless this trust chain is already configured in PMS.

**Important Note for ABIS Partners:**\
While linking policies for an ABIS partner, **only Data Share policies are applicable and should be selected**.

#### 2. Updated Partner Admin & Policy Manager Registration Flow

The registration flow for **Partner Admin** and **Policy Manager** roles has been simplified.

From **PMS v1.3.0-Beta4 onwards**, Partner Admins and Policy Managers:

* **Are no longer required to self-register via the PMS UI**
* Can be **directly created in Keycloak**
* Can be assigned the **Partner Admin** or **Policy Manager** role in Keycloak
* Can use the **same Keycloak credentials** to log in directly to the PMS portal

This change reduces onboarding steps, eliminates redundant self-registration, and centralizes role management within Keycloak.

**Info Note:**\
This updated registration flow is applicable to **PMS v1.3.0-Beta4 and later versions only**.

#### 3. OIDC Client Enhancement – Additional Configuration Fields

The OIDC client creation flow has been enhanced with a **new “Additional Information” section** in the UI.

This section allows Relying Parties (RPs) to:

* Configure additional parameters
* Customize OIDC client behavior with greater flexibility
* Support advanced integration and use-case specific requirements

**Compatibility Note:**\
The **Additional Information** section is **editable only when PMS is integrated with eSignet version 1.6.2 or above**.\
For PMS deployments using **eSignet versions lower than 1.6.2**, this section will be visible but **not editable**.

#### 4. API Key Expiry Date Management by Partner Admin

An enhancement has been introduced to allow **editing the expiry date of API keys** by partner admin. With this change:

* Partner can continue using the **same API key** for an extended period
* The expiry date can be **updated as it approaches**, without regenerating a new key
* This reduces operational overhead and improves key lifecycle management

#### 5. MISP License Key Expiry Notifications

PMS now provides **proactive notifications for expiring MISP license keys**, targeted at the **Partner Admin** role.

Notifications are delivered through:

* **Email alerts** to the Partner Admin
* **Notifications** in the PMS notification panel
* **Dashboard alerts** within the PMS portal

This enhancement ensures Partner Admins are informed well in advance and can take timely action to renew licenses.

#### 6. Removal of Partner Existence Validation for Partner Admins:

Validation checks to verify whether a partner already exists in the PMS database have been removed for Partner Admin operations, simplifying workflows and improving onboarding efficiency.

### Deprecated APIs

| API Endpoint                                          | Method | Deprecation                                                                       | Replacement                                                                     |
| ----------------------------------------------------- | ------ | --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| /oauth/client                                         | GET    | This endpoint retrieves a list of all OAuth clients created by the Auth Partners. | Link to “GET /oidc-clients.“                                                    |
| /oauth/client                                         | POST   | This endpoint is used for creating OIDC Client.                                   | Link to “ POST /oidc-clients.“                                                  |
| /oauth/client/{client\_id}                            | GET    | This endpoint retrieves the OIDC client details by client id                      | Link to “GET /oidc-clients/{clientId}.“                                         |
| /oauth/client/{client\_id}                            | PUT    | This endpoint is used for updating OIDC Client based on client id                 | Link to “PUT /oidc-clients/{clientId}.“                                         |
| /partners/{partnerId}/policy/{policyId}/apikey/status | PATCH  | Service to activate/de-activate partner API key                                   | Link to “PATCH /partners/{partnerId}/policies/{policyId}/api-keys/{apiKeyName}“ |

### User Stories

| **Feature**                                         | **Issue ID**                                                  | **Summary**                                                                                                                |
| --------------------------------------------------- | ------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| OIDC Client Enchancement                            |                                                               |                                                                                                                            |
|                                                     | [MOSIP-43551](https://mosip.atlassian.net/browse/MOSIP-43551) | OIDC Client Enhancement: API POST /oidc-clients                                                                            |
|                                                     | [MOSIP-43552](https://mosip.atlassian.net/browse/MOSIP-43552) | OIDC Client Enhancement: API PUT /oidc-clients/{clientId}                                                                  |
|                                                     | [MOSIP-43565](https://mosip.atlassian.net/browse/MOSIP-43565) | Edit OIDC Client Enhancements - Incorporate all changes in UI page                                                         |
|                                                     | [MOSIP-43571](https://mosip.atlassian.net/browse/MOSIP-43571) | OIDC Client Enhancement: API PATCH /oidc-clients/{clientId}                                                                |
|                                                     | [MOSIP-43553](https://mosip.atlassian.net/browse/MOSIP-43553) | OIDC Client Enhancement: API GET /oidc-clients/{clientId}                                                                  |
|                                                     | [MOSIP-43578](https://mosip.atlassian.net/browse/MOSIP-43578) | OIDC Client Enhancement: API GET /oidc-clients                                                                             |
|                                                     | [MOSIP-43776](https://mosip.atlassian.net/browse/MOSIP-43776) | Partner Admin - View OIDC Client Enhancements - Incorporate all changes in UI page                                         |
|                                                     | [MOSIP-43699](https://mosip.atlassian.net/browse/MOSIP-43699) | View OIDC Client Enhancements - Incorporate all changes in UI page                                                         |
|                                                     | [MOSIP-38711](https://mosip.atlassian.net/browse/MOSIP-38711) | Create OIDC Client Partner Enhancement to Support Multilingual Fields and Additional Information                           |
|                                                     | [MOSIP-43657](https://mosip.atlassian.net/browse/MOSIP-43657) | Backward Compatibility for OIDC Client Creation UI Based on eSignet Version                                                |
| Change in Partner Admin/Policy Manager Registration |                                                               |                                                                                                                            |
|                                                     | [MOSIP-41030](https://mosip.atlassian.net/browse/MOSIP-41030) | Change in Partner Admin/Policy Manager registration userflow                                                               |
| Update the Expiry date of API key                   |                                                               |                                                                                                                            |
|                                                     | [MOSIP-38157](https://mosip.atlassian.net/browse/MOSIP-38157) | Expiry duration of existing Active API Key should be editable in PMS Partner Admin portal                                  |
|                                                     | [MOSIP-44155](https://mosip.atlassian.net/browse/MOSIP-44155) | Create new endpoint PATCH /partners/{partnerId}/policies/{policyId}/api-keys/{apiKeyName}                                  |
|                                                     | [MOSIP-43536](https://mosip.atlassian.net/browse/MOSIP-43536) | New API Endpoint: PATCH /partners/{partnerId}/policy/{policyId}/apiKey/expiry-date                                         |
| MISP License Key Notifications for Partner Admin    |                                                               |                                                                                                                            |
|                                                     | [MOSIP-42488](https://mosip.atlassian.net/browse/MOSIP-42488) | MISP License Key Notification: View all Notifications Page (Partner Admin)                                                 |
|                                                     | [MOSIP-42862](https://mosip.atlassian.net/browse/MOSIP-42862) | MISP License Key Notification: Dashboard badge (Partner Admin)                                                             |
|                                                     | [MOSIP-42863](https://mosip.atlassian.net/browse/MOSIP-42863) | MISP License Key Notification in Notification Panel (Partner Admin)                                                        |
|                                                     | [MOSIP-43187](https://mosip.atlassian.net/browse/MOSIP-43187) | MISP License Key Notification: via email (Partner Admin)                                                                   |
|                                                     | [MOSIP-43462](https://mosip.atlassian.net/browse/MOSIP-43462) | Notifications: API Update GET /notifications API to support MISP Notifications                                             |
|                                                     | [MOSIP-43488](https://mosip.atlassian.net/browse/MOSIP-43488) | Notifications: API Add expiryPeriod optional query param in GET /misp-license API                                          |
| Save & Publish Policy                               |                                                               |                                                                                                                            |
|                                                     | [MOSIP-43256](https://mosip.atlassian.net/browse/MOSIP-43256) | Policies (Partner Admin): Save & Publish Policy during Policy creation                                                     |
| Onboard ABIS Partner                                |                                                               |                                                                                                                            |
|                                                     | [MOSIP-38721](https://mosip.atlassian.net/browse/MOSIP-38721) | Partner Admin: ABIS Partner Onboarding in PMS portal                                                                       |
|                                                     | [MOSIP-43607](https://mosip.atlassian.net/browse/MOSIP-43607) | Create ABIS Partner                                                                                                        |
|                                                     | [MOSIP-43610](https://mosip.atlassian.net/browse/MOSIP-43610) | Request and Manage ABIS Partner Policy Linking                                                                             |
|                                                     | [MOSIP-43609](https://mosip.atlassian.net/browse/MOSIP-43609) | Upload/ Re-Upload ABIS Partner Certificate                                                                                 |
|                                                     | [MOSIP-43826](https://mosip.atlassian.net/browse/MOSIP-43826) | ABIS Partner: API GET /partners/v3?status={status}\&policyGroupAvailable={policyGroupAvailable}\&partnerType=ABIS\_Partner |

### Known Issues

| Issue key                                                     | Summary                                                                                                                                                    |
| ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [MOSIP-44147](https://mosip.atlassian.net/browse/MOSIP-44147) | When a Keycloak user who does not have any valid PMS role logs in to the PMS Portal, the page continuously refreshes and results in a 403 Forbidden error. |
| [MOSIP-44017](https://mosip.atlassian.net/browse/MOSIP-44017) | PMS - Revamp - MacBook : Keyboard functionality not working for some fields in Additional Information section on Create OIDC Client page in Safari.        |
| [MOSIP-43487](https://mosip.atlassian.net/browse/MOSIP-43487) | PMS-Revamp - “Pending for approval” status label overlaps with the "linked devices" icon in French language on MacBook (Safari browser).                   |
| [MOSIP-43996](https://mosip.atlassian.net/browse/MOSIP-43996) | PMS Revamp - Error Message popping up When Editing and adding Client Name With Allowed Max Characters                                                      |
| [MOSIP-43997](https://mosip.atlassian.net/browse/MOSIP-43997) | PMS-Revamp -“Forgot Password” and “SignUp” link is visible on the eSignet screen even when their respective toggles are OFF                                |
| [MOSIP-43927](https://mosip.atlassian.net/browse/MOSIP-43927) | PMS Revamp- Inappropriate error\[bad request] message when user enters a value greater than 10 minutes in "consent\_expire\_in\_mins" field                |

### Repositories Released

| **Repository**              | **Branch Name** | **Version**   |
| --------------------------- | --------------- | ------------- |
| partner-management-services | release-1.3.x   | v1.3.0-beta.4 |
| partner-management-portal   | release-1.3.x   | v1.3.0-beta.4 |
| mosip-data                  | release-1.3.x   | v1.3.0-beta.4 |

### Compatible Modules

| **Module/ Repo**    | **Tags**      |
| ------------------- | ------------- |
| Key Manager         | v1.3.0-beta.3 |
| mosip-openid-bridge | v1.3.0-beta.2 |
| artifactory         | v1.2.0.2      |
| IDA                 | v1.2.1.0      |
| eSignet             | v1.4.1        |
| Reg Proc            | v1.2.0.2      |
| Notifier (Kernel)   | v1.2.0.1      |
| Audit manager       | v1.2.0.1      |
| ID Repo             | v1.2.2.0      |
| datashare           | v1.2.0.1      |
| Keycloak            | v1.2.0.1      |
| config-server       | v1.1.2        |
| Websub              | v1.2.0.1      |

#### Learn More

* [Services](https://github.com/mosip/partner-management-services/tree/release-1.3.x)
* [Partner Management Portal](https://github.com/mosip/partner-management-portal/tree/release-1.3.x): For code and implementation of Partner Management Portal (revamp),
* [Features](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/overview/features)
* [End User Guide](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding)
* [Technical Guide](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/develop)
