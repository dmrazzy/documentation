# Test Report

## Introduction

The scope of testing is to verify fitment to the specification from the perspective of&#x20;

* Functionality
* Deployability
* Configurability&#x20;

### Overview and Scope

Verification is performed not only from the end user perspective but also from the System Integrator (SI) point of view. Hence Configurability and Extensibility of the software is also assessed. This ensures readiness of software for use in multiple countries. Since MOSIP is an “API First” product platform, Verification scope required comprehensive automation testing for all the MOSIP APIs. An automation Test Rig is created for the same.

## Test Approach

The Persona based approach has been adopted to perform the IV\&V, by simulating test scenarios that resemble a real-time implementation.

A Persona is a fictional character/user profile created to represent a user type that might use a product/or a service in a similar way. Persona based testing is a software testing technique that puts software testers in the customer's shoes, assesses their needs from the software and thereby determines use cases/scenarios that the customers will execute. The persona needs may be addressed through any of the following.

* Functionality
* Combination
* UI Automation
* API automation
* Upgrade Testing

## Test Organization <a href="#toc217295279" id="toc217295279"></a>

This part lists the team members involved in the testing process and their responsibilities. It clarifies who is accountable for which roles.

<table><thead><tr><th width="241.4375" valign="top">Name</th><th width="165.8671875" valign="top">Functional Role</th><th valign="top">Responsibilities</th></tr></thead><tbody><tr><td valign="top">Ragini Krishna</td><td valign="top">Manager</td><td valign="top">Defining test strategy, managing QA activities, and ensuring overall product quality.</td></tr><tr><td valign="top">N. Chandra Sekhar, Chaitanya Kesiraju, Maheswara Sarma and Mohanachandran S</td><td valign="top">Leads</td><td valign="top">Leading the test team, planning and executing tests, and ensuring timely delivery of quality results.</td></tr><tr><td valign="top">Likhitha, Swetha N, Aswin, Damodar Guru, Santosh, and Jayesh</td><td valign="top">Test engineers</td><td valign="top">Developing and executing test cases, logging defects, and verifying software quality through functional and automation.</td></tr></tbody></table>

## Test Planning <a href="#toc17829895" id="toc17829895"></a>

### Functional test results

Below are the test metrics by performing functional testing using mock MDS, mock Auth and mock ABIS. The process followed was black box testing which based its test cases on the specifications of the software component under test. Functional test was performed in combination of individual module testing as well as integration testing. Test data were prepared in line with the user stories. Expected results were monitored by examining the user interface. The coverage includes GUI testing, System testing, End-To-End flows across multiple languages and configurations. The testing cycle included simulation of multiple identity schema and respective UI schema configurations.

The Test Planning section outlines the strategy and activities planned for executing the testing process to ensure comprehensive coverage.

### Test Environment

This section details the specific environments used for testing. It lists the different platforms, and the versions of the images used.

<table><thead><tr><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Environment</td><td valign="top">Details</td></tr><tr><td valign="top">qaga.mosip.net</td><td valign="top">For Platform 1.2.1.0 testing</td></tr><tr><td valign="top">qa11.mosip.net</td><td valign="top">For Upgrade 1.2.0.1 to 1.2.1.0 testing</td></tr></tbody></table>

## Test Execution Report

### Functional test results <a href="#toc184990131" id="toc184990131"></a>

Below are the test metrics by performing functional testing using mock MDS, mock Auth and mock ABIS. The process followed was black box testing which based its test cases on the specifications of the software component under test. Functional test was performed in combination of individual module testing as well as integration testing. Test data were prepared in line with the user stories. Expected results were monitored by examining the user interface. The coverage includes GUI testing, System testing, End-To-End flows across multiple languages and configurations. The testing cycle included simulation of multiple identity schema and respective UI schema configurations.

**API Based Testing (with mock ABIS & MV -v1.2.0.2)**

<table><thead><tr><th valign="top">Module</th><th valign="top">Total</th><th valign="top">Passed</th><th valign="top">Failed</th><th valign="top">Skipped</th><th valign="top">Ignored</th><th valign="top">Known Issues</th></tr></thead><tbody><tr><td valign="top">Master Data (Eng.)</td><td valign="top">945</td><td valign="top">933</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td></tr><tr><td valign="top">Master Data (Ara.)</td><td valign="top">945</td><td valign="top">930</td><td valign="top">0</td><td valign="top">0</td><td valign="top">15</td><td valign="top">0</td></tr><tr><td valign="top">Master Data (Fra.)</td><td valign="top">945</td><td valign="top">930</td><td valign="top">0</td><td valign="top">0</td><td valign="top">15</td><td valign="top">0</td></tr><tr><td valign="top">PreReg</td><td valign="top">288</td><td valign="top">282</td><td valign="top">0</td><td valign="top">0</td><td valign="top">2</td><td valign="top">4</td></tr><tr><td valign="top">IDRepo</td><td valign="top">414</td><td valign="top">336</td><td valign="top">0</td><td valign="top">0</td><td valign="top">78</td><td valign="top">0</td></tr><tr><td valign="top">IDA</td><td valign="top">612</td><td valign="top">598</td><td valign="top">0</td><td valign="top">0</td><td valign="top">5</td><td valign="top">9</td></tr><tr><td valign="top">PMS</td><td valign="top">509</td><td valign="top">497</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td><td valign="top">12</td></tr><tr><td valign="top">Resident</td><td valign="top">1142</td><td valign="top">1142</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td></tr></tbody></table>

**DSL - End to end scenarios results (with mock ABIS & MV -v1.2.0.2)**:

<table><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Module</td><td valign="top">Total</td><td valign="top">Passed</td><td valign="top">Failed</td><td valign="top">Skipped</td><td valign="top">Ignored</td><td valign="top">Known Issues</td></tr><tr><td valign="top">DSL</td><td valign="top">209</td><td valign="top">172</td><td valign="top">2</td><td valign="top">0</td><td valign="top">12</td><td valign="top">23</td></tr></tbody></table>

&#x20;**API Based Testing (with mock ABIS & MV -v1.3.x)**:

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Module</td><td valign="top">Total</td><td valign="top">Passed</td><td valign="top">Failed</td><td valign="top">Skipped</td><td valign="top">Ignored</td><td valign="top">Known Issues</td></tr><tr><td valign="top">Master Data (Eng.)</td><td valign="top">945</td><td valign="top">933</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td></tr><tr><td valign="top">Master Data (Ara.)</td><td valign="top">945</td><td valign="top">930</td><td valign="top">0</td><td valign="top">0</td><td valign="top">15</td><td valign="top">0</td></tr><tr><td valign="top">Master Data (Fra.)</td><td valign="top">945</td><td valign="top">930</td><td valign="top">0</td><td valign="top">0</td><td valign="top">15</td><td valign="top">0</td></tr><tr><td valign="top">PreReg</td><td valign="top">288</td><td valign="top">282</td><td valign="top">1</td><td valign="top">0</td><td valign="top">2</td><td valign="top">3</td></tr><tr><td valign="top">IDRepo</td><td valign="top">414</td><td valign="top">336</td><td valign="top">0</td><td valign="top">0</td><td valign="top">78</td><td valign="top">0</td></tr><tr><td valign="top">IDA</td><td valign="top">612</td><td valign="top">598</td><td valign="top">0</td><td valign="top">0</td><td valign="top">5</td><td valign="top">9</td></tr><tr><td valign="top">PMS</td><td valign="top">509</td><td valign="top">497</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td><td valign="top">12</td></tr><tr><td valign="top">Resident</td><td valign="top">1142</td><td valign="top">1142</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td></tr></tbody></table>

&#x20;**Note**: PreReg 1 failure has been moved to Known Issues, and the PreReg API test rig was run with mock version 1.2.0.2. There were no failures reported in PreReg.

**DSL - End to end scenarios results (with mock ABIS & MV -v1.3.x)**:

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Module</td><td valign="top">Total</td><td valign="top">Passed</td><td valign="top">Failed</td><td valign="top">Skipped</td><td valign="top">Ignored</td><td valign="top">Known Issues</td></tr><tr><td valign="top">DSL</td><td valign="top">209</td><td valign="top">172</td><td valign="top">2</td><td valign="top">0</td><td valign="top">12</td><td valign="top">23</td></tr></tbody></table>

**UI Based Testing**:

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Module</td><td valign="top">Total</td><td valign="top">Passed</td><td valign="top">Failed</td><td valign="top">Skipped</td></tr><tr><td valign="top">Admin UI</td><td valign="top">16</td><td valign="top">16</td><td valign="top">0</td><td valign="top">0</td></tr><tr><td valign="top">Reg. Client UI</td><td valign="top">33</td><td valign="top">32</td><td valign="top">1</td><td valign="top">0</td></tr></tbody></table>

**Note**: Reg-client UI Automation 1 failure is only observed in automation, manually it is working, raised a Jira for Automation.

### Detailed Test metrics

Below are the detailed test metrics by performing manual/automation testing. The project metrics are derived from Defect density, Test coverage, Test execution coverage, test tracking and efficiency.

The various metrics that assist in test tracking and efficiency are as follows:

* Passed Test Cases Coverage: It measures the percentage of passed test cases. (Number of passed tests / Total number of tests executed) x 100
* Failed Test Case Coverage: It measures the percentage of all the failed test cases. (Number of failed tests / Total number of test cases executed) x 100

### API Test rig reports were generated by below sha id for mosipqa/automationtests:1.3.x image:

PreReg: sha256:9335f015b1170d7d2a082f6c8c86ffcb5cba2caf04ff7d690bd5093faf1af23d

MasterData: sha256:7b746e83193a1f0a04e66cd08e243d6df3e45371f48051517272b26b55b69013

IDRepo: sha256:610572e6f1a69e8c1e04fd7ad990379d83048c7802ebf8eacb71eab52f8376ed

IDA: sha256:9b1d874bd96f485d6e7ecfc4923a3630fb227692de1a7740672f61650ab7973b

Resident: sha256:927c2803d9a7a605142f2b64842882c83d04d5cf843822d2d381118c64c4788c

### Sonar Report

| Repo Name                                      | Branch Name              | Release Version | Coverage (>80%) | Reliability (0) | Security (0) | Hotspots (0) | Duplications |   |
| ---------------------------------------------- | ------------------------ | --------------- | --------------- | --------------- | ------------ | ------------ | ------------ | - |
| mosip-config                                   | release-1.3.x            | v1.3.0          |                 |                 |              |              |              |   |
| commons                                        | release-1.3.x            | v1.3.0          | 82.8            | 1               | 0            | 0            | 5.3          |   |
| mosip-data                                     | release-1.3.x            | v1.3.0          |                 |                 |              |              |              |   |
| bio-utils                                      | release-1.3.x            | v1.3.0          | 19.1            | 0               | 0            | 0            | 8.7          |   |
| converters                                     | release-1.3.x            | v1.3.0          | 99.4            | 0               | 0            | 0            | 0            |   |
| mosip-openid-bridge                            | release-1.3.x            | v1.3.0          | 80.8            | 0               | 0            | 0            | 13.5         |   |
| biosdk-client                                  | release-1.3.x            | v1.3.0          | 83              | 0               | 0            | 0            | 1.4          |   |
| khazana                                        | release-1.3.x            | v1.3.0          | 2.1             | 4               | 0            | 0            | 0            |   |
| demosdk                                        | release-1.3.x (ask Jana) | v1.3.0          | 97.3            | 0               | 0            | 0            | 0            |   |
| keymanager                                     | release-1.3.x            | v1.3.0          | 81.1            | 0               | 0            | 0            | 3.6          |   |
| packet-manager                                 | release-1.3.x            | v1.3.0          |                 |                 |              |              |              |   |
| biosdk-services                                | release-1.3.x            | v1.3.0          | 96.5            | 0               | 0            | 0            | 0            |   |
| id-repository                                  | release-1.3.x            | v1.3.0          | 82.8            | 0               | 0            | 0            | 3.2          |   |
| registration                                   | release-1.3.x            | v1.3.0          | 83.5            | 1               | 0            | 0            | 8.4          |   |
| id-authentication                              | release-1.3.x            | v1.3.0          | 67.4            | 18              | 0            | 0            | 2.6          |   |
| admin-services                                 | release-1.3.x            | v1.3.0          | 71.7            | 1               | 0            | 0            | 3.5          |   |
| admin-ui                                       | release-1.3.x            | v1.3.0          | 0               | 69              | 0            | 0            | 9            |   |
| captcha                                        | release-0.1.x            | v0.1.0          | 46.5            | 0               | 0            | 0            | 0            |   |
| mosip-ref-impl                                 | release-1.3.x            | v1.3.0          |                 |                 |              |              |              |   |
| durian                                         | release-1.3.x            | v1.3.0          | 82.7            | 0               | 0            | 0            | 0            |   |
| otp manager                                    | release-1.3.x            | v1.3.0          | 48.1            | 0               | 0            | 0            | 0            |   |
| print                                          | release-1.3.x            | v1.3.0          | 73.2            | 4               | 0            | 0            | 1.9          |   |
| resident-services                              | release-1.3.x            | v1.3.0          | 80.4            | 0               | 0            | 0            | 1.9          |   |
| websub                                         | release-1.3.x            | v1.3.0          |                 |                 |              |              |              |   |
| audit-manager                                  | release-1.3.x            | v1.3.0          | 70.4            | 0               | 0            | 0            | 0            |   |
| image-compressor (release but no announcement) | release-0.1.x            | v0.1.0          | 81.6            | 0               | 0            | 0            | 11.8         |   |
| imagedecoder (release but no announcement)     | release-0.10.x           | v0.10.0         | 30.9            | 0               | 0            | 0            | 7.4          |   |
| mosip-mock-services                            | release-1.3.x            | v1.3.0          |                 |                 |              |              |              |   |
| nfiq (ask Jana)                                | release-0.1.x            | v0.1.0          | 76              | 3               | 0            | 0            | 7.1          |   |
| mosip-onboarding                               | release-1.3.x            | v1.3.0          | N/A             | N/A             | N/A          | N/A          | N/A          |   |
| pre-registration                               | release-1.3.x            | v1.3.0          | 82.6            | 0               | 5            | 0            | 2.8          |   |
| pre-registration-ui                            | release-1.3.x            | v1.3.0          | 0               | 0               | 0            | 0            | 2.3          |   |
| digital-card-service                           | release-1.3.x            | v1.3.0          | 69              | 0               | 0            | 0            | 0            |   |
| artifactory-ref-impl                           | release-1.3.x            | v1.3.0          |                 |                 |              |              |              |   |

## Observations:

* Meta data is missing in the Minio during backup and restore (MOSIP-44059)
* Template DB Scripts are missing for update statements in the DB upgrade scripts rely on auto-generated IDs (MOSIP-44100)

## Out of Sope:

* Roll back scripts were not verified from QA Side.
* Partner Management System and Registration Client
* Deployment and Docker compose Testing.
* Real BIO-SDK and ABIS

## Conclusion

* Basic functionality of all the Modules in Java 21 env is the same as the functionality in Java 11 env.
* QA Approved build release.

## QA Approval

Build has met the defined exit criteria and is recommended for release.

* Test Case Execution Completion
* Defect Status Closure
* Automation reports -API, DSL and UI
* Documentation Sign-off
* Test Environment Stability

&#x20;         &#x20;

**Report signed off details**

<table><thead><tr><th width="167.4453125" valign="top">Name</th><th width="134.84765625" valign="top">Functional Role</th><th valign="top">Responsibilities</th></tr></thead><tbody><tr><td valign="top">Ragini Krishna</td><td valign="top">Manager</td><td valign="top">Defining test strategy, managing QA activities, and ensuring overall product quality.</td></tr><tr><td valign="top">Chandra Sekhar</td><td valign="top">Lead</td><td valign="top">Leading the test team, planning and executing tests, and ensuring timely delivery of quality results.</td></tr><tr><td valign="top">Chaitanya Kesiraju</td><td valign="top">Lead</td><td valign="top">Leading the test team, planning and executing tests, and ensuring timely delivery of quality results.</td></tr></tbody></table>

## Appendix

This includes additional reference information for the report. It contains a history of document versions and a list of acronyms and their meanings.

### Appendix A: Versions

<table data-header-hidden><thead><tr><th></th><th></th><th></th><th valign="top"></th></tr></thead><tbody><tr><td>Version</td><td>Date</td><td>Author</td><td valign="top">Reviewers</td></tr><tr><td>V1.0</td><td>19/12/2025</td><td>N. Chandra Sekhar</td><td valign="top">Ragini Krishna</td></tr></tbody></table>

### &#x20;Appendix B: Acronyms

<table data-header-hidden><thead><tr><th valign="top">Acronym</th><th valign="top">Literal Translation</th></tr></thead><tbody><tr><td valign="top"><p>Reg.Client</p><p>PreReg</p><p>IDA</p><p>ABIS</p><p>MV</p></td><td valign="top"><p>Registration Client</p><p>Pre-Registration</p><p>ID Authentication</p><p>Automated Biometric Identification System</p><p>Manual Verification</p></td></tr></tbody></table>

### Document History

It outlines the strategy used to ensure a comprehensive evaluation.

<table><thead><tr><th width="130.73046875">Version</th><th>Author</th><th>Date</th><th valign="top">Review</th><th valign="top">Affected Sections</th></tr></thead><tbody><tr><td>V1.0</td><td>N. Chandra Sekhar</td><td>18/12/2025</td><td valign="top">Ragini Krishna</td><td valign="top"> </td></tr></tbody></table>

&#x20;

