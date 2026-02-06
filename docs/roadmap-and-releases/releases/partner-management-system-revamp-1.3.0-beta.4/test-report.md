# Test Report

## Introduction

The Partner Management System Revamp testing scope includes the following: Features: MISP Partner, ABIS Partner, Authentication Partner, Device Partner, FTM Partner, Partner Admin, Certificate Trust Store, Partners, Policies, Partner-Policy Linking, SBI-Device, FTM Chip, Authentication Services, User Profile, User Dashboard, Root CA Certificate expiry notifications, Intermediate Certificate expiry notifications, Partner Certificate expiry notifications, API Key expiry notifications, FTM Chip expiry notifications, SBI ID expiry notifications, and Weekly Summary notifications for Partner Certificate, API Key, FTM Chip, and SBI ID expiry.

### Overview and Scope

The scope of testing defines the boundaries, functionalities, and features that will be tested for the Partner Management System (PMS) Revamp. This ensures comprehensive validation of critical workflows while clearly identifying what is included and excluded from testing.

Functional Features: MISP Partner, ABIS Partner, Authentication Partner, Device Partner, FTM Partner, Partner Admin, Certificate Trust Store, Partners, Policies, Partner–Policy Linking, SBI–Device Mapping, FTM Chip, Authentication Services, User Profile, User Dashboard.

Cross-Platform Support:

* Multilingual: English, Arabic, French
* Multi-Browser: Edge, Firefox, Chrome
* Devices: Windows, Mac, Tablet, Mobile, Extra-large screens
* Testing Types: Sanity, Regression Testing and Integration Testing
* Cross-browser and cross-device compatibility testing.

## 2 Test Approach

The scope of testing is to verify fitment to the specification from the perspective of&#x20;

* Functionality
* Combination
* UI Automation
* API automation
* Library verification

## 3 Test Organization <a href="#toc221277010" id="toc221277010"></a>

Table 1: Test Organization

<table><thead><tr><th valign="top">Name</th><th valign="top">Functional Role</th><th valign="top">Responsibilities</th></tr></thead><tbody><tr><td valign="top">Ragini Krishnamurthy</td><td valign="top">Manager</td><td valign="top">Defining test strategy, managing QA activities, and ensuring overall product quality.</td></tr><tr><td valign="top">Chandra Sekhar</td><td valign="top">Lead</td><td valign="top">Leading the test team, planning and executing tests, and ensuring timely delivery of quality results.</td></tr><tr><td valign="top"><p>Swetha N</p><p>Rachana S P</p></td><td valign="top">Test engineers</td><td valign="top">Designing and executing test cases, performing functional and regression testing, verifying PMS module functionality, logging and tracking defects, and validating fixes to ensure quality standards.</td></tr></tbody></table>

### Test Planning <a href="#toc17829895" id="toc17829895"></a>

This Test Plan outlines the testing approach, scope, resources, and schedule for the Partner Management System (PMS) Revamp. The objective is to ensure that all functional, integration, and non-functional requirements are met with high quality before release.

* Validate end-to-end functionality of the Partner Management System Revamp.
* Ensure system stability across supported browsers, devices, and languages.
* Verify integrations with dependent systems and services.
* Identify and mitigate risks early through regression and integration testing

### Sanity Scenarios Verified

Sanity testing will be performed to ensure basic application stability before detailed test execution. The following high-level sanity scenarios will be verified:

* Application accessibility and successful login for different partner roles
* Core partner creation and management flows (MISP, Device, Auth, FTM and ABIS Partner)
* Partner–Policy linking
* Authentication services basic validation
* Certificate upload and validation
* Notification triggers for certificate/API key expiry (basic check)
* User profile and dashboard accessibility

Only upon successful completion of sanity testing will the build be accepted for full regression and integration testing.

### Test Environment

https://qa-core.mosip.net/

Table 2: Test Environment -images

<table><thead><tr><th valign="top">Tested on qa-core env with components</th></tr></thead><tbody><tr><td valign="top">docker.io/mosipid/admin-service:1.2.1.1</td></tr><tr><td valign="top">docker.io/mosipid/admin-ui:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/apitest-auth:1.2.1.1-beta.1</td></tr><tr><td valign="top">docker.io/mosipid/apitest-esignet:1.6.2</td></tr><tr><td valign="top">docker.io/mosipid/apitest-idrepo:1.2.2.2</td></tr><tr><td valign="top">docker.io/mosipid/apitest-masterdata:1.2.1.2</td></tr><tr><td valign="top">docker.io/mosipid/apitest-resident:1.2.1.2</td></tr><tr><td valign="top">docker.io/mosipid/artifactory-server:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/artifactory-server:1.2.0.3</td></tr><tr><td valign="top">docker.io/mosipid/artifactory-server:1.3.0-beta.1</td></tr><tr><td valign="top">docker.io/mosipid/artifactory-server:1.4.1-ES</td></tr><tr><td valign="top">docker.io/mosipid/authentication-internal-service:1.2.1.0</td></tr><tr><td valign="top">docker.io/mosipid/authentication-otp-service:1.2.1.0</td></tr><tr><td valign="top">docker.io/mosipid/authentication-service:1.2.1.0</td></tr><tr><td valign="top">docker.io/mosipid/biosdk-server:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/commons-packet-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/config-server:1.1.2</td></tr><tr><td valign="top">docker.io/mosipid/consolidator-websub-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/credential-request-generator:1.2.2.0</td></tr><tr><td valign="top">docker.io/mosipid/credential-service:1.2.2.0</td></tr><tr><td valign="top">docker.io/mosipid/data-share-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/esignet-with-plugins:1.6.2</td></tr><tr><td valign="top">docker.io/mosipid/esignet:1.4.1</td></tr><tr><td valign="top">docker.io/mosipid/hotlist-service:1.2.1.1</td></tr><tr><td valign="top">docker.io/mosipid/id-repository-identity-service:1.2.2.0</td></tr><tr><td valign="top">docker.io/mosipid/id-repository-salt-generator:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/id-repository-vid-service:1.2.2.0</td></tr><tr><td valign="top">docker.io/mosipid/kernel-auditmanager-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/kernel-auth-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/kernel-idgenerator-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/kernel-keymanager-service:1.3.0-beta.3</td></tr><tr><td valign="top">docker.io/mosipid/kernel-masterdata-service:1.2.1.1</td></tr><tr><td valign="top">docker.io/mosipid/kernel-notification-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/kernel-otpmanager-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/kernel-pridgenerator-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/kernel-ridgenerator-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/kernel-salt-generator:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/kernel-syncdata-service:1.2.1.1</td></tr><tr><td valign="top">docker.io/mosipid/keycloak-init:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/keys-generator:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/minio-client-util:latest</td></tr><tr><td valign="top">docker.io/mosipid/mock-abis:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/mock-identity-system:0.11.2</td></tr><tr><td valign="top">docker.io/mosipid/mock-identity-system:0.9.3</td></tr><tr><td valign="top">docker.io/mosipid/mock-mv:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/mock-relying-party-service:0.11.2</td></tr><tr><td valign="top">docker.io/mosipid/mock-relying-party-service:0.9.3</td></tr><tr><td valign="top">docker.io/mosipid/mock-relying-party-ui:0.11.2</td></tr><tr><td valign="top">docker.io/mosipid/mock-relying-party-ui:0.9.3</td></tr><tr><td valign="top">docker.io/mosipid/mock-smtp:1.0.0</td></tr><tr><td valign="top">docker.io/mosipid/mosip-artemis-keycloak:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/mosip-file-server:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/oidc-ui:1.4.1</td></tr><tr><td valign="top">docker.io/mosipid/oidc-ui:1.6.2</td></tr><tr><td valign="top">docker.io/mosipid/partner-onboarder:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/postgres-init:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/pre-registration-application-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/pre-registration-batchjob:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/pre-registration-booking-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/pre-registration-captcha-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/pre-registration-datasync-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/pre-registration-ui:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/print:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipid/registration-client:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/registration-processor-common-camel-bridge:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/registration-processor-dmz-packet-server:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/registration-processor-notification-service:1.2.1.1</td></tr><tr><td valign="top">docker.io/mosipid/registration-processor-registration-transaction-service:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/registration-processor-reprocessor:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/registration-processor-stage-group-1:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/registration-processor-stage-group-2:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/registration-processor-stage-group-3:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/registration-processor-stage-group-4:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/registration-processor-stage-group-5:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/registration-processor-stage-group-6:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/registration-processor-stage-group-7:1.2.0.2</td></tr><tr><td valign="top">docker.io/mosipid/resident-service:1.2.1.1</td></tr><tr><td valign="top">docker.io/mosipid/resident-ui:0.9.1</td></tr><tr><td valign="top">docker.io/mosipid/softhsm:v2</td></tr><tr><td valign="top">docker.io/mosipid/websub-service:1.2.0.1</td></tr><tr><td valign="top">docker.io/mosipqa/activemq-artemis:1.1.5</td></tr><tr><td valign="top">docker.io/mosipqa/apitest-esignet:develop</td></tr><tr><td valign="top">docker.io/mosipqa/apitest-pms:1.3.x</td></tr><tr><td valign="top">docker.io/mosipqa/certmanager:develop</td></tr><tr><td valign="top">docker.io/mosipqa/dsl-orchestrator:1.3.x</td></tr><tr><td valign="top">docker.io/mosipqa/dsl-orchestrator:develop</td></tr><tr><td valign="top">docker.io/mosipqa/dsl-packetcreator:1.3.x</td></tr><tr><td valign="top">docker.io/mosipqa/dsl-packetcreator:develop</td></tr><tr><td valign="top">docker.io/mosipqa/inji-verify-service:develop</td></tr><tr><td valign="top">docker.io/mosipqa/inji-verify-ui:develop</td></tr><tr><td valign="top">docker.io/mosipqa/partner-management-service:1.3.x</td></tr><tr><td valign="top">docker.io/mosipqa/pmp-ui-v2:1.3.x</td></tr><tr><td valign="top">docker.io/mosipqa/policy-management-service:1.3.x</td></tr><tr><td valign="top">docker.io/mosipqa/postgres-init:develop</td></tr><tr><td valign="top">docker.io/mosipqa/uitest-pmp-v2:1.3.x</td></tr><tr><td valign="top"><p> </p><p>docker.io/mosipqa/uitest-resident:develop</p><p> </p></td></tr><tr><td valign="top"><p> </p><p>mosipid/apitest-esignet:1.6.2</p><p> </p></td></tr><tr><td valign="top"><p> </p><p>mosipid/config-server:1.1.2</p><p> </p></td></tr><tr><td valign="top"><p> </p><p>mosipid/keycloak-init:1.2.0.1</p><p> </p></td></tr><tr><td valign="top"><p> </p><p>mosipid/postgres-init:1.2.0.1</p><p> </p></td></tr><tr><td valign="top"><p> </p><p>mosipid/softhsm:v2</p><p> </p></td></tr><tr><td valign="top"><p> </p><p>mosipqa/apitest-esignet:develop</p><p> </p></td></tr><tr><td valign="top"><p> </p><p>mosipqa/postgres-init:develop</p><p> </p></td></tr><tr><td valign="top"><p> </p><p>mosipqa/uitest-pmp-v2:1.3.x</p><p> </p></td></tr><tr><td valign="top"><p> </p><p>mosipqa/uitest-pmp-v2:release-1.3.x</p><p> </p></td></tr></tbody></table>

## Test Execution Report

Below are the test metrics by performing functional testing. The process followed was black box testing which based its test cases on the specifications of the software component under test. The functional test was performed in combination with individual module testing as well as integration testing. Test data were prepared in line with the user stories. Expected results were monitored by examining the user interface. The coverage includes GUI testing, System testing, End-To-End flows across multiple languages and configurations.

### Test case execution summary

The Test Case Execution Summary section provides a detailed overview of the total test cases executed across platforms, including pass, fail, and skip counts. It includes a table summarizing results and observations on execution pass rates.<br>

Table 3: Test Case - Manual Verification (UI):

<table><thead><tr><th valign="top">Total</th><th valign="top">Passed</th><th valign="top">Failed</th><th valign="top">Skipped (N/A)</th></tr></thead><tbody><tr><td valign="top">7272</td><td valign="top">7254</td><td valign="top">40</td><td valign="top">33</td></tr><tr><td valign="top">Test Rate: 99% with Pass Rate: 99.7%</td><td valign="top"></td><td valign="top"></td><td valign="top"></td></tr></tbody></table>

&#x20;Note: NA - 33 Test Cases which are descoped scenarios/not developed feature

Table 4: Test Case - Manual Verification (API):

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Total</td><td valign="top">Passed</td><td valign="top">Failed</td><td valign="top">Skipped (N/A)</td></tr><tr><td valign="top">1249</td><td valign="top">1237</td><td valign="top">2</td><td valign="top">10</td></tr><tr><td valign="top">Test Rate: 100% with Pass Rate: 100%</td><td valign="top"></td><td valign="top"></td><td valign="top"></td></tr></tbody></table>

&#x20;Note: NA - 6 Test Cases which are descoped scenarios/not developed feature

### Automation Results

This section provides a summary of the automated test execution. It shows the pass, fail, and known issues from the automated test suite.

Table 5: Automation Execution Result -API Testrig

Note- API flow is tested through automation for both positive and negative scenarios, while test cases that are not automated are tested manually.

Table 6: Automation Execution Result - UI Automation

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Total</td><td valign="top">Passed</td><td valign="top">Failed</td><td valign="top">Skipped (N/A)</td><td valign="top">Ignored</td><td valign="top">Known issues</td></tr><tr><td valign="top">88</td><td valign="top">88</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td></tr><tr><td valign="top">Test Rate: 100% with Pass Rate: 100%</td><td valign="top"></td><td valign="top"></td><td valign="top"></td><td valign="top"></td><td valign="top"></td></tr></tbody></table>

### Detailed Test metrics

Below are the detailed test metrics by performing Manual/automation testing. The project metrics are derived from Defect density, Test coverage, Test execution coverage, test tracking and efficiency.

The various metrics that assist in test tracking and efficiency are as follows:

* Passed Test Cases Coverage: It measures the percentage of passed test cases. (Number of passed tests / Total number of tests executed) x 100
* Failed Test Case Coverage: It measures the percentage of all failed test cases. (Number of failed tests / Total number of test cases executed) x 100

## Test Execution Report

Verification is performed on various configurations as mentioned below

* Default configuration with verified configuration for 3 Lang (English/Arabic/French)

## Browser compatibility evaluations

Table 7: Browser versions tested on desktop/laptop

<table><thead><tr><th valign="top">Sl.No</th><th valign="top">Browser</th><th valign="top">Versions</th></tr></thead><tbody><tr><td valign="top">1</td><td valign="top">Chrome</td><td valign="top">Version 144.0.7559.110</td></tr><tr><td valign="top">2</td><td valign="top">Firefox</td><td valign="top">Version 147.0.2</td></tr><tr><td valign="top">3</td><td valign="top">Edge</td><td valign="top">Version 144.0.3719.92</td></tr><tr><td valign="top">4</td><td valign="top">Safari</td><td valign="top">Version 18.6 (20621.3.11.11.3</td></tr></tbody></table>

&#x20;Table 8: Browser versions tested on tablet device

<table><thead><tr><th valign="top">Sl.No</th><th valign="top">Browser</th><th valign="top">Versions</th></tr></thead><tbody><tr><td valign="top">1</td><td valign="top">Chrome</td><td valign="top">Version 144.0.7559.110</td></tr><tr><td valign="top">2</td><td valign="top">Firefox</td><td valign="top">Version 147.0.2</td></tr><tr><td valign="top">3</td><td valign="top">Edge</td><td valign="top">Version 144.0.3719.92</td></tr></tbody></table>

Table 9: Browser versions tested on extra-large screens

<table><thead><tr><th valign="top">Sl.No</th><th valign="top">Browser</th><th valign="top">Versions</th></tr></thead><tbody><tr><td valign="top">1</td><td valign="top">Chrome</td><td valign="top">Version 144.0.7559.110</td></tr><tr><td valign="top">2</td><td valign="top">Firefox</td><td valign="top">Version 147.0.2</td></tr><tr><td valign="top">3</td><td valign="top">Edge</td><td valign="top">Version 144.0.3719.92</td></tr><tr><td valign="top">4</td><td valign="top">Safari</td><td valign="top">Version 18.6 (20621.3.11.11.3)</td></tr></tbody></table>

### &#x20;Screen sizes used for UI responsiveness validation

* Laptop/Desktop: 1920x1080
* Tablet: 1280X800
* Extra-large screens: 3840x2160
* Mac book: 2560 x 1664

## Feature Health

### Known Issues Metrics

This section focuses on a separate category of issues that are known but not addressed in the current release. It provides a count and severity distribution for these defects across releases.

Table 10: Defect Metrics for the known issues

<table><thead><tr><th valign="top">Blocker</th><th valign="top">Critical</th><th valign="top">Major</th><th valign="top">Minor</th><th valign="top">Total</th></tr></thead><tbody><tr><td valign="top">0</td><td valign="top">0</td><td valign="top">12</td><td valign="top">28</td><td valign="top">40</td></tr></tbody></table>

### Sonar Report

* Partner-Management-Service:
* Partner- Management-Portal:

## Conclusion

The Partner Management System (PMS) Revamp version 1.3.0-beta.4 has been successfully validated in the qa-core environment, with all critical functionalities performing as expected.

Sanity, regression, and integration testing were completed within the defined scope.

No critical or high-severity defects remain open.

Based on the successful test execution and results, QA approves the build for release.

### QA Approval

The build has met all the defined exit criteria and is recommended for release based on the following:

* Test Case Execution: All planned test cases have been executed successfully.
* Story and Defect Closure: All user stories are completed, and no critical or high-severity defects remain open.
* Automation Reports: API-Testrig and UI automation execution reports have been reviewed and approved.
* Documentation Sign-off: All required test and release documentation has been reviewed and signed off.
* Test Environment Stability: The test environment remained stable throughout the testing cycle.

&#x20;

&#x20;Table 11: Report is signed off details

<table data-header-hidden><thead><tr><th valign="top">Name</th><th valign="top">Functional Role</th><th valign="top">Responsibilities</th></tr></thead><tbody><tr><td valign="top">Ragini Krishna</td><td valign="top">Manager</td><td valign="top">Defining test strategy, managing QA activities, and ensuring overall product quality.</td></tr><tr><td valign="top">Chandra Sekhar N</td><td valign="top">Lead</td><td valign="top">Leading the test team, planning and executing tests, and ensuring timely delivery of quality results.</td></tr></tbody></table>

## Appendix

This includes additional reference information for the report. It contains a history of document versions and a list of acronyms and their meanings.

### Appendix A: Versions

<table><thead><tr><th valign="top">Version</th><th valign="top">Date</th><th valign="top">Author</th><th valign="top">Reviewers</th></tr></thead><tbody><tr><td valign="top">V1.0</td><td valign="top">30/01/2026</td><td valign="top">Swetha N</td><td valign="top">Ragini Krishna</td></tr></tbody></table>

### Appendix B: Acronyms

<table data-header-hidden><thead><tr><th valign="top">Acronym</th><th valign="top">Literal Translation</th></tr></thead><tbody><tr><td valign="top"><p>SBI</p><p>FTM</p><p>ABIS</p></td><td valign="top"><p>Secure Biometric Interface</p><p>Foundational Trust Module</p><p>Automated Biometric Identification System</p></td></tr></tbody></table>
