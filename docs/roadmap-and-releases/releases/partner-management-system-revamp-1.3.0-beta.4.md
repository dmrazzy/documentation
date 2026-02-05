# Partner Management System Revamp 1.3.0-beta.4

**Release Name**: Partner Management System Revamp

**Release Number**: 1.3.0-beta.4

**Release Date**: 4th February, 2026

### Overview

PMS Revamp v1.3.0-Beta4 introduces key enhancements focused on partner onboarding expansion, role registration simplification, security configuration flexibility, and proactive notifications. This release adds support for a new ABIS partner type, streamlines Partner Admin and Policy Manager onboarding, enhances OIDC client configuration, improves API key lifecycle management, and introduces MISP license expiry notifications for better operational visibility.

### Key Features & Enhancements

#### 1. ABIS Partner Onboarding Support

PMS now supports onboarding of a new partner type — [**ABIS (Automated Biometric Identification System) Partner**](../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/abis-partner-onboarding-by-partner-admin.md#overview) — through the Partner Admin role.

With this enhancement, a **Partner Admin** can:

* Create new ABIS partner entries
* Upload and re-upload partner certificates
* Link relevant policies to the ABIS partner

{% hint style="info" %}
**Note:** Before onboarding **any partner** (including ABIS partners), the **upload of Root CA and Intermediate CA certificates is a mandatory prerequisite**. Partner onboarding cannot proceed unless this trust chain is already configured in PMS.
{% endhint %}

{% hint style="success" %}
**Important Note for ABIS Partners:** While linking policies for an ABIS partner, **only Data Share policies are applicable and should be selected**.
{% endhint %}

#### 2. Updated Partner Admin & Policy Manager Registration Flow

The registration flow for [**Partner Admin**](../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/partner-administration.md) and [**Policy Manager**](../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/policy-manager.md) roles has been simplified.

From **PMS v1.3.0-Beta4 onwards**, Partner Admins and Policy Managers:

* **Are no longer required to self-register via the PMS UI**
* Can be **directly created in Keycloak**
* Can be assigned the **Partner Admin** or **Policy Manager** role in Keycloak
* Can use the **same Keycloak credentials** to log in directly to the PMS portal

This change reduces onboarding steps, eliminates redundant self-registration, and centralizes role management within Keycloak.

{% hint style="info" %}
**Note:** This updated registration flow is applicable to **PMS v1.3.0-Beta4 and later versions only**.
{% endhint %}

#### 3. OIDC Client Enhancement – Additional Configuration Fields

The OIDC client creation flow has been enhanced with a [**new “Additional Information” section**](../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/end-user-guide.md#creating-oidc-client-with-additional-details) in the UI.

This section allows Relying Parties (RPs) to:

* Configure additional parameters
* Customize OIDC client behavior with greater flexibility
* Support advanced integration and use-case specific requirements

{% hint style="info" %}
**Compatibility Note:** The **Additional Information** section is **editable only when PMS is integrated with eSignet version 1.6.2 or above**. For PMS deployments using **eSignet versions lower than 1.6.2**, this section will be visible but **not editable**.
{% endhint %}

#### 4. API Key Expiry Date Management by Partner Admin

An enhancement has been introduced to allow [**editing the expiry date of API keys**](../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/end-user-guide.md#api-key) by partner admin. With this change:

* Partner can continue using the **same API key** for an extended period
* The expiry date can be **updated as it approaches**, without regenerating a new key
* This reduces operational overhead and improves key lifecycle management

#### 5. MISP License Key Expiry Notifications

PMS now provides [**proactive notifications for expiring MISP license keys**](../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/pms-notification.md#what-all-types-of-notifications-does-a-partner-admin-get), targeted at the **Partner Admin** role.

Notifications are delivered through:

* **Email alerts** to the Partner Admin
* **Notifications** in the PMS notification panel
* **Dashboard alerts** within the PMS portal

This enhancement ensures Partner Admins are informed well in advance and can take timely action to renew licenses.

#### 6. Removal of Partner Existence Validation for Partner Admins

Validation checks to verify whether a partner already exists in the PMS database have been removed for Partner Admin operations, simplifying workflows and improving onboarding efficiency.

### Deprecated APIs

<table><thead><tr><th width="198.140625">API Endpoint</th><th width="98.5546875">Method</th><th width="253.6328125">Deprecation</th><th>Replacement</th></tr></thead><tbody><tr><td>/oauth/client</td><td>GET</td><td>This endpoint retrieves a list of all OAuth clients created by the Auth Partners.</td><td><a href="../../id-lifecycle-management/support-systems/partner-management-services/deploy/api-changes-with-pms-revamp.md">/oidc-clients</a></td></tr><tr><td>/oauth/client</td><td>POST</td><td>This endpoint is used for creating OIDC Client.</td><td><a href="../../id-lifecycle-management/support-systems/partner-management-services/deploy/api-changes-with-pms-revamp.md">/oidc-clients</a></td></tr><tr><td>/oauth/client/{client_id}</td><td>GET</td><td>This endpoint retrieves the OIDC client details by client id.</td><td><a href="../../id-lifecycle-management/support-systems/partner-management-services/deploy/api-changes-with-pms-revamp.md">/oidc-clients/{clientId}.“</a></td></tr><tr><td>/oauth/client/{client_id}</td><td>PUT</td><td>This endpoint is used for updating OIDC Client based on client id.</td><td><a href="../../id-lifecycle-management/support-systems/partner-management-services/deploy/api-changes-with-pms-revamp.md">/oidc-clients/{clientId}.“</a></td></tr><tr><td>/partners/{partnerId}/policy/{policyId}/apikey/status</td><td>PATCH</td><td>Service to activate/de-activate partner API key.</td><td><a href="../../id-lifecycle-management/support-systems/partner-management-services/deploy/api-changes-with-pms-revamp.md">/partners/{partnerId}/policies/{policyId}/api-keys/{apiKeyName}</a></td></tr></tbody></table>

### User Stories

<table><thead><tr><th width="166.9453125">Feature</th><th width="151.3046875">Issue ID</th><th>Summary</th></tr></thead><tbody><tr><td>OIDC Client Enchancement</td><td></td><td></td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43551">MOSIP-43551</a></td><td>OIDC Client Enhancement: API POST /oidc-clients</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43552">MOSIP-43552</a></td><td>OIDC Client Enhancement: API PUT /oidc-clients/{clientId}</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43565">MOSIP-43565</a></td><td>Edit OIDC Client Enhancements - Incorporate all changes in UI page</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43571">MOSIP-43571</a></td><td>OIDC Client Enhancement: API PATCH /oidc-clients/{clientId}</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43553">MOSIP-43553</a></td><td>OIDC Client Enhancement: API GET /oidc-clients/{clientId}</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43578">MOSIP-43578</a></td><td>OIDC Client Enhancement: API GET /oidc-clients</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43776">MOSIP-43776</a></td><td>Partner Admin - View OIDC Client Enhancements - Incorporate all changes in UI page.</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43699">MOSIP-43699</a></td><td>View OIDC Client Enhancements - Incorporate all changes in UI page.</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38711">MOSIP-38711</a></td><td>Create OIDC Client Partner Enhancement to Support Multilingual Fields and Additional Information.</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43657">MOSIP-43657</a></td><td>Backward Compatibility for OIDC Client Creation UI Based on eSignet Version.</td></tr><tr><td>Change in Partner Admin/Policy Manager Registration</td><td></td><td></td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-41030">MOSIP-41030</a></td><td>Change in Partner Admin/Policy Manager registration userflow</td></tr><tr><td>Update the Expiry date of API key</td><td></td><td></td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38157">MOSIP-38157</a></td><td>Expiry duration of existing Active API Key should be editable in PMS Partner Admin portal.</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44155">MOSIP-44155</a></td><td>Create new endpoint PATCH /partners/{partnerId}/policies/{policyId}/api-keys/{apiKeyName}</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43536">MOSIP-43536</a></td><td>New API Endpoint: PATCH /partners/{partnerId}/policy/{policyId}/apiKey/expiry-date</td></tr><tr><td>MISP License Key Notifications for Partner Admin</td><td></td><td></td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-42488">MOSIP-42488</a></td><td>MISP License Key Notification: View all Notifications Page (Partner Admin).</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-42862">MOSIP-42862</a></td><td>MISP License Key Notification: Dashboard badge (Partner Admin).</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-42863">MOSIP-42863</a></td><td>MISP License Key Notification in Notification Panel (Partner Admin).</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43187">MOSIP-43187</a></td><td>MISP License Key Notification: via email (Partner Admin).</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43462">MOSIP-43462</a></td><td>Notifications: API Update GET /notifications API to support MISP Notifications.</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43488">MOSIP-43488</a></td><td>Notifications: API Add expiryPeriod optional query param in GET /misp-license API.</td></tr><tr><td>Save &#x26; Publish Policy</td><td></td><td></td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43256">MOSIP-43256</a></td><td>Policies (Partner Admin): Save &#x26; Publish Policy during Policy creation.</td></tr><tr><td>Onboard ABIS Partner</td><td></td><td></td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38721">MOSIP-38721</a></td><td>Partner Admin: ABIS Partner Onboarding in PMS portal.</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43607">MOSIP-43607</a></td><td>Create ABIS Partner.</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43610">MOSIP-43610</a></td><td>Request and Manage ABIS Partner Policy Linking.</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43609">MOSIP-43609</a></td><td>Upload/ Re-Upload ABIS Partner Certificate.</td></tr><tr><td></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-43826">MOSIP-43826</a></td><td>ABIS Partner: API GET /partners/v3?status={status}&#x26;policyGroupAvailable={policyGroupAvailable}&#x26;partnerType=ABIS_Partner</td></tr></tbody></table>

### Known Issues

<table><thead><tr><th width="204.09375">Issue key</th><th>Summary</th></tr></thead><tbody><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-44147">MOSIP-44147</a></td><td>When a Keycloak user who does not have any valid PMS role logs in to the PMS Portal, the page continuously refreshes and results in a 403 Forbidden error.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-44017">MOSIP-44017</a></td><td>PMS - Revamp - MacBook : Keyboard functionality not working for some fields in Additional Information section on Create OIDC Client page in Safari.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-43487">MOSIP-43487</a></td><td>PMS-Revamp - “Pending for approval” status label overlaps with the "linked devices" icon in French language on MacBook (Safari browser).</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-43996">MOSIP-43996</a></td><td>PMS Revamp - Error Message popping up When Editing and adding Client Name With Allowed Max Characters.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-43997">MOSIP-43997</a></td><td>PMS-Revamp -“Forgot Password” and “SignUp” link is visible on the eSignet screen even when their respective toggles are OFF.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-43927">MOSIP-43927</a></td><td>PMS Revamp- Inappropriate error[bad request] message when user enters a value greater than 10 minutes in "consent_expire_in_mins" field.</td></tr></tbody></table>

### Repositories Released

<table><thead><tr><th width="302.3828125">partner-management-services</th><th width="221.76171875">release-1.3.x</th><th>v1.3.0-beta.4</th></tr></thead><tbody><tr><td><strong>partner-management-portal</strong></td><td>release-1.3.x</td><td><a href="https://github.com/mosip/partner-management-portal/tree/v1.3.0-beta.4">v1.3.0-beta.4</a></td></tr><tr><td><strong>partner-management-services</strong></td><td>release-1.3.x</td><td><a href="https://github.com/mosip/partner-management-services/tree/1.3.0-beta.4">1.3.0-beta.4</a></td></tr><tr><td><strong>mosip-data</strong></td><td>release-1.3.x</td><td><a href="https://github.com/mosip/mosip-functional-tests/tree/v1.4.0">v1.3.0-beta.4</a></td></tr></tbody></table>

### Compatible Modules

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

***

### Learn More

* [Services](https://github.com/mosip/partner-management-services/tree/release-1.3.x)
* [Partner Management Portal](https://github.com/mosip/partner-management-portal/tree/release-1.3.x): For code and implementation of Partner Management Portal (revamp),
* [Features](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/overview/features)
* [End User Guide](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding)
* [Technical Guide](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/develop)

### Test Report

For details on the test results, refer here.

