# Partner Management System 1.2.2.3

**Release Name**: Partner Management System&#x20;

**Release Number**: 1.2.2.3

**Release Date**: 27th Nov 2025

### **Overview** <a href="#overview" id="overview"></a>

We are pleased to announce the release of PMS v1.2.2.3, a patch update focused exclusively on resolving critical functional bugs and automation-related issues identified in earlier versions.

This release builds upon PMS v1.2.2.2 and delivers improvements that enhance stability, user experience, and reliability across partner onboarding, certificate management, and verification workflows.

### Bug Fixes <a href="#bug-fixes" id="bug-fixes"></a>

| **Bug ID**                                                    | **Description**                                                                              |
| ------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| [MOSIP-35301](https://mosip.atlassian.net/browse/MOSIP-35301) | PMS 1.2.0.1 - Unable to regenerate MISP licensekey after expiry.                             |
| [MOSIP-42232](https://mosip.atlassian.net/browse/MOSIP-42232) | High number of failures for "PMS API Automation 1.2.2.2" in "released" env                   |
| [MOSIP-42571](https://mosip.atlassian.net/browse/MOSIP-42571) | PMS Legacy: Incorrect JSON format in `pms_auth_policy.csv` DML script in v1.2.2.2            |
| [MOSIP-42606](https://mosip.atlassian.net/browse/MOSIP-42606) | PMS 1.2.2.2 (Collab) API Automation Report: Smoke Testing report has many skips and failures |
| [MOSIP-42843](https://mosip.atlassian.net/browse/MOSIP-42843) | PMS : Error message re-structuring                                                           |

### **Story Development** <a href="#story-development" id="story-development"></a>

| **Story ID**                                                  | **Description**                                                                                                              |
| ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| [MOSIP-42249](https://mosip.atlassian.net/browse/MOSIP-42249) | PMS 1.2.2.3 (Java 11 compatible): (PMS Legacy) Policy expiry date is hardcoded in partner policy table and auth policy table |

### Repositories Released <a href="#repositories-released" id="repositories-released"></a>

| **Repository Released**     | **Version**                                                                    |
| --------------------------- | ------------------------------------------------------------------------------ |
| partner-management-services | [v1.2.2.3](https://github.com/mosip/partner-management-services/tree/v1.2.2.3) |
| partner-management-portal   | [v1.2.2.3](https://github.com/mosip/partner-management-portal/tree/v1.2.2.3)   |

### Compatible Modules <a href="#compatible-modules" id="compatible-modules"></a>

| **Module/ Repo**      | **1.1.5.x versions**                                                         | **1.2.2.0 versions**                                                    |
| --------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Key Manager**       | [v1.1.5.5-P3](https://github.com/mosip/keymanager/tree/v1.1.5.5-P3)          | [v1.3.0-beta.2](https://github.com/mosip/keymanager/tree/v1.3.0-beta.2) |
| **auth manager**      | [1.1.5.5-P2](https://github.com/mosip/mosip-openid-bridge/tree/v1.1.5.5-P2)  | [v1.2.0.1](https://github.com/mosip/mosip-openid-bridge/tree/v1.2.0.1)  |
| **artifactory**       | [1.1.5.5-P2](https://github.com/mosip/artifactory-ref-impl/tree/v1.1.5.5-P2) | [v1.2.0.2](https://github.com/mosip/artifactory-ref-impl/tree/v1.2.0.2) |
| **IDA**               | [1.2.0.1-B4](https://github.com/mosip/id-authentication/tree/v1.2.0.1-B4)    | [v1.2.1.0](https://github.com/mosip/id-authentication/tree/v1.2.1.0)    |
| **eSignet**           | NA                                                                           | [v1.4.1](https://github.com/mosip/esignet/tree/v1.4.1)                  |
| **Reg Proc**          | [v1.1.5.5-P5](https://github.com/mosip/registration/tree/v1.1.5.5-P5)        | [v1.2.0.1](https://github.com/mosip/registration/tree/v1.2.0.1)         |
| **Notifier (Kernel)** | [1.1.5.3](https://github.com/mosip/commons/tree/v1.1.5.3/kernel)             | [v1.2.0.1](https://github.com/mosip/commons/tree/v1.2.0.1/kernel)       |
| **Audit manager**     | [1.1.5.5](https://github.com/mosip/audit-manager/tree/1.1.5.5)               | [v1.2.0.1](https://github.com/mosip/audit-manager/tree/v1.2.0.1)        |
| **ID Repo**           | [v1.1.5.5-P2](https://github.com/mosip/id-repository/tree/v1.1.5.5-P2)       | [v1.2.1.0](https://github.com/mosip/id-repository/tree/v1.2.1.0)        |
| **datashare**         | [1.1.5.3-P2](https://github.com/mosip/durian/tree/v1.1.5.3-P2)               | [v1.2.0.1](https://github.com/mosip/durian/tree/v1.2.0.1)               |
| **Keycloak**          | [mosipid/mosip-keycloak:9.0.0](https://github.com/mosip/keycloak)            | [v1.2.0.1](https://github.com/mosip/keycloak/tree/v1.2.0.1)             |
| **config-server**     | [1.1.0](https://github.com/mosip/mosip-config/tree/v1.1.0)                   | [v1.1.2](https://github.com/mosip/mosip-config/tree/v1.1.2)             |
| **Websub**            | NA                                                                           | [v1.2.0.1](https://github.com/mosip/websub/tree/v1.2.0.1)               |

### Documentation <a href="#documentation" id="documentation"></a>

* [Feature Documentation](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/functional-overview/auth-partner/features)
* [End User Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/functional-overview)
* [Services Deployment Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/technical-overview/build-and-development-guide)
* [UI Developer’s Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/technical-overview/ui-developers-guide)
* [Backend Developer’s Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/pms-revamp/technical-overview/backend-developers-guide)
* [API Documentation](https://mosip.stoplight.io/docs/partner-management-portal-revamp)
* [QA Report](test-report.md)
