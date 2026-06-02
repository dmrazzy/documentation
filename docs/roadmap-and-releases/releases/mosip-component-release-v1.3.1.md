# MOSIP Component Release - v1.3.1

**Release Version:** MOSIP Component Release - v1.3.1

**Release Type:** Patch Release

**Release Date:** Coming Soon!

## Overview

This patch release addresses an issue related to invalid file paths in the licenses folder across multiple MOSIP repositories caused by an extra space in the folder path naming convention.

The issue impacted license file path validation and repository consistency across components. As part of this release, the extra space present in the license folder paths has been corrected to ensure proper folder structure alignment and eliminate invalid path references.

## Highlights & Enhancements

### License Folder Path Correction

* Corrected invalid file paths in the licenses folder caused by extra spaces in folder names.
* Standardized the license folder structure across impacted repositories.
* Improved repository consistency and ensured proper path resolution for license-related validations and processing.

## Bugs Fixed

<table><thead><tr><th width="161.23828125">Issue</th><th width="425.08203125">Description</th><th>JIRA</th></tr></thead><tbody><tr><td>License Folder Path Fix</td><td>Fixed invalid file paths in license folders by removing extra spaces from folder paths across impacted repositories.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-44867">MOSIP-44867</a></td></tr></tbody></table>

## Repositories Released

| Repository           | Tag Version                                                         |
| -------------------- | ------------------------------------------------------------------- |
| resident-service     | [v1.3.1](https://github.com/mosip/resident-services/tree/v1.3.1)    |
| print                | [v1.3.1](https://github.com/mosip/print/tree/v1.3.1)                |
| websub               | [v1.3.1](https://github.com/mosip/websub/tree/v1.3.1)               |
| admin-services       | [v1.3.1](https://github.com/mosip/admin-services/tree/v1.3.1)       |
| bio-utils            | [v1.3.1](https://github.com/mosip/bio-utils/tree/v1.3.1)            |
| artifactory-ref-impl | [v1.3.1](https://github.com/mosip/artifactory-ref-impl/tree/v1.3.1) |
| mosip-data           | [v1.3.1](https://github.com/mosip/mosip-data/tree/v1.3.1)           |
| audit-manager        | [v1.3.1](https://github.com/mosip/audit-manager/tree/v1.3.1)        |
| mosip-openid-bridge  | [v1.3.1](https://github.com/mosip/mosip-openid-bridge/tree/v1.3.1)  |
| khazan               | [v1.3.1](https://github.com/mosip/khazana/tree/v1.3.1)              |
| nfiq                 | [v0.1.1](https://github.com/mosip/nfiq/tree/v0.1.1)                 |
| biosdk-client        | [v1.3.1](https://github.com/mosip/biosdk-client/tree/v1.3.1)        |
| demosdk              | [v1.3.1](https://github.com/mosip/demosdk/tree/v1.3.1)              |
| captcha              | [v0.1.1](https://github.com/mosip/captcha/tree/v0.1.1)              |
| image-compressor     | [v0.1.1](https://github.com/mosip/image-compressor/tree/v0.1.1)     |

## Compatible Modules

| Module/Repo                 | Compatible Version                                                             |
| --------------------------- | ------------------------------------------------------------------------------ |
| partner-management-services | [v1.2.2.3](https://github.com/mosip/partner-management-services/tree/v1.2.2.3) |
| resident-ui                 | [v0.9.0](https://github.com/mosip/resident-ui/releases/tag/v0.9.0)             |
| eSignet                     | [v1.4.1](https://github.com/mosip/esignet/tree/v1.4.1)                         |
| Resident Portal - UI        | [v0.9.0](https://github.com/mosip/resident-ui/releases/tag/v0.9.0)             |
| Registration Client         | [v1.2.0.2](https://github.com/mosip/registration-client/tree/v1.2.0.2)         |
| mosip-automation-tests      | [v1.3.0](https://github.com/mosip/mosip-automation-tests/tree/release-1.3.x)   |

## Dependency Matrix

| Component                                                | Version                                                    | Helm Chart Version (If applicable) |
| -------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------- |
| Keycloak                                                 | v7.1.18                                                    | NA                                 |
| <p>Kafka</p><p>Kafka-zookeeper</p>                       | 3.2.1 ( Both components deployed with the same helm chart) | <p>3.8.0</p><p>18.3.1</p>          |
| Postgres                                                 | v16                                                        | 13.1.5                             |
| Mock-SMTP                                                | 1.0.0                                                      | 1.0.0                              |
| ActiveMQ                                                 | 2.39.0                                                     | 0.0.3                              |
| Minio                                                    | 2025.2.28-debian-12-r1                                     | 15.0.6                             |
| Redis (used only if eSignet exists)                      | v7.0.5                                                     | 17.3.14                            |
| Postgres                                                 | v16                                                        | 13.1.5                             |
| Softhsm (Recommended to use only in sandbox environment) | v2                                                         | 12.0.1                             |
| clamav                                                   | v1.2                                                       | 3.1.0                              |
