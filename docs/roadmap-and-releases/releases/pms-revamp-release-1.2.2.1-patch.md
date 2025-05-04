# PMS Revamp Release - 1.2.2.1 - Patch

**Release Name**: Partner Management System Revamp (Patch)

**Release Number**: v1.2.2.1

**Release Date**: Coming Soon

### Overview

We are pleased to announce the release of **PMS Revamp v1.2.2.1**, now compatible with multiple backward-compatible versions of the MOSIP Platform.

This patch builds upon PMS v1.2.2.0, introducing key **compatibility fixes**, especially for the following KeyManager versions: v1.1.5, v1.2.0.1, v1.2.1.0, v1.3.0-beta.1

These updates improve backward compatibility and ensure smoother integration across supported MOSIP platform modules.

### **Bug Fixes:**

<table data-header-hidden><thead><tr><th width="263.8046875"></th><th></th></tr></thead><tbody><tr><td><strong>Bug ID</strong></td><td><strong>Description</strong></td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41349">MOSIP-41349</a></td><td>No proper spacing between ‘Re-upload’ and ‘Download’ buttons in Arabic language.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41348">MOSIP-41348</a></td><td>The column names - Certificate expiry date and status are not specified clearly</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41211">MOSIP-41211</a></td><td>An error message is displayed automatically when the user navigates to the View page of any Approved or Pending for approval FTM Chip record, even before clicking the 'Download' button.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41210">MOSIP-41210</a></td><td>The column headers for "List of Policy requests" screen is not aligned properly in French language</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41209">MOSIP-41209</a></td><td><em>Select Partner ID</em> placeholder is not getting translated into French and Arabic language on the ‘Create Policy request’ screen.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41208">MOSIP-41208</a></td><td>The user's first and last name are not displayed in the User Profile or on the Dashboard.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41206">MOSIP-41206</a></td><td>FTM Chip provider is unable to retrieve the expiry date and status values in list of FTM Chip details.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41205">MOSIP-41205</a></td><td>Partner is unable to download the Original &#x26; MOSIP Signed Certificates in Partner Certificate, FTM Chip Certificate and Partner details page.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-40946">MOSIP-40946</a></td><td>Partner Admin is unable to deactivate Partners, OIDC Client and API Key.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-40945">MOSIP-40945</a></td><td>Partner Admin is unable to retrieve the list of uploaded Root and Intermediate certificates.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-40944">MOSIP-40944</a></td><td>Authentication Partner is unable to create OIDC Client and API Key.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-40927">MOSIP-40927</a></td><td>Partner Admin is unable to access the Policies features.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-40926">MOSIP-40926</a></td><td>Unable to upload Partner Certificate</td></tr></tbody></table>

### **Story Development**

<table><thead><tr><th width="515.764892578125">Description</th><th>Story ID</th></tr></thead><tbody><tr><td>New error messages have been customized fof features not available in previous versions of MOSIP Platform</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-41243">MOSIP-41243</a></td></tr></tbody></table>



* **Browser Support:**
  * Complete support on **Chrome, Firefox, Edge and Safari** ensures a seamless user experience across these popular browsers.
* **Language Support:**
  * Multi Language is supported with support for resource bundles in 3 languages (Eng, Ara, Fra). More resource bundles can be easily added by following the following documentation [https://mosip.atlassian.net/wiki/spaces/PMS/pages/1606254602](https://mosip.atlassian.net/wiki/spaces/PMS/pages/1606254602)
* **Compatibility:**
  * Optimized for standard browser sizes (laptop/desktop/tablet/ extra large screens) with responsive UI design for laptop/desktop views.

For detailed description of the various features available, refer to [Feature Documentation](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/functional-overview/auth-partner/features).

### **Repositories Released**

| Repository Released         | Version  |
| --------------------------- | -------- |
| partner-management-services | v1.2.2.1 |
| partner-management-portal   | v1.2.2.1 |
| keymanager                  | v1.1.5.6 |

### Compatible Modules

The following table outlines the tested and certified compatibility of PMS 1.2.2.1 with other backward compatible modules.

| Module/ Repo          | 1.1.5.x versions                                                             | 1.2.2.0 versions                                                        |
| --------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Key Manager**       | v1.1.5.6                                                                     | [v1.3.0-beta.2](https://github.com/mosip/keymanager/tree/v1.3.0-beta.2) |
| **auth manager**      | [1.1.5.5-P2](https://github.com/mosip/mosip-openid-bridge/tree/v1.1.5.5-P2)  | [v1.2.0.1](https://github.com/mosip/mosip-openid-bridge/tree/v1.2.0.1)  |
| **artifactory**       | [1.1.5.5-P2](https://github.com/mosip/artifactory-ref-impl/tree/v1.1.5.5-P2) | [v1.2.0.2](https://github.com/mosip/artifactory-ref-impl/tree/v1.2.0.2) |
| **IDA**               | [1.2.0.1-B4](https://github.com/mosip/id-authentication/tree/v1.2.0.1-B4)    | [v1.2.1.0](https://github.com/mosip/id-authentication/tree/v1.2.1.0)    |
| **eSignet**           | NA                                                                           | [v1.4.1](https://github.com/mosip/esignet/tree/v1.4.1)                  |
| **Reg Proc**          | [v1.1.5.5-P5](https://github.com/mosip/registration/tree/v1.1.5.5-P5)        | [v1.2.0.1](https://github.com/mosip/registration/tree/v1.2.0.1)         |
| **Notifier (Kernel)** | [1.1.5.3](https://github.com/mosip/commons/tree/v1.1.5.3/kernel)             | [v1.2.0.1](https://github.com/mosip/commons/tree/v1.2.0.1/kernel)       |
| **Audit manager**     | mosipid/kernel-notification-service:1.1.5.3                                  | [v1.2.0.1](https://github.com/mosip/audit-manager/tree/v1.2.0.1)        |
| **ID Repo**           | [v1.1.5.5-P2](https://github.com/mosip/id-repository/tree/v1.1.5.5-P2)       | [v1.2.1.0](https://github.com/mosip/id-repository/tree/v1.2.1.0)        |
| **datashare**         | [1.1.5.3-P2](https://github.com/mosip/durian/tree/v1.1.5.3-P2)               | [v1.2.0.1](https://github.com/mosip/durian/tree/v1.2.0.1)               |
| **Keycloak**          | mosipid/mosip-keycloak:9.0.0                                                 | [v1.2.0.1](https://github.com/mosip/keycloak/tree/v1.2.0.1)             |
| **config-server**     | [1.1.0](https://github.com/mosip/mosip-config/tree/v1.1.0)                   | [v1.1.2](https://github.com/mosip/mosip-config/tree/v1.1.2)             |
| **Websub**            | NA                                                                           | [v1.2.0.1](https://github.com/mosip/websub/tree/v1.2.0.1)               |

### Documentation

#### Services

For code and implementation of Partner Management Services, refer [here](https://github.com/mosip/partner-management-services/tree/release-1.2.2.x).

#### Partner Management Portal UI

For code and implementation of Partner Management Portal (revamp) , refer [here](https://github.com/mosip/partner-management-portal/tree/release-1.2.2.x).

[Partner Management Portal End User Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/functional-overview).

[Partner Management Services Deployment Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/technical-overview/build-and-development-guide)

[PMS Revamp Configuration Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/technical-overview/pms-configuration-guide)

[UI Developer’s Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/technical-overview/ui-developers-guide)

[Backend Developer’s Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/technical-overview/backend-developers-guide)

[API Documentation](https://mosip.stoplight.io/docs/partner-management-portal-revamp).

[Test Report](https://github.com/mosip/test-management/tree/master/PMS%20Revamp/1.2.2.0)

