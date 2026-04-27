# Test Report

### Introduction

The scope of testing is to verify fitment to the specification from the perspective of&#x20;

* Update Biometric feature - MOSIP-34112
* IDRepo - Handles with special characters - MOSIP-43481

### Overview and Scope

Verification is performed not only from the end user perspective but also from the System Integrator (SI) point of view. Hence Configurability and Extensibility of the software is also assessed. This ensures readiness of software for use in multiple countries. Since MOSIP is an “API First” product platform, Verification scope required comprehensive automation testing for all the MOSIP APIs. An automation Test Rig is created for the same.

## Test Approach

The verification methods may differ based on how the need was addressed.

MOSIP Test Rig covers the end to end test execution and reporting. The end to end functional test scenarios are written starting from pre-registration, to creation of packet in registration center, processing the packet through the registration processor, generating UIN and authenticating identity using IDA through various permutation and combinations of cases being covered. MOSIP Test Rig will be an open source artifact which can also be enhanced and used by countries to validate the SI deliveries before going live.

Reference docs:

[https://mosip.atlassian.net/wiki/x/CoDmWw](https://mosip.atlassian.net/wiki/x/CoDmWw)

## Test Organization <a href="#toc227764331" id="toc227764331"></a>

This part lists the team members involved in the testing process and their responsibilities. It clarifies who is accountable for which roles.

Table: Test Organization

<table><thead><tr><th width="177.046875" valign="top">Name</th><th width="215.26171875" valign="top">Functional Role</th><th valign="top">Responsibilities</th></tr></thead><tbody><tr><td valign="top">Ragini Krishna</td><td valign="top">Manager</td><td valign="top">Defining test strategy, managing QA activities, and ensuring overall product quality.</td></tr><tr><td valign="top">N. Chandra Sekhar</td><td valign="top">Lead/ Test engineer</td><td valign="top">Developing and executing test cases, logging defects, and verifying deployment, functional and automation testing.</td></tr></tbody></table>

## Test Planning <a href="#toc17829895" id="toc17829895"></a>

### Functional test results

Below are the test metrics by performing functional testing using mock MDS, mock Auth and mock ABIS. The process followed was black box testing which based its test cases on the specifications of the software component under test. Functional test was performed in combination of individual module testing as well as integration testing. Test data were prepared in line with the user stories. Expected results were monitored by examining the user interface. The coverage includes GUI testing, System testing, End-To-End flows across multiple languages and configurations. The testing cycle included simulation of multiple identity schema and respective UI schema configurations.

The Test Planning section outlines the strategy and activities planned for executing the testing process to ensure comprehensive coverage.

### Test Environment

This section details the specific environments used for testing. It lists the different platforms, and the versions of the images used.

Table 2: Test Environments

## Docker versions in qa11 environment

<table data-header-hidden><thead><tr><th valign="bottom"></th></tr></thead><tbody><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/admin-service:1.2.1.4</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/admin-ui:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/admintest:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/apitest-auth:1.2.1.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/apitest-masterdata:1.2.1.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/apitest-pms:1.2.2.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/apitest-prereg:1.2.0.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/apitest-resident:1.2.1.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/artifactory-server:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/artifactory-server:1.2.0.4</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/authentication-internal-service:1.2.1.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/authentication-otp-service:1.2.1.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/authentication-service:1.2.1.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/biosdk-server:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/captcha-validation-service:0.1.0</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/clamav:1.3.0_base</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/commons-packet-service:1.2.0.4</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/config-server:1.1.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/consolidator-websub-service:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/data-share-service:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/digital-card-service:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/dsl-orchestrator:1.2.2.0</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/dsl-packetcreator:1.2.2.0</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/esignet:1.4.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/hotlist-service:1.2.1.4</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/id-repository-salt-generator:1.2.2.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/kafka:3.2.1-debian-11-r9</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/kernel-auditmanager-service:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/kernel-auth-service:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/kernel-idgenerator-service:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/kernel-keymanager-service:1.2.1.0</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/kernel-masterdata-service:1.2.1.4</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/kernel-notification-service:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/kernel-otpmanager-service:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/kernel-pridgenerator-service:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/kernel-ridgenerator-service:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/kernel-syncdata-service:1.2.1.4</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/keycloak-init:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/keys-generator:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/kibana:7.17.2-debian-10-r0</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/masterdata-loader:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/minio-client-util</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/minio-client-util:latest</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/minio:2022.2.7-debian-10-r0</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/mock-abis:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/mock-identity-system:0.9.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/mock-mv:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/mock-relying-party-service:0.9.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/mock-relying-party-ui:0.9.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/mock-smtp:1.0.0</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/mosip-artemis-keycloak:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/mosip-file-server:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/oidc-ui:1.4.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/os-shell:12-debian-12-r46</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/partner-management-service:1.2.2.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/partner-onboarder:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/pmp-revamp-ui:1.2.2.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/pmptest:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/policy-management-service:1.2.2.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/postgres-init:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/postgres-init:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/postgresql:14.2.0-debian-10-r70</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/pre-registration-application-service:1.2.0.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/pre-registration-batchjob:1.2.0.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/pre-registration-booking-service:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/pre-registration-captcha-service:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/pre-registration-datasync-service:1.2.0.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/pre-registration-ui:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/print:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/redis:7.0.5-debian-11-r25</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/regclient-keystore:1.0.0</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/registration-client:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/resident-service:1.2.1.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/resident-ui:0.9.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/softhsm:v2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/websub-service:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipid/zookeeper:3.8.0-debian-11-r30</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipint/elasticsearch:7.17.2-debian-10-r4</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/activemq-artemis:1.1.5</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/credential-request-generator:1.2.3.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/credential-service:1.2.3.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/id-repository-identity-service:1.2.3.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/id-repository-vid-service:1.2.3.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-common-camel-bridge:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-dmz-packet-server:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-landing-zone:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-notification-service:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-registration-status-service:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-registration-transaction-service:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-reprocessor:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-stage-group-1:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-stage-group-2:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-stage-group-3:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-stage-group-4:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-stage-group-5:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-stage-group-6:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-stage-group-7:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>docker.io/mosipqa/registration-processor-workflow-manager-service:1.2.1.x</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/apitest-auth:1.2.1.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/apitest-masterdata:1.2.1.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/apitest-pms:1.2.2.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/apitest-prereg:1.2.0.3</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/apitest-resident:1.2.1.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/clamav:1.3.0_base</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/config-server:1.1.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/keycloak-init:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/masterdata-loader:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/postgres-init:1.2.0.1</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/postgres-init:1.2.0.2</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/regclient-keystore:1.0.0</p><p> </p></td></tr><tr><td valign="bottom"><p> </p><p>mosipid/softhsm:v2</p><p> </p></td></tr></tbody></table>

## Main features tested

Manual verification of RegProc and IDRepo modules.

* Update Biometric feature - MOSIP-34112
* IDRepo - Handles with special characters - MOSIP-43481

### Out of Scope

* Docker compose testing
* Deployment testing
* Real device testing
* Upgrade testing

### Functional test results <a href="#toc184990131" id="toc184990131"></a>

Below are the test metrics by performing functional testing using mock MDS, mock Auth and mock ABIS. The process followed was black box testing which based its test cases on the specifications of the software component under test. Functional test was performed in combination of individual module testing as well as integration testing. Test data were prepared in line with the user stories. Expected results were monitored by examining the user interface. The coverage includes GUI testing, System testing, End-To-End flows across multiple languages and configurations. The testing cycle included simulation of multiple identity schema and respective UI schema configurations.

#### API Based Testing

| Env Name: qa11.mosip.net                    |                            |                            |                          |                          |                           |                           |
| ------------------------------------------- | -------------------------- | -------------------------- | ------------------------ | ------------------------ | ------------------------- | ------------------------- |
| Module                                      | Total                      | Pass                       | Skip                     | Fail                     | Ignored                   | Known Issues              |
| <p> </p><p>IDRepo - API Testrig</p><p> </p> | <p> </p><p>414</p><p> </p> | <p> </p><p>316</p><p> </p> | <p> </p><p>0</p><p> </p> | <p> </p><p>0</p><p> </p> | <p> </p><p>78</p><p> </p> | <p> </p><p>20</p><p> </p> |
| <p> </p><p>DSL</p><p> </p>                  | <p> </p><p>230</p><p> </p> | <p> </p><p>193</p><p> </p> | <p> </p><p>0</p><p> </p> | <p> </p><p>0</p><p> </p> | <p> </p><p>0</p><p> </p>  | <p> </p><p>37</p><p> </p> |

Detailed Test metrics T-230\_P-193\_KI-37\_I-0\_S-0\_F-0

Below are the detailed test metrics by performing manual/automation testing. The project metrics are derived from Defect density, Test coverage, Test execution coverage, test tracking and efficiency.

The various metrics that assist in test tracking and efficiency are as follows:

* Passed Test Cases Coverage: It measures the percentage of passed test cases. (Number of passed tests / Total number of tests executed) x 100
* Failed Test Case Coverage: It measures the percentage of all the failed test cases. (Number of failed tests / Total number of test cases executed) x 100

## Conclusion

* Functional testing was completed in accordance with the Approach for Update Biometric Flow document. The basic functionality of all modules is working as expected in the qa11.mosip.net environment.
* QA Approved build release.

## QA Approval

Build has met the defined exit criteria and is recommended for release.

* Functional testing of RegProc and ID Repo modules.
* Automation reports -API and DSL
* Documentation Sign-off
* Test Environment Stability

Table 8: Report is signed off details

<table><thead><tr><th valign="top">Name</th><th valign="top">Functional Role</th><th valign="top">Responsibilities</th></tr></thead><tbody><tr><td valign="top">Ragini Krishna</td><td valign="top">Manager</td><td valign="top">Defining test strategy, managing QA activities, and ensuring overall product quality.</td></tr><tr><td valign="top">Chandra Sekhar</td><td valign="top">Lead/ Test Engineer</td><td valign="top">Leading the test team, planning and executing tests, and ensuring timely delivery of quality results.</td></tr></tbody></table>

## Appendix

This includes additional reference information for the report. It contains a history of document versions and a list of acronyms and their meanings.

### Appendix A: Versions

<table><thead><tr><th>Version</th><th>Date</th><th>Author</th><th valign="top">Reviewers</th></tr></thead><tbody><tr><td>V1.0</td><td>021/04/2026</td><td>N. Chandra Sekhar</td><td valign="top">Ragini Krishna</td></tr></tbody></table>

### Appendix B: Acronyms

<table><thead><tr><th valign="top">Acronym</th><th valign="top">Literal Translation</th></tr></thead><tbody><tr><td valign="top"><p>Reg.Client</p><p>PreReg</p><p>ABIS</p><p>MV</p></td><td valign="top"><p>Registration Client</p><p>Pre-Registration</p><p>Automated Biometric Identification System</p><p>Manual Verification</p></td></tr></tbody></table>

### Document History

It outlines the strategy used to ensure a comprehensive evaluation.

<table><thead><tr><th>Version</th><th>Author</th><th>Date</th><th valign="top">Review</th><th valign="top">Affected Sections</th></tr></thead><tbody><tr><td>V1.0</td><td>N. Chandra Sekhar</td><td>21/04/2026</td><td valign="top">Ragini Krishna</td><td valign="top"> </td></tr></tbody></table>

&#x20;



