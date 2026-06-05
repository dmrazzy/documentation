# Test Report

## Introduction <a href="#toc230882377" id="toc230882377"></a>

The scope of testing is to verify fitment to the specification from the perspective of

* Performance Fine tuning: MOSIP-42620: Enhancing the performance of MOSIP Platform J21

### Overview and Scope <a href="#toc230882378" id="toc230882378"></a>

Verification is performed not only from the end user perspective but also from the System Integrator (SI) point of view. Hence Configurability and Extensibility of the software is also assessed. This ensures readiness of software for use in multiple countries. Since MOSIP is an “API First” product platform, Verification scope required comprehensive automation testing for all the MOSIP APIs. An automation Test Rig is created for the same.

## Test Approach <a href="#toc230882379" id="toc230882379"></a>

The verification methods may differ based on how the need was addressed.

MOSIP Test Rig covers the end to end test execution and reporting. The end to end functional test scenarios are written starting from pre-registration, to creation of packet in registration center, processing the packet through the registration processor, generating UIN and authenticating identity using IDA through various permutation and combinations of cases being covered. MOSIP Test Rig will be an open source artifact which can also be enhanced and used by countries to validate the SI deliveries before going live.

Reference docs/jira:

[https://github.com/mosip/registration/issues/2283](https://github.com/mosip/registration/issues/2283)

[https://mosip.atlassian.net/browse/MOSIP-42620](https://mosip.atlassian.net/browse/MOSIP-42620)

## Test Organization <a href="#toc17829893" id="toc17829893"></a>

This part lists the team members involved in the testing process and their responsibilities. It clarifies who is accountable for which roles.

**Table 1**: Test Organization

| Name              | Functional Role     | Responsibilities                                                                                                   |
| ----------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Ragini Krishna    | Manager             | Defining test strategy, managing QA activities, and ensuring overall product quality.                              |
| N. Chandra Sekhar | Lead/ Test engineer | Developing and executing test cases, logging defects, and verifying deployment, functional and automation testing. |

## Test Planning <a href="#toc17829895" id="toc17829895"></a>

### Functional test results <a href="#toc230882382" id="toc230882382"></a>

Below are the test metrics by performing functional testing using mock MDS, mock Auth and mock ABIS. The process followed was black box testing which based its test cases on the specifications of the software component under test. Functional test was performed in combination of individual module testing as well as integration testing. Test data were prepared in line with the user stories. Expected results were monitored by examining the user interface. The coverage includes GUI testing, System testing, End-To-End flows across multiple languages and configurations. The testing cycle included simulation of multiple identity schema and respective UI schema configurations.

The Test Planning section outlines the strategy and activities planned for executing the testing process to ensure comprehensive coverage.

### Test Environment <a href="#toc230882383" id="toc230882383"></a>

This section details the specific environments used for testing. It lists the different platforms, and the versions of the images used.

**Table 2**: Test Environments

| Environment     | Details            |
| --------------- | ------------------ |
| qaj21.mosip.net | Java21 environment |

## Docker versions in qaj21 environment <a href="#toc230882384" id="toc230882384"></a>

<table data-header-hidden><thead><tr><th valign="bottom"></th></tr></thead><tbody><tr><td valign="bottom">mosipid/apitest-masterdata:1.3.0</td></tr><tr><td valign="bottom">mosipid/apitest-pms:1.2.2.2</td></tr><tr><td valign="bottom">mosipid/apitest-prereg:1.3.0</td></tr><tr><td valign="bottom">mosipid/apitest-resident:1.3.0</td></tr><tr><td valign="bottom">mosipid/clamav:1.3.0_base</td></tr><tr><td valign="bottom">mosipid/keycloak-init:1.2.0.2</td></tr><tr><td valign="bottom">mosipid/keycloak-init:1.3.0</td></tr><tr><td valign="bottom">mosipid/masterdata-loader:1.3.0</td></tr><tr><td valign="bottom">mosipid/postgres-init:1.2.0.1</td></tr><tr><td valign="bottom">mosipid/postgres-init:1.3.0</td></tr><tr><td valign="bottom">mosipid/regclient-keystore:1.0.0</td></tr><tr><td valign="bottom">mosipid/softhsm:v2</td></tr><tr><td valign="bottom">mosipqa/apitest-auth:1.3.x</td></tr><tr><td valign="bottom">mosipqa/apitest-idrepo:1.3.x</td></tr><tr><td valign="bottom">mosipqa/kernel-config-server:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipid/activemq-artemis:2.39.0</td></tr><tr><td valign="bottom">docker.io/mosipid/admin-service:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/admin-ui:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/apitest-masterdata:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/apitest-pms:1.2.2.2</td></tr><tr><td valign="bottom">docker.io/mosipid/apitest-prereg:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/apitest-resident:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/artifactory-server:1.2.0.2</td></tr><tr><td valign="bottom">docker.io/mosipid/captcha-validation-service:0.1.0</td></tr><tr><td valign="bottom">docker.io/mosipid/clamav:1.3.0_base</td></tr><tr><td valign="bottom">docker.io/mosipid/digital-card-service:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/dsl-orchestrator:1.4.0</td></tr><tr><td valign="bottom">docker.io/mosipid/dsl-packetcreator:1.4.0</td></tr><tr><td valign="bottom">docker.io/mosipid/esignet:1.4.1</td></tr><tr><td valign="bottom">docker.io/mosipid/hotlist-service:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/id-repository-salt-generator:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/kafka:3.2.1-debian-11-r9</td></tr><tr><td valign="bottom">docker.io/mosipid/kernel-auth-service:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/kernel-masterdata-service:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/kernel-otpmanager-service:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/kernel-salt-generator:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/kernel-syncdata-service:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/keycloak-init:1.2.0.2</td></tr><tr><td valign="bottom">docker.io/mosipid/keycloak-init:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/keys-generator:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/keys-generator:1.4.0</td></tr><tr><td valign="bottom">docker.io/mosipid/kibana:7.17.2-debian-10-r0</td></tr><tr><td valign="bottom">docker.io/mosipid/masterdata-loader:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/minio:2025.2.28-debian-12-r1</td></tr><tr><td valign="bottom">docker.io/mosipid/mock-relying-party-service:0.9.3</td></tr><tr><td valign="bottom">docker.io/mosipid/mock-relying-party-ui:0.9.3</td></tr><tr><td valign="bottom">docker.io/mosipid/mock-smtp:1.0.0</td></tr><tr><td valign="bottom">docker.io/mosipid/mosip-artemis-keycloak:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/mosip-file-server:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/oidc-ui:1.4.1</td></tr><tr><td valign="bottom">docker.io/mosipid/os-shell:12-debian-12-r46</td></tr><tr><td valign="bottom">docker.io/mosipid/partner-management-service:1.2.2.3</td></tr><tr><td valign="bottom">docker.io/mosipid/partner-onboarder:1.2.0.1</td></tr><tr><td valign="bottom">docker.io/mosipid/partner-onboarder:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/pmp-revamp-ui:1.2.2.2</td></tr><tr><td valign="bottom">docker.io/mosipid/policy-management-service:1.2.2.3</td></tr><tr><td valign="bottom">docker.io/mosipid/postgres-init:1.2.0.1</td></tr><tr><td valign="bottom">docker.io/mosipid/postgres-init:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/postgresql:14.2.0-debian-10-r70</td></tr><tr><td valign="bottom">docker.io/mosipid/pre-registration-application-service:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/pre-registration-batchjob:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/pre-registration-datasync-service:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/pre-registration-ui:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/print:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/redis:7.0.5-debian-11-r25</td></tr><tr><td valign="bottom">docker.io/mosipid/regclient-keystore:1.0.0</td></tr><tr><td valign="bottom">docker.io/mosipid/registration-client:1.2.0.2</td></tr><tr><td valign="bottom">docker.io/mosipid/resident-service:1.3.0</td></tr><tr><td valign="bottom">docker.io/mosipid/resident-ui:0.9.1</td></tr><tr><td valign="bottom">docker.io/mosipid/softhsm:v2</td></tr><tr><td valign="bottom">docker.io/mosipid/zookeeper:3.8.0-debian-11-r30</td></tr><tr><td valign="bottom">docker.io/mosipint/elasticsearch:7.17.2-debian-10-r4</td></tr><tr><td valign="bottom">docker.io/mosipqa/artifactory-server:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/authentication-internal-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/authentication-otp-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/authentication-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/biosdk-server:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/commons-packet-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/consolidator-websub-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/credential-request-generator:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/credential-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/data-share-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/id-repository-identity-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/id-repository-vid-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/kernel-auditmanager-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/kernel-config-server:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/kernel-idgenerator-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/kernel-keymanager-service:1.4.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/kernel-notification-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/kernel-pridgenerator-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/kernel-ridgenerator-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/mock-abis:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/mock-mv:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/pre-registration-booking-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-common-camel-bridge:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-dmz-packet-server:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-landing-zone:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-notification-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-registration-status-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-registration-transaction-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-reprocessor:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-stage-group-1:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-stage-group-2:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-stage-group-3:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-stage-group-4:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-stage-group-5:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-stage-group-6:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-stage-group-7:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/registration-processor-workflow-manager-service:1.3.x</td></tr><tr><td valign="bottom">docker.io/mosipqa/websub-service:1.3.x</td></tr><tr><td valign="bottom">mosipqa/kernel-config-server:1.3.x</td></tr><tr><td valign="bottom">mosipid/clamav:1.3.0_base</td></tr><tr><td valign="bottom">mosipid/keycloak-init:1.2.0.2</td></tr><tr><td valign="bottom">mosipid/keycloak-init:1.3.0</td></tr><tr><td valign="bottom">mosipid/masterdata-loader:1.3.0</td></tr><tr><td valign="bottom">mosipid/postgres-init:1.2.0.1</td></tr><tr><td valign="bottom">mosipid/postgres-init:1.3.0</td></tr><tr><td valign="bottom">mosipid/regclient-keystore:1.0.0</td></tr><tr><td valign="bottom">mosipid/softhsm:v2</td></tr></tbody></table>

## Main features tested: <a href="#toc230882385" id="toc230882385"></a>

Manual verification of RegProc packet processing, IDA and IDRepo modules.

## Out of Scope: <a href="#toc230882386" id="toc230882386"></a>

* Docker compose testing
* Deployment testing
* Real device testing
* Upgrade testing

### Functional test results <a href="#toc184990131" id="toc184990131"></a>

Below are the test metrics by performing functional testing using mock MDS, mock Auth and mock ABIS. The process followed was black box testing which based its test cases on the specifications of the software component under test. Functional test was performed in combination of individual module testing as well as integration testing. Test data were prepared in line with the user stories. Expected results were monitored by examining the user interface. The coverage includes GUI testing, System testing, End-To-End flows across multiple languages and configurations. The testing cycle included simulation of multiple identity schema and respective UI schema configurations.

API Based Testing

| Env Name: qaj21.mosip.net |       |      |      |      |         |              |
| ------------------------- | ----- | ---- | ---- | ---- | ------- | ------------ |
| Module                    | Total | Pass | Skip | Fail | Ignored | Known Issues |
| IDRepo - API Testrig      | 414   | 336  | 0    | 0    | 78      | 0            |
| IDA - API Testrig         | 612   | 598  | 0    | 0    | 5       | 9            |
| DSL                       | 230   | 201  | 0    | 1    | 0       | 28           |

Detailed Test metrics

Below are the detailed test metrics by performing manual/automation testing. The project metrics are derived from Defect density, Test coverage, Test execution coverage, test tracking and efficiency.

The various metrics that assist in test tracking and efficiency are as follows:

* Passed Test Cases Coverage: It measures the percentage of passed test cases. (Number of passed tests / Total number of tests executed) x 100
* Failed Test Case Coverage: It measures the percentage of all the failed test cases. (Number of failed tests / Total number of test cases executed) x 100

## Conclusion <a href="#toc230882388" id="toc230882388"></a>

* Functional testing was completed in accordance with the approach of enhancing the performance of MOSIP Platform document. The basic functionality of all modules is working as expected in the qaj21.mosip.net environment.
* QA Approved build release.

## QA Approval <a href="#toc230882389" id="toc230882389"></a>

Build has met the defined exit criteria and is recommended for release.

* Functional testing of RegProc, ID Repo & IDA modules.
* Automation reports -API and DSL
* Documentation Sign-off
* Test Environment Stability

**Table 8**: Report is signed off details

| Name           | Functional Role     | Responsibilities                                                                                      |
| -------------- | ------------------- | ----------------------------------------------------------------------------------------------------- |
| Ragini Krishna | Manager             | Defining test strategy, managing QA activities, and ensuring overall product quality.                 |
| Chandra Sekhar | Lead/ Test Engineer | Leading the test team, planning and executing tests, and ensuring timely delivery of quality results. |

## Appendix <a href="#toc230882390" id="toc230882390"></a>

This includes additional reference information for the report. It contains a history of document versions and a list of acronyms and their meanings.

### Appendix A: Versions <a href="#toc230882391" id="toc230882391"></a>

<table><thead><tr><th>Version</th><th>Date</th><th>Author</th><th valign="top">Reviewers</th></tr></thead><tbody><tr><td>V1.0</td><td>28/05/2026</td><td>N. Chandra Sekhar</td><td valign="top">Ragini Krishna</td></tr></tbody></table>

### Appendix B: Acronyms <a href="#toc230882392" id="toc230882392"></a>

| Acronym                                            | Literal Translation                                                                                                         |
| -------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| <p>Reg.Client</p><p>PreReg</p><p>ABIS</p><p>MV</p> | <p>Registration Client</p><p>Pre-Registration</p><p>Automated Biometric Identification System</p><p>Manual Verification</p> |

### Document History

It outlines the strategy used to ensure a comprehensive evaluation.

<table data-header-hidden><thead><tr><th>Version</th><th>Author</th><th>Date</th><th valign="top">Review</th><th valign="top">Affected Sections</th></tr></thead><tbody><tr><td>V1.0</td><td>N. Chandra Sekhar</td><td>27/05/2026</td><td valign="top">Ragini Krishna</td><td valign="top"><br></td></tr></tbody></table>

<br>
