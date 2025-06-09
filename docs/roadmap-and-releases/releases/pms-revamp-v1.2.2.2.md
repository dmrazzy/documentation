# PMS Revamp v1.2.2.2 (Patch)

**Release Name**: Partner Management System Revamp (Patch)

**Release Number**: v1.2.2.2

**Release Date**: <em>Coming Soon</em>


### Overview

We are pleased to announce the release of **PMS Revamp v1.2.2.2**, a patch focused on resolving key issues related to backward compatibility with earlier versions of the MOSIP Platform.

This release builds upon PMS v1.2.2.0 and **extends the enhancements delivered in v1.2.2.1**, introducing further compatibility fixes — particularly for the following KeyManager versions: v1.1.5, v1.2.0.1, v1.2.1.0, v1.3.0-beta.1 and 1.3.0-beta.2.

These updates enhance platform interoperability and ensure smoother integration across supported MOSIP modules.



### **Bug Fixes**

<table><thead><tr><th width="263.8046875">Bug ID</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41629">MOSIP-41629</a></td><td>For non-valid PMS partners, when required attributes are missing in Keycloak (but roles are present), PMS portal returns a “User ID not found” error on login.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41628">MOSIP-41628</a></td><td>For valid PMS partners with missing Keycloak attributes in Keycloak (but required role mappings present), the PMS dashboard does not display all expected cards and the user profile shows incomplete details.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41594">MOSIP-41594</a></td><td>Compatibility configurations have been enabled ( `true` by default) in the `release-1.2.2.x` GitHub branch. Country-specific deployments can disable these settings as needed. The README has also been updated with relevant configuration details.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41592">MOSIP-41592</a></td><td>In screens where a feature is unsupported, the info note now displays the feature name in <em>italics</em> for better visual distinction.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41445">MOSIP-41445</a></td><td>Automation failures observed in API test rigs caused by issues with Key Manager, OIDC authentication, and partner certificate retrieval.</td></tr></tbody></table>


### **Known Issues**

<table><thead><tr><th width="176.47869873046875">JIRA ID</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41707">MOSIP-41707</a></td><td>The “Account Setup Required” popup does not currently support RTL layout for Arabic language users. This issue will be addressed in the develop branch and is planned for delivery in an upcoming release.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41719">MOSIP-41719 </a></td><td>When attributes are missing in Keycloak, the user profile currently displays “Not available” without any actionable next steps. A design enhancement is under consideration to better handle scenarios involving missing attributes.</td></tr></tbody></table>


### **Repositories Released**

| Repository Released         | Version                                                                        |
| --------------------------- | ------------------------------------------------------------------------------ |
| partner-management-services | [v1.2.2.2](https://github.com/mosip/partner-management-services/tree/v1.2.2.1) |
| partner-management-portal   | [v1.2.2.2](https://github.com/mosip/partner-management-portal/tree/v1.2.2.1)   |



### Compatible Modules

The following table outlines the tested and certified compatibility of PMS 1.2.2.2 with other backward compatible modules.

| Module/ Repo          | 1.2.2.0 versions                                                        |
| --------------------- | ----------------------------------------------------------------------- |
| **Key Manager**       | [v1.3.0-beta.2](https://github.com/mosip/keymanager/tree/v1.3.0-beta.2) |
| **auth manager**      | [v1.2.0.1](https://github.com/mosip/mosip-openid-bridge/tree/v1.2.0.1)  |
| **artifactory**       | [v1.2.0.2](https://github.com/mosip/artifactory-ref-impl/tree/v1.2.0.2) |
| **IDA**               | [v1.2.1.0](https://github.com/mosip/id-authentication/tree/v1.2.1.0)    |
| **eSignet**           | [v1.4.1](https://github.com/mosip/esignet/tree/v1.4.1)                  |
| **Reg Proc**          | [v1.2.0.1](https://github.com/mosip/registration/tree/v1.2.0.1)         |
| **Notifier (Kernel)** | [v1.2.0.1](https://github.com/mosip/commons/tree/v1.2.0.1/kernel)       |
| **Audit manager**     | [v1.2.0.1](https://github.com/mosip/audit-manager/tree/v1.2.0.1)        |
| **ID Repo**           | [v1.2.1.0](https://github.com/mosip/id-repository/tree/v1.2.1.0)        |
| **datashare**         | [v1.2.0.1](https://github.com/mosip/durian/tree/v1.2.0.1)               |
| **Keycloak**          | [v1.2.0.1](https://github.com/mosip/keycloak/tree/v1.2.0.1)             |
| **config-server**     | [v1.1.2](https://github.com/mosip/mosip-config/tree/v1.1.2)             |
| **Websub**            | [v1.2.0.1](https://github.com/mosip/websub/tree/v1.2.0.1)               |


To know more on the module- wise versions we have tested within 1.1.5.x platform , please refer to QA reports.

### Documentation

* [Feature Documentation](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/functional-overview/auth-partner/features)
* [End User Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/functional-overview)
* [Services Deployment Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/technical-overview/build-and-development-guide)
* [UI Developer’s Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/technical-overview/ui-developers-guide)
* [Backend Developer’s Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/technical-overview/backend-developers-guide)
* [API Documentation](https://mosip.stoplight.io/docs/partner-management-portal-revamp)
* [Test Report](https://github.com/mosip/test-management/tree/master/PMS%20Revamp/1.2.2.1)
