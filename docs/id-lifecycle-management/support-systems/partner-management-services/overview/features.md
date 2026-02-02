# Features

### Overview

The Partner Management System (PMS) provides a centralized platform for onboarding, governing, and managing partners integrating with the MOSIP ecosystem. It enables secure partner registration, certificate trust establishment, policy configuration, and operational approvals while ensuring compliance with MOSIP security and governance requirements.

PMS supports multiple partner types and enforces role-based access through integration with Keycloak. The system offers a modern, intuitive user interface for both Partner Users and Partner Admins, enabling efficient management of partner lifecycles, certificates, policies, authentication services, devices, and notifications.

### Ease of Use and Usability Enhancements

#### Self-Registration and User Access

PMS allows partners to self-register through a guided and partially automated process, reducing the need for manual intervention. During registration, users provide organizational and contact details and explicitly consent to Terms and Conditions, which clearly describe how personal and organizational data will be used within PMS.

Once registered, partners can log in using their email or username and password. Password recovery is supported through the “Forgot Password” option, enabling secure credential reset without administrative involvement.

**Note:** From PMS version **1.3.0 – beta 4 onward**, Partner Admin users are no longer required to self-register via the PMS UI. Instead, they are created directly in Keycloak and assigned the appropriate Partner Admin role.

#### Interface and User Experience

The PMS interface has undergone a comprehensive UI and UX revamp. The system now follows a card-based design approach that allows users to quickly identify and access available services. Each card represents a major functional area and includes concise descriptions to improve discoverability.

The user profile section, accessible from the top-right corner, displays the organization name and username and provides options to access the user profile or log out. The interface is optimized for desktop and laptop usage, with responsive layouts ensuring usability across supported screen sizes.
<!-- 

Please refer here for more detials (@Keshav Singh PMS collab Guide Link)

-->

### Partner Admin Capabilities

#### Admin Dashboard <!--  ( @Keshav Singh Link to the Partner Admin User Guide ) -->

The Partner Admin Dashboard serves as the central control panel for governance and operational oversight. It presents all major administrative functions as individual cards, including certificate management, partner onboarding, policy governance, authentication services, and device approvals.

The dashboard also highlights pending approval counts for Partner Policy Linking, SBI–Device, and FTM Chip requests, allowing Partner Admins to quickly identify actions requiring attention.

#### Certificate Trust Store Management <!-- (@Keshav Singh Link to the Certificate Upload Guide) --> 

PMS provides a centralized Certificate Trust Store to establish cryptographic trust across the partner ecosystem. Partner Admins can upload, view, and download Root and Intermediate CA certificates, which form the foundation of trust validation for all partner certificates.

The system continuously tracks certificate validity and triggers expiry notifications to prevent service disruption. Uploaded certificates are validated for structure and integrity before being accepted into the trust store.

### Partner Onboarding and Management

#### Partner Types Supported <!--  (@Keshav Singh Link to the main page where all partner are defined) --> 

PMS supports onboarding and lifecycle management for multiple partner types, including Authentication Partners, MISP Partners, ABIS Partners, Device Providers, FTM Chip Providers, and Credential Partners. Each partner type is governed by role-specific permissions and service capabilities.

#### Partner Management <!-- (@Keshav Singh Link to the self registration of partner user guide) --> 

Partners can be viewed in both list and detailed views. Depending on the partner type and onboarding mode, Partner Admins can deactivate partners, download original and MOSIP-signed certificates, and manage policy associations.

For **self-registered partners**, PMS supports certificate download and lifecycle management.\
For **MISP Partners (**@Keshav Singh **Link to MISP partner Onboarding) and ABIS Partners (**@Keshav Singh **Link to the ABIS partner onboarding)**, onboarding and management are handled exclusively by Partner Admins through the PMS UI.

### Policy Management and Governance

PMS provides comprehensive policy governance through Policy Groups and individual policies. Policy Groups act as logical containers for related policies and enable structured access control across partner types.

These activities are performed by users assigned the **Policy Manager** role in Keycloak. Once a user is created in Keycloak and granted the Policy Manager role, they can manage policies within PMS.

Within a Policy Group, a Policy Manager can create Authentication Policies and Data Share Policies, edit policies in draft state, publish policies, clone policies across Policy Groups, and deactivate policies when no active partner–policy mappings exist.

#### Partner Policy Linking <!-- (@Keshav Singh Link to the Policy manager user guide) --> 

Once a Policy Group is assigned, partners can request policies through the Partner Policy Linking module. Partner Admins review these requests and can approve or reject them through a structured workflow.

Enhancements are available for MISP Partners, allowing Partner Admins to manage MISP-specific policy associations through a dedicated tabular interface.

**Note:** For **ABIS Partners**, only **Data Share Policies** must be selected during policy linking.

### Authentication Services

#### OIDC Client Management <!--  ( @Keshav Singh Link to the OIDC client creation guide) -->

PMS enables Authentication Partners to create, manage, and associate OIDC Clients with approved policies. Users can view OIDC Client details in both tabular and detailed views, including client identifiers and configuration settings.

Partner Admins and authorized Partner Users can access a **read-only view** of OIDC Client details to review mandatory and additional information without modifying configurations.

**Note:** For PMS deployments using eSignet versions **1.6.1 and higher**, OIDC Client creation includes additional information fields to support enhanced customization for relying parties.

#### API Key Management <!--  ( @Keshav Singh Link to the API key generation User guide) -->

API Keys can be generated for approved policies to enable secure access to authentication services. Generated keys are visible in a tabular view and can be deactivated as required.

API Keys automatically deactivate upon expiry. By default, newly generated API Keys are configured with a long expiration duration (100 years), though this value can be overridden through admin-level configuration.

### Device and Biometric Ecosystem Management

#### SBI and Device Management <!--  ( @Keshav Singh Link to the Device Partner Guide) --> 

Device Providers can register SBIs and associate devices under them. SBIs and devices must be submitted for Partner Admin approval before activation. PMS supports viewing, approving, rejecting, and deactivating SBIs and devices.

**Note:** Orphaned devices (devices without an associated SBI) can only be rejected by Partner Admins. SBIs and associated devices are automatically deactivated upon expiry.

#### FTM Chip Management <!-- ( @Keshav Singh Link tot he FTM partner guide) -->

FTM Chip Providers can add, view, and deactivate FTM chip details and manage FTM chip certificates. Partner Admins review and approve or reject submitted FTM chip requests.

### MISP Services

PMS includes a dedicated MISP Services module for managing MISP License Keys. Partner Admins can generate new license keys, regenerate them with updated validity, view existing keys, and deactivate active keys.

If multiple license keys are generated for the same Partner ID, only the latest key remains active in the ID Authentication (IDA) module. License keys automatically deactivate upon expiry.

**Note:** Selecting a Policy Group and linking policies is optional for MISP Partners, but it is strongly recommended before generating a MISP License Key.

### Notifications and Alerts

PMS provides proactive notifications through both the PMS portal and email. Notifications cover certificate expiries (Root, Intermediate, Partner, FTM Chip), API Keys, SBIs, and weekly summary updates for Partner Admins.

Notifications are sent at defined intervals: 30 days before expiry, 15 days before expiry, and daily reminders starting 10 days prior to expiry. Weekly summary notifications are sent every seven days.

Email notifications are delivered in the language selected during user registration, while PMS portal notifications follow the language selected at login.

Please refer here for more details <!--  (@Keshav Singh Link to the Notification user guide) -->

**Note:** Portal notifications older than 60 days are automatically deleted.

### Audit Logging and Traceability

PMS captures audit logs for all certificate and expiry-related notifications, including both portal and email notifications. Logs are recorded for success and failure events and include metadata such as module name, notification ID, and event type.

For weekly summary notifications, audit logs are generated even when no expiring items are identified, ensuring consistent traceability across notification cycles.

### Browser, Language, and Compatibility Support

PMS fully supports Chrome, Firefox, Edge, and Safari browsers. The UI is optimized for standard desktop and laptop screen sizes with responsive layouts.

Multi-language support is enabled. However, only English, Arabic, and French resource bundles are provided by default. Additional languages must be added directly in the UI code and require recompilation.








<!--

Old content


## Overview

The key features of the Authentication Partner and the Partner Management System are discussed here through sections.

### Ease of Use and Usability Enhancements

#### Self-registration

A Partner can self-register with much of the process automated with minimal manual intervention.

#### User Access

* **Login**: Existing Partner who has already registered can login to the portal with email / username and password.
* **Retrieve Password / Forgot Password**: Partner will have option to reset password using the **Forget Password** option.

**T & C / Consent-** Partner consent refers to voluntary and informed agreement provided by a partner user on behalf of the Partner Organisation, to a specific action or process where the users have a clear understanding of what they are consenting to. User consent is important to ensure data privacy, where it is compliant to obtain explicit consent from partners before collecting, processing, or sharing their personal/organisation level data.

A detailed description explaining which of their personal and organisational data is used and for what purposes it will be used in PMP will be informed while seeking user consent.

#### Interface / User Experience

For its user part, the new interface of PMS has undergone a complete revamp not only in UI but also UX, restructured from the ground up.

**Card view presentation** -The "Partner User Dashboard" now offers a Card-based layout, providing a clean and intuitive overview of available services. Each card includes a one-line description to help users quickly understand its purpose.

**User Profile** -- The user can view their organization name and username on the top-right corner. The dropdown menu provides access to "User Profile" and "Logout" options.

### Partner Admin Features

#### Admin Dashboard

The Admin dashboard provides a structured and modern user experience, where each major functionality is represented as a card. The dashboard includes the following cards:

* Certificate Trust Store
* Partners
* Policies
* Partner Policy-Linking
* SBI-Device
* FTM Chip
* Authentication Services
* **MISP Services**

It also displays pending request counts for Partner Policy-Linking, SBI-Device, and FTM Chip.

#### Certificate Trust Store

Upload, download, or view (List and Detailed view) of Root and Intermediate Certificates.

#### Partners

For self- registered partners:

* Download (Original & MOSIP Signed Certificate), Deactivate Partner, or View (List and Detailed View).

For MISP Partners (onboarded by Partner Admin):

Partner Admins can now onboard and manage MISP Partners through the PMS portal. This includes creation of MISP Partners, uploading CA-signed partner certificates and selecting policy groups.

The tabular view shows all partners, with MISP Partners having additional options such as:

* Upload / Re-Upload Partner Certificate - option enabled only for MISP Partner
* Select Policy Group (if not assigned during creation) - option enabled only for MISP Partner
* View Partner Details
* Deactivate Partner

#### Policies

This section enables creation and management of policy groups and individual policies.

* Create Policy Group
* Create Policy
* Authentication Policy
* Data Share Policy
* Clone Policy in Different Policy Groups
* Publish Policy in Draft Status
* Edit Policy in Draft Status
* View (List & Detail View)
* Deactivate Policy Group (if no active policy is linked)
* Deactivate Policy (if no active partner policy mappings exist)

#### Partner Policy Linking

Approve, Reject, or View (List and Detail Views).

**Enhancements for MISP Partners:**

Partner Admins can now request and link MISP-specific policies once a policy group is assigned. The Policy Linking tabular view displays all MISP Partner--Policy associations and allows quick approval or rejection.

#### Authentication Services

* **OIDC Client**:
  * **Tabular View**
  * **View** individual details
  * **Deactivate**
  * **API Key**:
    * **Tabular View**
    * **View** individual details
    * **Deactivate**

#### SBI-Device

**SBI:**

* **Approve/ Reject**
* **Tabular View**
* **View** individual details
* **Deactivate** (if no active devices present)

Device:

* **Approve/ Reject**
* **Tabular View**
* **View** individual details
* **Deactivate**

{% hint style="warning" %}
Note: For devices that are orphaned without an associated SBI, the partner admin will only have the option to **Reject**.
{% endhint %}

#### FTM Chip

* **Approve/ Reject**
* **Tabular View**
* **View** individual details
* **Deactivate**

#### The key features of MISP Partner

PMS now supports complete lifecycle management of MISP Partners, including their onboarding, certificate management, policy association, and MISP License Key management.

**MISP Partner Onboarding:**

* Partner Admins can create new MISP Partners directly from the "Partners" card.
* During creation, the Partner Admin must ensure Root and Intermediate CA Certificates are already uploaded.
* Once created, a MISP Partner can upload or re-upload their CA-signed partner certificate and link a Policy Group (one-time action).

**Policy Linking for MISP Partners:**

* Once a Policy Group is assigned, Partner Admins can request, approve, or reject MISP policy linkages.
* The interface provides a clean tabular view showing all the partners and the linked policy details.

**MISP Services:**

* A dedicated "MISP Services" card enables management of MISP License Keys.
* Partner Admins can:
  * Generate a new MISP License Key (user is expected to enter the validity of the license key)
  * Regenerate a license key with updated validity
  * View existing license keys and their details
  * Deactivate active license keys

This new capability allows Partner Admins to manage the full MISP Partner lifecycle within PMS

{% hint style="warning" %}
**Note:**

* Selecting a Policy Group and linking policies is optional for a MISP Partner. However, it is highly recommended to complete these actions before generating a MISP License Key for the respective MISP Partner.
* If multiple MISP license keys are generated for the same Partner ID, only the latest MISP license key will remain active in the ID Authentication (IDA) module. Older license keys will be overwritten. An instruction is provided to admin during MISP license key creation to exercise caution when creating multiple license keys for the same Partner.
* The MISP license key automatically deactivates post expiry
{% endhint %}

#### Partner Type Feature

**Key Feature of Authentication Partner**

#### Partner Certificate

* Upload and Re-upload Certificate Authority (CA)-signed Partner Certificates easily.
* Download CA-signed Partner Certificates and corresponding MOSIP-signed Certificates.

Features here are provided only for partner type **Authentication Partner**.

* Policies: Request Policy, View, Select Policy Group
* Enable Authentication mechanisms for approved policies

**API Key:**

* Generate API Key for approved policies
* View API Key details in a tabular list
* Deactivate API Keys as needed

**OIDC Client:**

* Create, View, Edit, or Deactivate OIDC Clients
* Access details such as Client IDs and policy associations

{% hint style="warning" %}
**Note**:

1. API Key gets auto deactivated after its expiry.
2. API Key expiration duration can be configured : Newly generated API keys should be set to 100 years of expiration duration by default, since the time of its creation. **But an** admin-level configuration (via config file) allows setting the API key expiration duration in days (such that it can accommodate months/years too). But please note that existing API keys (created before this feature) remains unaffected.
{% endhint %}

**The key features of Device Provider**

**Partner Certificate**

* Upload and Re-upload CA-signed Partner Certificates
* Download both CA-signed and MOSIP-signed Partner Certificates

**Device Provider Services**

* SBI-Device creation (Add SBI, Add Devices associated with SBI)
* Submit SBI/Device for Partner Admin approval
* Deactivate SBI or associated devices
* View SBI and associated devices

**Note:** SBI and its associated devices are auto-deactivated after SBI expiry.

**The key features of FTM Chip Provider**

**Partner Certificate**

* Upload and Re-upload CA-signed Partner Certificates
* Download both CA-signed and MOSIP-signed Partner Certificates

**FTM Chip Services**

* Add, View and deactivate FTM details
* Upload, Re-upload, or download FTM chip certificates

### Receive Notifications via PMS portal and email

Users receive timely notifications through both the PMS portal and email regarding the upcoming expiry of certificates (Root/ Intermediate CA Certificate, Partner Certificate, FTM Chip Certificate), API Key and SBI linked to the Partner Management System (PMS).

**Notification Details**

* **Notification Channels**
  * Email
  * PMS Portal notifications
* **Recipients**
  * **Partner Admins**
    * Receive notifications about the expiry of all **Root / Intermediate CA certificates** (regardless of who uploaded them) which is set to expire in next 30 days.
    * Receive a **weekly summary** listing partners whose partner certificates, FTM Chip Certificates, API Keys and SBIs are expiring in the next 7 days.
    * Receive individual notifications before the expiry of:
      * Root CA Certificates
      * Intermediate CA Certificates
  * **Partner Users**
    * Receive notifications for partner certificates they personally uploaded.
    * Notifications are specific to their uploaded partner certificates or corresponding MOSIP signed certificates expiring within the next 30 days.
  * FTM Chip Providers receive notifications for FTM Chip Certificates they have personally uploaded that are expiring within the next 30 days.
  * Device Providers receive notifications for SBIs they have created that are expiring within the next 30 days.
  * Authentication Partners receive notifications for API Keys they have generated that are expiring within the next 30 days.

**Notification Schedule**

* **For Individual Certificate (Root CA/ Intermediate CA/ Partner Certificate / FTM Chip Certificate, API Key and SBI) Expirations:**
  * 30 days before expiry
  * 15 days before expiry
  * Daily reminders starting from 10 days before expiry up to the expiry date (i.e., from Day - 10 to Day 0).
* **For Weekly Summary Notifications (for Partner Admins)**
  * Sent every 7 days.
  * Lists all partner certificates expiring within the next 7 days.
  * The next weekly notification triggers only after another 7 days.

**Notification Language Handling**

* **Email Notifications**
  * Sent in the language selected by the user during registration (partner/partner admin).
* **PMS Portal Notifications**
  * Displayed in the language selected at the time of login, regardless of the user's registration language preference.

**Notification Retention**

* Notifications in the PMS portal older than **60 days** are **automatically deleted**.

#### Audit Logging for Certificates, API Key, SBI Expiry Notifications

To enhance traceability, PMS now captures audit logs for all certificate expiry-related notifications, including those shown on the portal and sent via email. Notifications for Root CA, Intermediate CA, Partner, FTM Chip Certificates & API Keys, SBIs---as well as the Weekly Summary of partner certificates, FTM Chip Certificates, SBIs, API Keys sent to Partner Admins---are tracked for both success and failure events. Each event is recorded in the audit.app\_audit\_log table with a unique event ID and reference details. The logs include metadata such as module name, notification ID, and type. The feature ensures greater transparency and accountability in certificate lifecycle communication.

In case of weekly summary notifications, audit logs are generated regardless of whether any partner-specific items---such as FTM Chip Certificates, SBI, Partner Certificates, or API Keys---are expiring (or not) in a given week. This includes logging events where no expiry notifications are triggered, ensuring consistent traceability for all weekly notification cycles, even if the system determines that there are no expiring items for the period.

### Browser Support

Full support on Chrome, Firefox, Edge, and Safari ensures a seamless experience across devices.

### Language Support

* Multi Language is supported but with the released code- only 3 (Eng, Ara, Fra) languages resource bundles will be given.

Note: The SI will have to add new Resource Bundles directly in UI code and recompile it since we will no longer use artifactory for Resource Bundles.

### Compatibility

Optimized for standard browser sizes (laptop/desktop/extra-large screens) with a responsive design for desktop and laptop views.



***


-->









