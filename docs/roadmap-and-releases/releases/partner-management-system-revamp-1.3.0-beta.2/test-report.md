# Test Report



## Testing Scope

The scope of testing is to verify fitment to the specification from the perspective of&#x20;

●      Functionality&#x20;

●      Deployability&#x20;

●      Configurability&#x20;

●      Customizability

&#x20;

Verification is performed not only from the end user perspective but also from the System Integrator (SI) point of view. Hence Configurability and Extensibility of the software is also assessed. This ensures the readiness of software for use in multiple countries. Since MOSIP is an “API First” product platform, Verification scope required comprehensive automation testing for all the MOSIP APIs. An automation Test Rig is created for the same.

&#x20;

The Partner Management System Revamp testing scope revolves around the following flows:

&#x20;

●      Features: Authentication Partner, Device Partner, FTM Partner, Partner Admin Certificate Trust Store, Partners, Policies, Partner-Policy Linking, SBI-Device, FTM Chip, Authentication Services, User Profile, User Dashboard, Root CA Certificate expiry notification, Intermediate certificate expiry notifications, Partner Certificate Notification expiry notification, API Key expiry, FTM Chip expiry, SBI ID expiry notifications, Weekly Summary notifications for Partner certificate expiry, API Key expiry, FTM Chip expiry, SBI ID.

●      Multilingual (English/Arabic/French)

●      Multi-browser testing: Using Edge, Firefox, and Chrome browsers (Windows/ Mac, Tablet, Extra-large screen)

●      Regression Testing

●      Integration Testing

## Test Approach

Persona based approach has been adopted to perform the IV\&V, by simulating test scenarios that resemble a real-time implementation.

&#x20;

A Persona is a fictional character/user profile created to represent a user type that might use a product/or a service in a similar way. Persona based testing is a software testing technique that puts software testers in the customer's shoes, assesses their needs from the software and thereby determines use cases/scenarios that the customers will execute. The persona needs may be addressed through any of the following.

&#x20;

&#x20;

●      Functionality&#x20;

●      Deployability&#x20;

●      Configurability&#x20;

●      Customizability

&#x20;

The verification methods may differ based on how the need was addressed.

&#x20;

For regression check, “MOSIP Test Rig” - an automation testing suite - which is indigenously designed and developed for supporting persona-based testing. MOSIP Test Rig covers the end-to-end test execution and reporting. The end-to-end functional test scenarios are written starting from creation of packet in registration center, processing the packet through the registration processor, generating UIN and authenticating identity using IDA through various permutation and combinations of cases being covered. MOSIP Test Rig will be an open-source artifact which can also be enhanced and used by countries to validate the SI deliveries before going live. Persona classes include positive personas.

&#x20;

## Verified configuration

Verification is performed on various configurations as mentioned below

·      Default configuration with verified configuration for 3 Lang (English/Arabic/French)

## Browser compatibility evaluations

·      Browser versions tested on desktop/laptop

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Sl.No</td><td valign="top">Browser</td><td valign="top">Versions</td></tr><tr><td valign="top">1</td><td valign="top">Chrome</td><td valign="top">Version 138.0.7204.185</td></tr><tr><td valign="top">2</td><td valign="top">Firefox</td><td valign="top">Version 142.0</td></tr><tr><td valign="top">3</td><td valign="top">Edge</td><td valign="top">Version 138.0.3351.121</td></tr><tr><td valign="top">4</td><td valign="top">Safari</td><td valign="top">Version 18.6 (20621.3.11.11.3</td></tr></tbody></table>

&#x20;

·      Browser versions tested on tablet device

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Sl.No</td><td valign="top">Browser</td><td valign="top">Versions</td></tr><tr><td valign="top">1</td><td valign="top">Chrome</td><td valign="top">Version 139.0.7258.62</td></tr><tr><td valign="top">2</td><td valign="top">Firefox</td><td valign="top">Version 141.0.3</td></tr><tr><td valign="top">3</td><td valign="top">Edge</td><td valign="top">Version 139.0.3405.86</td></tr></tbody></table>

&#x20;

·      Browser versions tested on extra-large screens

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Sl.No</td><td valign="top">Browser</td><td valign="top">Versions</td></tr><tr><td valign="top">1</td><td valign="top">Chrome</td><td valign="top">Version 138.0.7204.185</td></tr><tr><td valign="top">2</td><td valign="top">Firefox</td><td valign="top">Version 142.0</td></tr><tr><td valign="top">3</td><td valign="top">Edge</td><td valign="top">Version 138.0.3351.121</td></tr><tr><td valign="top">4</td><td valign="top">Safari</td><td valign="top">Version 18.6 (20621.3.11.11.3)</td></tr></tbody></table>

&#x20;

## Screen sizes used for UI responsiveness validation

Ø  Laptop/Desktop: 1920x1080

Ø  Tablet: 1280X800

Ø  Extra-large screens: 3840x2160

Ø  Mac book: 2560 x 1664

&#x20;

&#x20;

## Feature Health

&#x20;

## Test execution statistics

### Functional test results by modules

Below are the test metrics by performing functional testing. The process followed was black box testing which based its test cases on the specifications of the software component under test. The functional test was performed in combination with individual module testing as well as integration testing. Test data were prepared in line with the user stories. Expected results were monitored by examining the user interface. The coverage includes GUI testing, System testing, End-To-End flows across multiple languages and configurations. The testing cycle included simulation of multiple identity schema and respective UI schema configurations.

&#x20;

MANUAL VERIFICATION (UI):

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Total</td><td valign="top">Passed</td><td valign="top">Failed</td><td valign="top">Skipped (N/A)</td></tr><tr><td valign="top">5945</td><td valign="top">5890</td><td valign="top">32</td><td valign="top">23</td></tr><tr><td valign="top">Test Rate: 99% with Pass Rate: 99%</td><td valign="top"></td><td valign="top"></td><td valign="top"></td></tr></tbody></table>

Note: NA - 23 Test Cases which are descoped scenarios/not developed feature

&#x20;

&#x20;

MANUAL VERIFICATION (API):

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Total</td><td valign="top">Passed</td><td valign="top">Failed</td><td valign="top">Skipped (N/A)</td></tr><tr><td valign="top">742</td><td valign="top">741</td><td valign="top">0</td><td valign="top">1</td></tr><tr><td valign="top">Test Rate: 99% with Pass Rate: 99%</td><td valign="top"></td><td valign="top"></td><td valign="top"></td></tr></tbody></table>

&#x20;

Note: NA - 1 Test Cases which are descoped scenarios/not developed feature

&#x20;

&#x20;

API Test Rig:

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Total</td><td valign="top">Passed</td><td valign="top">Failed</td><td valign="top">Skipped (N/A)</td><td valign="top">Ignored</td><td valign="top">Known issues</td></tr><tr><td valign="top">1068</td><td valign="top">1068</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td><td valign="top">0</td></tr><tr><td valign="top">Test Rate: 100% with Pass Rate: 100%</td><td valign="top"></td><td valign="top"></td><td valign="top"></td><td valign="top"></td><td valign="top"></td></tr></tbody></table>

&#x20;

Note- API flow is tested through automation for both positive and negative scenarios, while test cases that are not automated are tested manually.

&#x20;

### Detailed Test metrics

Below are the detailed test metrics by performing Manual/automation testing. The project metrics are derived from Defect density, Test coverage, Test execution coverage, test tracking and efficiency.

The various metrics that assist in test tracking and efficiency are as follows:

●     Passed Test Cases Coverage: It measures the percentage of passed test cases. (Number of passed tests / Total number of tests executed) x 100

●     Failed Test Case Coverage: It measures the percentage of all failed test cases. (Number of failed tests / Total number of test cases executed) x 100

&#x20;

### Tested with components:

&#x20;

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Module/Repo</td><td valign="top">Compatible Version</td><td valign="top">Comments</td></tr><tr><td valign="top">partner-management-portal</td><td valign="top">mosipqa/pmp-ui-v2:1.3.x</td><td valign="top"> </td></tr><tr><td valign="top">partner-management-services</td><td valign="top">mosipqa/partner-management-service:1.3.x</td><td valign="top"> </td></tr><tr><td valign="top">Policy Management service</td><td valign="top">mosipqa/policy-management-service:1.3.x</td><td valign="top"> </td></tr><tr><td valign="top">Key-manager</td><td valign="top">mosipid/kernel-keymanager-service:1.3.0-beta.3</td><td valign="top"> </td></tr><tr><td valign="top">IDA Auth</td><td valign="top">mosipid/authentication-internal-service:1.2.1.0</td><td valign="top"> </td></tr><tr><td valign="top">Artifactory</td><td valign="top">mosipid/artifactory-server:1.2.0.2</td><td valign="top"> </td></tr><tr><td valign="top">eSignet</td><td valign="top">mosipid/esignet:1.4.1</td><td valign="top"> </td></tr><tr><td valign="top">Notifier (Kernel)</td><td valign="top">mosipid/kernel-notification-service:1.2.0.1</td><td valign="top"> </td></tr><tr><td valign="top">Audit manager</td><td valign="top">mosipid/kernel-auditmanager-service:1.2.0.1</td><td valign="top"> </td></tr><tr><td valign="top">ID Repro</td><td valign="top">mosipid/id-repository-identity-service:1.2.2.0</td><td valign="top"> </td></tr><tr><td valign="top">datashare</td><td valign="top">mosipid/data-share-service:1.2.0.1</td><td valign="top"> </td></tr><tr><td valign="top">Keycloak</td><td valign="top">1.2.0.1</td><td valign="top"> </td></tr><tr><td valign="top">config-server</td><td valign="top">mosipqa/kernel-keymanager-service:1.3.x</td><td valign="top"> </td></tr><tr><td valign="top">Websub</td><td valign="top">mosipid/websub-service:1.2.0.1</td><td valign="top"> </td></tr><tr><td valign="top">postgres</td><td valign="top">mosipid/artifactory-server:1.2.0.2</td><td valign="top"> </td></tr></tbody></table>

### &#x20;

### &#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

### Sonar Report

&#x20;

·      Partner-Management-Service:

&#x20;

&#x20;

&#x20;

·       Partner-Management-Portal:

&#x20;
