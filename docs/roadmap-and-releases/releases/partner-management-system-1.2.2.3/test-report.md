# Test Report

## Testing Scope

The scope of testing is to verify fitment to the specification from the perspective of &#x20;

* Functionality &#x20;
* Configurability &#x20;
* Customizability

Verification is performed not only from the end user perspective but also from the System Integrator (SI) point of view. Hence Configurability and Extensibility of the software is also assessed. This ensures readiness of software for use in multiple countries. Since MOSIP is an “API First” product platform, Verification scope required comprehensive automation testing for all the MOSIP APIs. An automation Test Rig is created for the same.&#x20;

## Test Approach

Persona based approach has been adopted to perform the IV\&V, by simulating test scenarios that resemble a real-time implementation.&#x20;

A Persona is a fictional character/user profile created to represent a user type that might use a product/or a service in a similar way. Persona based testing is a software testing technique that puts software testers in the customer's shoes, assesses their needs from the software and thereby determines use cases/scenarios that the customers will execute. The persona needs may be addressed through any of the following.&#x20;

* Functionality &#x20;
* Deployability &#x20;
* Configurability &#x20;
* Customizability

The verification methods may differ based on how the need was addressed.&#x20;

For regression check, “MOSIP Test Rig” - an automation testing suite - which is indigenously designed and developed for supporting persona based testing. MOSIP Test Rig covers the end to end test execution and reporting. The end to end functional test scenarios are written starting from pre-registration, to creation of packet in registration center, processing the packet through the registration processor, generating UIN and authenticating identity using IDA through various permutation and combinations of cases being covered. MOSIP Test Rig will be an open source artifact which can also be enhanced and used by countries to validate the SI deliveries before going live. Persona classes include both negative and positive personas. Negative persona classes include users like Bribed Registration Office, Malicious Insider etc. The needs of positive persona classes must be met, whereas the needs of negative persona classes must be effectively restricted by the software.

## Main feature tested

* Bugs - Manual Verification
* Module Sanity
* Automation Testing - API testrig&#x20;

## Not in scope for QA testing

* Upgrade testing, Real Device testing, Documentation Review, Deployment and Docker compose testing

## Test execution statistics&#x20;

### Functional test results

The process followed was black box testing which based on the specifications of the software component under test. Functional test was performed in combination of individual module testing as well as integration testing. Test data were prepared in line. Expected results were monitored by examining the user interface. The coverage includes System testing, End-To-End flows across multiple languages and configurations. The testing cycle included simulation of multiple identity schema and respective UI schema configurations.

### PMS automation results:

<table><thead><tr><th width="190.8828125">Platform</th><th>Total</th><th>Pass</th><th>Fail</th><th>Skip</th><th>Ignored</th><th>Known Issues</th></tr></thead><tbody><tr><td>API </td><td>511</td><td>497</td><td>0</td><td>0</td><td>2</td><td>12</td></tr><tr><td>Test Rate: 100%, With Pass Rate: 97.38%</td><td></td><td></td><td></td><td></td><td></td><td></td></tr></tbody></table>

### Detailed Test metrics

Below are the detailed test metrics by performing manual/automation testing. The project metrics are derived from Defect density, Test coverage, Test execution coverage, test tracking and efficiency.&#x20;

The various metrics that assist in test tracking and efficiency are as follows:

* Passed Test Cases Coverage: It measures the percentage of passed test cases. (Number of passed tests / Total number of tests executed) x 100
* Failed Test Case Coverage: It measures the percentage of all the failed test cases. (Number of failed tests / Total number of test cases executed) x 100

## Docker versions in rdt1 environment:

|  Images/Docker versions                                                                  |
| ---------------------------------------------------------------------------------------- |
| docker.io/mosipqa/apitest-pms:1.2.2.x                                                    |
| docker.io/mosipqa/partner-management-service:1.2.2.x                                     |
| docker.io/mosipqa/pmp-revamp-ui:1.2.2.x                                                  |
| docker.io/mosipqa/policy-management-service:1.2.2.x                                      |
| docker.io/mosipid/admintest:1.2.0.1                                                      |
| docker.io/mosipid/artifactory-server:1.4.1-ES                                            |
| docker.io/mosipid/commons-packet-service:1.2.0.3                                         |
| docker.io/mosipid/esignet:1.4.1                                                          |
| docker.io/mosipid/keycloak-init:1.2.0.1                                                  |
| docker.io/mosipid/mock-identity-system:0.9.3                                             |
| docker.io/mosipid/mock-relying-party-service:0.9.3                                       |
| docker.io/mosipid/mock-relying-party-ui:0.9.3                                            |
| docker.io/mosipid/oidc-ui:1.4.1                                                          |
| docker.io/mosipid/partner-onboarder:1.2.0.1                                              |
| docker.io/mosipid/postgres-init:1.2.0.1                                                  |
| docker.io/mosipid/regclient-keystore:1.3.0-beta.1                                        |
| docker.io/mosipid/softhsm:v2                                                             |
| docker.io/mosipint/kafka:3.2.1-debian-11-r9                                              |
| docker.io/mosipint/os-shell:12-debian-12-r46                                             |
| docker.io/mosipint/redis:7.0.5-debian-11-r25                                             |
| docker.io/mosipint/zookeeper:3.8.0-debian-11-r30                                         |
| harbor.mosip.net/mosipid/admin-service:1.2.1.2                                           |
| harbor.mosip.net/mosipid/admin-ui:1.2.0.1                                                |
| harbor.mosip.net/mosipid/apitest-auth:1.2.1.2-beta.1                                     |
| harbor.mosip.net/mosipid/apitest-idrepo:1.2.2.2                                          |
| harbor.mosip.net/mosipid/apitest-masterdata:1.2.1.3                                      |
| harbor.mosip.net/mosipid/apitest-prereg:1.2.0.3                                          |
| harbor.mosip.net/mosipid/apitest-resident:1.2.1.2                                        |
| harbor.mosip.net/mosipid/artifactory-server:1.2.0.4                                      |
| harbor.mosip.net/mosipid/authentication-internal-service:1.2.1.2-beta.1                  |
| harbor.mosip.net/mosipid/authentication-otp-service:1.2.1.2-beta.1                       |
| harbor.mosip.net/mosipid/authentication-service:1.2.1.2-beta.1                           |
| harbor.mosip.net/mosipid/biosdk-server:1.2.0.1                                           |
| harbor.mosip.net/mosipid/captcha-validation-service:0.1.0-beta.1                         |
| harbor.mosip.net/mosipid/config-server:1.1.2                                             |
| harbor.mosip.net/mosipid/consolidator-websub-service:1.2.0.1                             |
| harbor.mosip.net/mosipid/credential-request-generator:1.2.2.3                            |
| harbor.mosip.net/mosipid/credential-service:1.2.2.3                                      |
| harbor.mosip.net/mosipid/data-share-service:1.2.0.2                                      |
| harbor.mosip.net/mosipid/digital-card-service:1.2.0.1                                    |
| harbor.mosip.net/mosipid/dsl-orchestrator:1.2.1.0                                        |
| harbor.mosip.net/mosipid/dsl-packetcreator:1.2.1.0                                       |
| harbor.mosip.net/mosipid/hotlist-service:1.2.1.2                                         |
| harbor.mosip.net/mosipid/id-repository-identity-service:1.2.2.3                          |
| harbor.mosip.net/mosipid/id-repository-vid-service:1.2.2.3                               |
| harbor.mosip.net/mosipid/kernel-auditmanager-service:1.2.0.1                             |
| harbor.mosip.net/mosipid/kernel-auth-service:1.2.0.2                                     |
| harbor.mosip.net/mosipid/kernel-idgenerator-service:1.2.0.2                              |
| harbor.mosip.net/mosipid/kernel-keymanager-service:1.2.1.0                               |
| harbor.mosip.net/mosipid/kernel-masterdata-service:1.2.1.2                               |
| harbor.mosip.net/mosipid/kernel-notification-service:1.2.0.2                             |
| harbor.mosip.net/mosipid/kernel-otpmanager-service:1.2.0.1                               |
| harbor.mosip.net/mosipid/kernel-pridgenerator-service:1.2.0.2                            |
| harbor.mosip.net/mosipid/kernel-ridgenerator-service:1.2.0.2                             |
| harbor.mosip.net/mosipid/kernel-syncdata-service:1.2.1.2                                 |
| harbor.mosip.net/mosipid/keys-generator:1.2.0.1                                          |
| harbor.mosip.net/mosipid/masterdata-loader:1.2.0.1                                       |
| harbor.mosip.net/mosipid/mock-abis:1.2.0.2                                               |
| harbor.mosip.net/mosipid/mock-mv:1.2.0.2                                                 |
| harbor.mosip.net/mosipid/mock-smtp:1.0.0                                                 |
| harbor.mosip.net/mosipid/mosip-artemis-keycloak:1.2.0.1                                  |
| harbor.mosip.net/mosipid/mosip-file-server:1.2.0.1                                       |
| harbor.mosip.net/mosipid/partner-onboarder:1.2.0.1                                       |
| harbor.mosip.net/mosipid/pre-registration-application-service:1.2.0.3                    |
| harbor.mosip.net/mosipid/pre-registration-batchjob:1.2.0.3                               |
| harbor.mosip.net/mosipid/pre-registration-booking-service:1.2.0.2                        |
| harbor.mosip.net/mosipid/pre-registration-captcha-service:1.2.0.2                        |
| harbor.mosip.net/mosipid/pre-registration-datasync-service:1.2.0.3                       |
| harbor.mosip.net/mosipid/pre-registration-ui:1.2.0.1                                     |
| harbor.mosip.net/mosipid/print:1.2.0.1                                                   |
| harbor.mosip.net/mosipid/registration-client:1.2.0.2                                     |
| harbor.mosip.net/mosipid/registration-processor-common-camel-bridge:1.2.1.2              |
| harbor.mosip.net/mosipid/registration-processor-dmz-packet-server:1.2.1.2                |
| harbor.mosip.net/mosipid/registration-processor-landing-zone:1.2.1.2                     |
| harbor.mosip.net/mosipid/registration-processor-notification-service:1.2.1.2             |
| harbor.mosip.net/mosipid/registration-processor-registration-status-service:1.2.1.2      |
| harbor.mosip.net/mosipid/registration-processor-registration-transaction-service:1.2.1.2 |
| harbor.mosip.net/mosipid/registration-processor-reprocessor:1.2.1.2                      |
| harbor.mosip.net/mosipid/registration-processor-stage-group-1:1.2.1.2                    |
| harbor.mosip.net/mosipid/registration-processor-stage-group-2:1.2.1.2                    |
| harbor.mosip.net/mosipid/registration-processor-stage-group-3:1.2.1.2                    |
| harbor.mosip.net/mosipid/registration-processor-stage-group-4:1.2.1.2                    |
| harbor.mosip.net/mosipid/registration-processor-stage-group-5:1.2.1.2                    |
| harbor.mosip.net/mosipid/registration-processor-stage-group-6:1.2.1.2                    |
| harbor.mosip.net/mosipid/registration-processor-stage-group-7:1.2.1.2                    |
| harbor.mosip.net/mosipid/registration-processor-workflow-manager-service:1.2.1.2         |
| harbor.mosip.net/mosipid/resident-service:1.2.1.2                                        |
| harbor.mosip.net/mosipid/resident-ui:0.9.1                                               |
| harbor.mosip.net/mosipid/softhsm:v2                                                      |
| harbor.mosip.net/mosipid/websub-service:1.2.0.1                                          |
| harbor.mosip.net/mosipint/elasticsearch:7.17.2-debian-10-r4                              |
| harbor.mosip.net/mosipint/kibana:7.17.2-debian-10-r0                                     |
| harbor.mosip.net/mosipint/minio:2022.2.7-debian-10-r0                                    |
| harbor.mosip.net/mosipint/postgresql:14.2.0-debian-10-r70                                |
| harbor.mosip.net/mosipqa/activemq-artemis:1.1.5                                          |
| harbor.mosip.net/mosipqa/keycloak-init:develop                                           |
| mosipdev2/dsl-orchestrator:release-1.3.x                                                 |
| mosipdev2/dsl-packetcreator:release-1.3.x                                                |
| mosipid/commons-packet-service:1.2.0.3                                                   |
| mosipid/keycloak-init:1.2.0.1                                                            |
| mosipid/postgres-init:1.2.0.1                                                            |
| mosipid/regclient-keystore:1.3.0-beta.1                                                  |
| mosipid/softhsm:v2                                                                       |
| mosipqa/apitest-pms:1.2.2.x                                                              |
| mosipqa/partner-management-service:1.2.2.x                                               |
| mosipqa/policy-management-service:1.2.2.x                                                |

<br>
