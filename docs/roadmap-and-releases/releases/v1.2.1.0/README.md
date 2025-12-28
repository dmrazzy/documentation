# v1.2.1.0

**Release Version:** v1.2.1.0

**Release Type:** Major Release

**Release Date:** Coming Soon

### **Overview**

The MOSIP v1.2.1.0 release marks a major platform advancement, delivering a **stable and production-ready migration from Java 11 to Java 21**. This transition brings significant improvements in performance, security, and long-term maintainability. The release also consolidates all enhancements introduced during the beta cycle and introduces several key updates across security, automation, data handling, and developer tooling.

Additionally, this version includes a **Resource Calculator** built from extensive performance testing, to help implementers size their MOSIP deployments accurately and efficiently, You can find the **Resource Calculator** on [**Server Hardware Requirements**](../../../setup/deploymentnew/getting-started/production/server-hardware-requirements.md) page.

### **Major Areas of Work**

* **Stable Migration to Java 21** - Ensures a reliable upgrade to Java 21 for improved performance, security, and long-term support.
* **Digital Signature with ECC in Key Manager** - Enables secure and efficient ECC-based digital signing within the Key Manager.
* **Data Segregation Enhancements (Default, Seed, Test)** - Strengthens environment separation by organizing data into Default, Seed, and Test categories.
* **Performance Enhancements & Resource Calculator** - Boosts system performance with optimized operations and an improved resource estimation tool.
* **DSL Improvements & Automation Updates** - Enhances DSL capabilities and updates automation workflows for smoother execution.
* **API Automation Fixes** - Resolves issues in API automation to ensure consistent, reliable automated testing.
* **Functional Bug Fixes** - Addresses functional defects to improve stability and user experience.
* **Security Bug Fixes** - Patches security vulnerabilities to enhance platform protection and compliance.

### Stories Released

<table><thead><tr><th width="163.1875">Feature</th><th width="431.9921875">Description</th><th>JIRA</th></tr></thead><tbody><tr><td>did:web Support in Key Manager</td><td>Enabled did:web support in Key Manager to allow VC generation in SD-JWT format.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-41756">MOSIP-41756</a></td></tr><tr><td>PDF Generation Module Replacement</td><td>Replaces the deprecated kernel-pdfgenerator-itext component with a new, streamlined PDF generation implementation to improve maintainability, licensing compliance, and long-term support.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-41360">MOSIP-41360</a></td></tr><tr><td>Data Storage Enablement via Updated Durian APIs</td><td>Updates the existing Durian APIs to enable seamless integration with Inji Web, allowing efficient and secure data storage and retrieval within the platform.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-34253">MOSIP-34253</a></td></tr><tr><td>ECC Key Algorithm Support</td><td>Support ECC algorithm for Signing and Verification in KeyManager Service.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-33780">MOSIP-33780</a></td></tr><tr><td>Post-Migration Security Testing for Java 21</td><td>Conducts comprehensive security testing on the Platform module following its migration to Java 21, ensuring that the upgrade maintains compliance, mitigates vulnerabilities, and strengthens overall system security.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-32918">MOSIP-32918</a></td></tr><tr><td>Image Color Space HEX Value Compatibility Support</td><td>Enables support for both ISO-compliant Image Color Space values—0x00 (unspecified) and 0x01—in the ISO header to ensure seamless processing across implementations, including compatibility with Mock BioSDK and SBI-based integrations.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-32546">MOSIP-32546</a></td></tr><tr><td>PDF Generator Module Replacement</td><td>Removes the deprecated kernel-pdfgenerator-itext component and introduces a new PDF generation implementation to improve maintainability, compliance, and long-term platform stability.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-27719">MOSIP-27719</a></td></tr><tr><td>Default, Seed, and Test Data Sets for MOSIP Deployment</td><td>Provides essential, optional, and test data sets to enable ready-to-use, functional, and testable MOSIP deployments.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-26804">MOSIP-26804</a></td></tr><tr><td>Java 21 and Spring Boot 3.2.3 Upgrade</td><td>Upgrades the platform to Java 21 and Spring Boot 3.2.3, enhancing performance, security, and compatibility with modern frameworks.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-21117">MOSIP-21117</a></td></tr><tr><td>MOSIP Logo updated</td><td>Replace the old MOSIP logo with the new logo across all UI pages in all environment builds, starting with Collab.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-42060">MOSIP-42060</a></td></tr></tbody></table>

### Bug Fixes

Below are a few key bugs addressed in this release. Please click [here](https://mosip.atlassian.net/jira/software/c/projects/MOSIP/issues/?jql=project%20%3D%20MOSIP%20AND%20fixVersion%20%3D%20PLA_1.2.1.0%20AND%20type%20%3D%20Bug%20ORDER%20BY%20created%20DESC\&selectedIssue=MOSIP-43203) to view the complete list of fixes.

<table><thead><tr><th width="179.265625">JIRA</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-43188">MOSIP-43188</a></td><td>ADMIN-SERVICE: Longitude value is assigning to the setLongCode In kernel syncdata service.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-42697">MOSIP-42697</a></td><td>Packet Manager : Info cache causing issue while processing the BIOMETRIC_CORRECTION packet.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-42537">MOSIP-42537</a></td><td>Incorrect x5c Field in JWT Header – Certificate Passed Instead of Full Trust Chain.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-42320">MOSIP-42320</a></td><td>PSUT Token Generation Mismatch: Different PSUT for VID Compared to UIN and Handle.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41926">MOSIP-41926</a></td><td>Unable to upload packets from admin-bulkupload.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41917">MOSIP-41917</a></td><td>Reg client update from child to Adult - is not updating IDRepo (critical).</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41670">MOSIP-41670</a></td><td>regclient-We are unable to generate a new UIN packet as the process is stuck at the 'INTERNAL_WORKFLOW_ACTION' loading stage.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41497">MOSIP-41497</a></td><td>Unable to access Reg.Client because of a sync configuration failure.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41493">MOSIP-41493</a></td><td>Regproc: Packets are failing at Biographic verification stage.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41276">MOSIP-41276</a></td><td>Packets with invalid signatures are reprocessed instead of failing.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41251">MOSIP-41251</a></td><td>Regenerate Vid is failing with "Could not generate/regenerate VID as per policy" error.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41248">MOSIP-41248</a></td><td>NFIQ 1.0 fix reliability issues.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41188">MOSIP-41188</a></td><td>Draft extract biometric scenarios with only face ,only finger, only iris and with all together( ie, face,finger and iris) are failing.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-40984">MOSIP-40984</a></td><td>Exception is thrown while updating handle field in Update UIN flow.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-40959">MOSIP-40959</a></td><td>Add new column to the ca_cert_store.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-39903">MOSIP-39903</a></td><td>IDA: Upon marking an exception in any bioSubType, authentication for the entire corresponding bioType is not working.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-39717">MOSIP-39717</a></td><td>[Masterdata] Not getting proper error code for get applicant type api with invalid biometricAvailable.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-38915">MOSIP-38915</a></td><td>Security testing: Registration processor- header issues.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-38005">MOSIP-38005</a></td><td>Unable to Authenticate due to selected Handles.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-37463">MOSIP-37463</a></td><td>"Missing Input Parameter - identity/IDSchemaVersion" exception in FINALIZATION stage.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-37363">MOSIP-37363</a></td><td>During Demographic Authentication negative value for age is allowed to be authenticated.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-36529">MOSIP-36259</a></td><td>Prereg- On the login page, when switching the language to Hindi, Tamil, or Kannada, the alignment is displayed from right to left.</td></tr></tbody></table>

### Repositories Released

| Repositories         | Tags                                                                |
| -------------------- | ------------------------------------------------------------------- |
| admin-ui             | [v1.3.0](https://github.com/mosip/admin-ui/tree/v1.3.0)             |
| artifactory-ref-impl | [v1.3.0](https://github.com/mosip/artifactory-ref-impl/tree/v1.3.0) |
| admin-services       | [v1.3.0](https://github.com/mosip/admin-services/tree/v1.3.0)       |
| audit-manager        | [v1.3.0](https://github.com/mosip/audit-manager/tree/v1.3.0)        |
| biosdk-client        | [v1.3.0](https://github.com/mosip/biosdk-client/tree/v1.3.0)        |
| commons              | [v1.3.0](https://github.com/mosip/commons/tree/v1.3.0)              |
| id-authentication    | [v1.3.0](https://github.com/mosip/id-authentication/tree/v1.3.0)    |
| id-repository        | [v1.3.0](https://github.com/mosip/id-repository/tree/v1.3.0)        |
| otp-manager          | [v1.3.0](https://github.com/mosip/otp-manager/tree/v1.3.0)          |
| packet-manager       | [v1.3.0](https://github.com/mosip/packet-manager/tree/v1.3.0)       |
| demosdk              | [v1.3.0](https://github.com/mosip/demosdk/tree/v1.3.0)              |
| durian               | [v1.3.0](https://github.com/mosip/durian/tree/v1.3.0)               |
| keymanager           | [v1.3.0](https://github.com/mosip/keymanager/tree/v1.3.0)           |
| mosip-config         | [v1.3.0](https://github.com/mosip/mosip-config/tree/v1.3.0)         |
| admin-ui             | [v1.3.0](https://github.com/mosip/admin-ui/tree/v1.3.0)             |
| captcha              | [v1.3.0](https://github.com/mosip/captcha/tree/v0.1.0)              |
| registration         | [v1.3.0](https://github.com/mosip/registration/tree/v1.3.0)         |
| digital-card-service | [v1.3.0](https://github.com/mosip/digital-card-service/tree/v1.3.0) |
| khazana              | [v1.3.0](https://github.com/mosip/khazana/tree/v1.3.0)              |
| mosip-openid-bridge  | [v1.3.0](https://github.com/mosip/mosip-openid-bridge/tree/v1.3.0)  |
| mosip-ref-impl       | [v1.3.0](https://github.com/mosip/mosip-ref-impl/tree/v1.3.0)       |
| print                | [v1.3.0](https://github.com/mosip/print/tree/v1.3.0)                |
| resident-services    | [v1.3.0](https://github.com/mosip/resident-services/tree/v1.3.0)    |
| websub               | [v1.3.0](https://github.com/mosip/websub/tree/v1.3.0)               |
| mosip-data           | [v1.3.0](https://github.com/mosip/mosip-data/tree/v1.3.0)           |
| k8s-infra            | [v1.2.1.1](https://github.com/mosip/k8s-infra/tree/v1.2.1.1)        |
| keycloak             | [v1.3.0](https://github.com/mosip/keycloak/tree/v1.3.0)             |
| mosip-file-server    | [v1.3.0](https://github.com/mosip/mosip-file-server/tree/v1.3.0)    |
| mosip-helm           | [v1.3.0](https://github.com/mosip/mosip-helm/tree/v1.3.0)           |
| mosip-infra          | [v1.2.1.0](https://github.com/mosip/mosip-infra/tree/v1.2.1.0)      |
| postgres-init        | [v1.3.0](https://github.com/mosip/postgres-init/tree/v1.3.0)        |
| bio-utils            | [v1.3.0](https://github.com/mosip/bio-utils/tree/v1.3.0)            |
| biosdk-client        | [v1.3.0](https://github.com/mosip/biosdk-client/tree/v1.3.0)        |
| biosdk-services      | [v1.3.0](https://github.com/mosip/biosdk-services/tree/v1.3.0)      |
| converters           | [v1.3.0](https://github.com/mosip/converters/tree/v1.3.0)           |
| demosdk              | [v1.3.0](https://github.com/mosip/demosdk/tree/v1.3.0)              |
| id-repository        | [v1.3.0](https://github.com/mosip/id-repository/tree/v1.3.0)        |
| mosip-mock-services  | [v1.3.0](https://github.com/mosip/mosip-mock-services/tree/v1.3.0)  |
| nfiq                 | [v0.1.0](https://github.com/mosip/nfiq/tree/v0.1.0)                 |
| pre-registration     | [v1.3.0](https://github.com/mosip/pre-registration/tree/v1.3.0)     |
| pre-registration-ui  | [v1.3.0](https://github.com/mosip/pre-registration-ui/tree/v1.3.0)  |

### Known Issues

Below are a few key bugs marked as known issues for this release. Please click [here](https://mosip.atlassian.net/jira/software/c/projects/MOSIP/issues/?jql=project%20%3D%20MOSIP%20AND%20type%20%3D%20Bug%20AND%20labels%20%3D%20known_issue%20AND%20affectedVersion%20%3D%20PLA_1.2.1.0%20ORDER%20BY%20created%20DESC\&selectedIssue=MOSIP-43976) to view the complete list of fixes.

<table><thead><tr><th width="195.703125">JIRA</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-43976">MOSIP-43976</a></td><td>Pre registration - Backward compatibility not supported</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-43817">MOSIP-43817</a></td><td>Pre-registration - Failing automation test scenario in qajava21 environment</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-43752">MOSIP-43752</a></td><td>Admin - Unable to login into Admin UI after we logout</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-43468">MOSIP-43468</a></td><td>Unable to decommission machine – Error: “Machine cannot be decommissioned as some Registration centers are mapped”</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-43456">MOSIP-43456</a></td><td>IDA server side issues</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41698">MOSIP-41698</a></td><td>Packet Not Rejected Despite Manual Verification Status Set to REJECTED</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41694">MOSIP-41694</a></td><td>DSL-Scenario-108 Child Packet Should Be Rejected Only If Parent Packet is Rejected – Not Processed in DSL</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-41021">MOSIP-41021</a></td><td>OTP is being sent to blocked, deactivated, and locked channels for UIN/VID.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-35360">MOSIP-35360</a></td><td>Able to update biometrics of an UIN, with unregistered biometrics</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-35045">MOSIP-35045</a></td><td>Admin UI: Not getting all machine type in dropdown while creating new machine spec.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-31248">MOSIP-31248</a></td><td>Websub issue on sending messages to subscribers on startup</td></tr></tbody></table>

### Compatibility Matrix

| Module                     | Version                                                                        |
| -------------------------- | ------------------------------------------------------------------------------ |
| Partner Management Service | [v1.2.2.2](https://github.com/mosip/partner-management-services/tree/v1.2.2.2) |
| eSignet                    | [v1.4.1](https://github.com/mosip/esignet/tree/v1.4.1)                         |
| Resident Portal - UI       | [v0.9.0](https://github.com/mosip/resident-ui/releases/tag/v0.9.0)             |
| Registration Client        | [v1.2.0.2](https://github.com/mosip/registration-client/tree/v1.2.0.2)         |
| mosip-automation-tests     | [v1.3.0](https://github.com/mosip/mosip-automation-tests/tree/release-1.3.x)   |

## Dependency Matrix

| Component                                                | Version                                                                       | Helm Chart version (If applicable) |
| -------------------------------------------------------- | ----------------------------------------------------------------------------- | ---------------------------------- |
| Key Cloak                                                | v7.1.18                                                                       |                                    |
| <p>Kafka</p><p>Kafka-zookeeper</p>                       | <p>3.2.1 ( Both components deployed with the same helm chart)</p><p>3.8.0</p> | 18.3.1                             |
| Mock-SMTP                                                | 1.0.0                                                                         | 1.0.0                              |
| ActiveMQ                                                 | 2.39.0                                                                        | 0.0.3                              |
| Minio                                                    | 2025.2.28-debian-12-r1                                                        | 15.0.6                             |
| Redis (used only if eSignet exists)                      | v7.0.5                                                                        | 17.3.14                            |
| Postgres                                                 | v16                                                                           | 13.1.5                             |
| Softhsm (Recommended to use only in sandbox environment) | v2                                                                            | 12.0.1                             |
| clamav                                                   | v1.2                                                                          | 3.1.0                              |



***

### Learn More

* [Feature Documentation](https://docs.mosip.io/1.2.0/)
* **Documents published/revised with the release**:
  * [Versioning Policy](../versioning-policy.md)
  * [Upgrade Runbook 1.2.1.0](../../../setup/upgrade/upgrade-runbook-1.2.1.0.md)
  * [Hardware Requirements](../../../setup/deploymentnew/getting-started/production/server-hardware-requirements.md)
  * [Adapting Changes in Administration Roles](../../../setup/upgrade/upgrade-runbook/mock-services/upgrade-admin-services-roles-guide.md)
* **Reports**:
  * [QA Report](test-report.md)
  * [IDA Performance Report](ida-performance-report.md)
  * [Reg Proc and SyncData Performance Report](reg-proc-and-syncdata-performance-report.md)

