# Features

[Keshav
Singh](https://mosip.atlassian.net/wiki/people/712020:89022ae0-b73b-4f19-a5dc-8cf7cf3c67a7?ref=confluence)*I
reviewed the existing feature documentation at [[PMS Features \| MOSIP
Docs]{.underline}](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/overview/features)
and noticed several inconsistencies in the sequencing and structuring of
the listed features. I've marked the sections with discrepancies in
**red** for easier identification, along with the **newly added
content** that aligns with the latest PMS version updates.*

# Features

The key features of the Authentication Partner and the Partner
Management System are below.

## Ease of Use and Usability Enhancements

### Self-registration

A Partner can self-register with much of the process automated with
minimal manual intervention.

### User Access

·       Login: Existing Partner who has already registered can login to
the portal with email / username and password.

·       Retrieve Password / Forgot Password: Partner will have option to
reset password using the Forget Password option.

**T & C / Consent-** Partner consent refers to voluntary and informed
agreement provided by a partner user on behalf of the Partner
Organisation, to a specific action or process where the users have a
clear understanding of what they are consenting to. User consent is
important to ensure data privacy, where it is compliant to obtain
explicit consent from partners before collecting, processing, or sharing
their personal/ organisation level data.

A detailed description explaining which of their personal and
organisation data is used and for what purposes it will be used in PMP
will be informed while seeking user consent.

### Interface / User Experience

For its user part, the new interface of PMS has undergone a complete
revamp not only in UI but also UX, restructured from the ground up.

**Card view presentation** -- The "Partner User Dashboard" now offers a
Card-based layout, providing a clean and intuitive overview of available
services.\
Each card includes a one-line description to help users quickly
understand its purpose.

**User Profile** -- The user can view their organization name and
username on the top-right corner.\
The dropdown menu provides access to "User Profile" and "Logout"
options.

## Partner Admin Features

### Admin Dashboard

The Admin dashboard provides a structured and modern user experience,
where each major functionality is represented as a card.\
The dashboard includes the following cards:

- Certificate Trust Store

- Partners

- Policies

- Partner Policy-Linking

- SBI-Device

- FTM Chip

- Authentication Services

- **MISP Services**

It also displays pending request counts for Partner Policy-Linking,
SBI-Device, and FTM Chip.

### Certificate Trust Store

Upload, download, or view (List and Detailed view) of Root and
Intermediate Certificates.

### Partners

For self- registered partners:

- Download (Original & MOSIP Signed Certificate), Deactivate Partner, or
  View (List and Detailed View).

For MISP Partners (onboarded by Partner Admin):\
Partner Admins can now onboard and manage MISP Partners through the PMS
portal.\
This includes creation of MISP Partners, uploading CA-signed partner
certificates and selecting policy groups

The tabular view shows all partners, with MISP Partners having
additional options such as:

- Upload / Re-Upload Partner Certificate - option enabled only for MISP
  Partner

- Select Policy Group (if not assigned during creation) - option enabled
  only for MISP Partner

- View Partner Details

- Deactivate Partner

### Policies

This section enables creation and management of policy groups and
individual policies.

- Create Policy Group

- Create Policy

- Authentication Policy

- Data Share Policy

- Clone Policy in Different Policy Groups

- Publish Policy in Draft Status

- Edit Policy in Draft Status

- View (List & Detail View)

- Deactivate Policy Group (if no active policy is linked)

- Deactivate Policy (if no active partner policy mappings exist)

### Partner Policy Linking

Approve, Reject, or View (List and Detail Views).

**Enhancements for MISP Partners:**\
Partner Admins can now request and link MISP-specific policies once a
policy group is assigned.\
The Policy Linking tabular view displays all MISP Partner--Policy
associations and allows quick approval or rejection.

### [Authentication Services]{.underline}

·       OIDC Client:

o   **Tabular View**

o   **View** individual details

o   **Deactivate**

·       API Key:

o   **Tabular View**

o   **View** individual details

o   **Deactivate**

 

### [SBI-Device]{.underline}

 SBI:

·       **Approve/ Reject**

·       **Tabular View**

·       **View** individual details

·       **Deactivate** (if no active devices present)

Device:

·       **Approve/ Reject**

·       **Tabular View**

·       **View** individual details

·       **Deactivate**

Note: For devices that are orphaned without an associated SBI, the
partner admin will only have the option to **Reject**.

 

### [FTM Chip]{.underline}

·       **Approve/ Reject**

·       **Tabular View**

·       **View** individual details

·       **Deactivate**

### [The key features of MISP Partner:]{.underline}

PMS now supports complete lifecycle management of MISP Partners,
including their onboarding, certificate management, policy association,
and MISP License Key management.

**MISP Partner Onboarding:**

- Partner Admins can create new MISP Partners directly from the
  "Partners" card.

- During creation, the Partner Admin must ensure Root and Intermediate
  CA Certificates are already uploaded.

- Once created, a MISP Partner can upload or re-upload their CA-signed
  partner certificate and link a Policy Group (one-time action).

**Policy Linking for MISP Partners:**

- Once a Policy Group is assigned, Partner Admins can request, approve,
  or reject MISP policy linkages.

- The interface provides a clean tabular view showing all the partners
  and the linked policy details.

**MISP Services:**

- A dedicated "MISP Services" card enables management of MISP License
  Keys.

- Partner Admins can:

  - Generate a new MISP License Key (user is expected to enter the
    validity of the license key)

  - Regenerate a license key with updated validity

  - View existing license keys and their details

  - Deactivate active license keys

This new capability allows Partner Admins to manage the full MISP
Partner lifecycle within PMS

**Note:** Selecting a Policy Group and linking policies is optional for
a MISP Partner. However, it is highly recommended to complete these
actions before generating a MISP License Key for the respective MISP
Partner.

**Note:** If multiple MISP license keys are generated for the same
Partner ID, only the latest MISP license key will remain active in the
ID Authentication (IDA) module. Older license keys will be overwritten.
An instruction is provided to admin during MISP license key creation to
exercise caution when creating multiple license keys for the same
Partner.

**Note:** The MISP license key automatically deactivates post expiry

### Partner Type Feature:

#### Key Feature of Authentication Partner

### Partner Certificate: 

- Upload and Re-upload Certificate Authority (CA)-signed Partner
  Certificates easily.

- Download CA-signed Partner Certificates and corresponding MOSIP-signed
  Certificates.

Features here are provided only for partner type **Authentication
Partner**.

- Policies: Request Policy, View, Select Policy Group

- Enable Authentication mechanisms for approved policies

**API Key:**

- Generate API Key for approved policies

- View API Key details in a tabular list

- Deactivate API Keys as needed

**OIDC Client:**

- Create, View, Edit, or Deactivate OIDC Clients

- Access details such as Client IDs and policy associations

Note:

1.  API Key gets auto deactivated after its expiry.

2.  API Key expiration duration can be configured : Newly generated API
    keys should be set to 100 years of expiration duration by default,
    since the time of its creation. **But an** admin-level configuration
    (via config file) allows setting the API key expiration duration in
    days (such that it can accommodate months/years too). But please
    note that existing API keys (created before this feature) remains
    unaffected.

#### The key features of Device Provider

**Partner Certificate**

- Upload and Re-upload CA-signed Partner Certificates

- Download both CA-signed and MOSIP-signed Partner Certificates

**Device Provider Services**

- SBI-Device creation (Add SBI, Add Devices associated with SBI)

- Submit SBI/Device for Partner Admin approval

- Deactivate SBI or associated devices

- View SBI and associated devices

**Note:** SBI and its associated devices are auto-deactivated after SBI
expiry.

#### The key features of FTM Chip Provider

**Partner Certificate**

- Upload and Re-upload CA-signed Partner Certificates

- Download both CA-signed and MOSIP-signed Partner Certificates

**FTM Chip Services**

- Add, View and deactivate FTM details

- Upload, Re-upload, or download FTM chip certificates

## Receive Notifications via PMS portal and email

Users receive timely notifications through both the PMS portal and email
regarding the upcoming expiry of certificates (Root/ Intermediate CA
Certificate, Partner Certificate, FTM Chip Certificate), API Key and SBI
linked to the Partner Management System (PMS).

#### Notification Details

- **Notification Channels**

  - Email

  - PMS Portal notifications

- **Recipients**

  - **Partner Admins**

    - Receive notifications about the expiry of all **Root /
      Intermediate CA certificates** (regardless of who uploaded them)
      which is set to expire in next 30 days.

    - Receive a **weekly summary** listing partners whose partner
      certificates, FTM Chip Certificates, API Keys and SBIs are
      expiring in the next 7 days.

    - Receive individual notifications before the expiry of:

      - Root CA Certificates

      - Intermediate CA Certificates

  - **Partner Users**

    - Receive notifications for partner certificates they personally
      uploaded.

    - Notifications are specific to their uploaded partner certificates
      or corresponding MOSIP signed certificates expiring within the
      next 30 days.

  - FTM Chip Providers receive notifications for FTM Chip Certificates
    they have personally uploaded that are expiring within the next 30
    days.

  - Device Providers receive notifications for SBIs they have created
    that are expiring within the next 30 days.

  - Authentication Partners receive notifications for API Keys they have
    generated that are expiring within the next 30 days.

#### Notification Schedule

- **For Individual Certificate (Root CA/ Intermediate CA/ Partner
  Certificate / FTM Chip Certificate, API Key and SBI) Expirations:**

  - 30 days before expiry

  - 15 days before expiry

  - Daily reminders starting from 10 days before expiry up to the expiry
    date (i.e., from Day - 10 to Day 0).

- **For Weekly Summary Notifications (for Partner Admins)**

  - Sent every 7 days.

  - Lists all partner certificates expiring within the next 7 days.

  - The next weekly notification triggers only after another 7 days.

#### Notification Language Handling

- **Email Notifications**

  - Sent in the language selected by the user during registration
    (partner/partner admin).

- **PMS Portal Notifications**

  - Displayed in the language selected at the time of login, regardless
    of the user's registration language preference.

#### Notification Retention

- Notifications in the PMS portal older than **60 days** are
  **automatically deleted**.

### Audit Logging for Certificates, API Key, SBI Expiry Notifications

To enhance traceability, PMS now captures audit logs for all certificate
expiry-related notifications, including those shown on the portal and
sent via email. Notifications for Root CA, Intermediate CA, Partner, FTM
Chip Certificates & API Keys, SBIs---as well as the Weekly Summary of
partner certificates, FTM Chip Certificates, SBIs, API Keys sent to
Partner Admins---are tracked for both success and failure events. Each
event is recorded in the audit.app_audit_log table with a unique event
ID and reference details. The logs include metadata such as module name,
notification ID, and type. The feature ensures greater transparency and
accountability in certificate lifecycle communication.

In case of weekly summary notifications, audit logs are generated
regardless of whether any partner-specific items---such as FTM Chip
Certificates, SBI, Partner Certificates, or API Keys---are expiring (or
not) in a given week. This includes logging events where no expiry
notifications are triggered, ensuring consistent traceability for all
weekly notification cycles, even if the system determines that there are
no expiring items for the period.

## Browser Support

Full support on Chrome, Firefox, Edge, and Safari ensures a seamless
experience across devices.

## Language Support

·       Multi Language is supported but with the released code- only 3
(Eng, Ara, Fra) languages resource bundles will be given.

Note: The SI will have to add new Resource Bundles directly in UI code
and recompile it since we will no longer use artifactory for Resource
Bundles.

## Compatibility

Optimized for standard browser sizes (laptop/desktop/extra-large
screens) with a responsive design for desktop and laptop views.
