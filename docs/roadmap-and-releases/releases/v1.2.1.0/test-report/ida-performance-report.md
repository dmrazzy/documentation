# IDA Performance Report

## **Overview**

Identity life-cycle workflow underwent performance tuning in this release. Identity life-cycle consists of Registrations, Credential Processing and ID Authentication.

This report will cover only ID Authentication.

This Performance Report provides a comprehensive analysis of the system’s responsiveness, reliability, and scalability for above mentioned use case. It captures key performance metrics such as latency, throughput, resource utilization and memory consumption. This report is designed to help stakeholders understand how MOSIP v1.2.1.0 performs in real-world scenarios, its potential bottlenecks and recommendation for improvements.

**For additional details on functional enhancements, refer to**: [Release Notes 1.2.1.0](https://mosip.atlassian.net/wiki/spaces/ENGG/pages/2285699531)

## **Summary**

A load test was conducted to assess the performance and stability of the ID Authentication APIs.

Over the 10‑hour test window, the system successfully processed 1.8 million authentications originating from 90,000 unique identities, maintaining a steady throughput of 50 transactions per second. Throughout the test, all API response times consistently remained well below the 1‑second SLA target.

Resource Calculator is provided to help client countries estimate their hardware requirement to achieve similar performance at a larger scale.

No performance degradation or bottlenecks were observed throughout the test duration. Based on the results, this module is assessed to be stable, performant, and ready for release.

## **Test Environment**

### **Deviation from the default**

* For performance test purpose, OTP Manager Service was modified to return a static OTP post dynamic OTP generation. The change was made to support for performance test script without impacting the performance test.

&#x20;

### **Software (Under Test)**

Following ‘Images’ were under the scope of ‘Performance Testing’:

#### **Modules Segregation**

| **Image ID**                                                 | **Branch Name**            | <p><strong>Comments</strong></p><p> </p>                                                                                |
| ------------------------------------------------------------ | -------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| mosipqa/authentication-service:1.3.x                         |  release-1.3.x             |                                                                                                                         |
| mosipqa/authentication-internal-service:1.3.x                |  release-1.3.x             |                                                                                                                         |
| mosipqa/authentication-otp-service:1.3.x                     |  release-1.3.x             |                                                                                                                         |
| mosipqa/biosdk-server:1.3.x                                  |  release-1.3.x             |  Bio SDK service is configured with Mock SDK. It is being used for quality, extraction and matching just for this test. |
| mosipqa/kernel-notification-service:1.3.x                    |  release-1.3.x             |  Mock SMTP has been used for mail notification for this test.                                                           |
| mosipdev/kernel-otpmanager-service:MOSIP-42334-release-1.3.x |  MOSIP-42334-release-1.3.x |  This image is based on release-1.3.x with dynamic OTP generation disabled.                                             |
| mosipqa/kernel-auditmanager-service:1.3.x                    | release-1.3.x              |                                                                                                                         |
| mosipqa/kernel-keymanager-service:1.3.x                      | release-1.3.x              | This service was only used for preparation.                                                                             |
| mosipqa/credential-service:1.3.x                             | release-1.3.x              | This service was only used for preparation.                                                                             |
| mosipqa/credential-request-generator:1.3.x                   | release-1.3.x              | This service was only used for preparation.                                                                             |
| mosipqa/id-repository-identity-service:1.3.x                 | release-1.3.x              | This service was only used for preparation.                                                                             |
| mosipqa/id-repository-vid-service:1.3.x                      | release-1.3.x              | This service was only used for preparation.                                                                             |

#### **Test Base Data**

Performance data load has been populated before the run to ensure realistic results.

| **DB**     | **Table Name**  | <p><strong>Number Of Records</strong><br><strong>(Target)</strong></p> | <p><strong>Number Of Records</strong><br><strong>(Tested with)</strong></p> |
| ---------- | --------------- | ---------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| mosip\_ida | identity\_cache | 1,000,000                                                              | 875,711                                                                     |

### **Test Design**

* **Test Duration**: 10-Hours
* **Test Type: Load Test**
* **Ramp Up: 2 mins**
* **Total VUser Load: 55**
* **Tools Used**:
  * JMeter for user load simulation.
  * MOSIP Packet Creator for registration packet generation.

#### **Workload Model**

| **Scenario Name**                                                     | **Module Name** | **API Endpoint**                                                      | **SLA (ms)** | <p><strong>Weightage/</strong></p><p><strong>Load Distribution</strong></p> | **Users**          | **Throughput (TPS)** | **Target Volume**        |
| --------------------------------------------------------------------- | --------------- | --------------------------------------------------------------------- | ------------ | --------------------------------------------------------------------------- | ------------------ | -------------------- | ------------------------ |
| Authentication with OTP                                               | IDA             |  idauthentication/v1/otp/${mispLicenseKey}/${rpPartnerId}/${rpApiKey} |  1000        | <p> </p><p> 40%</p>                                                         | <p> </p><p> 21</p> | <p> </p><p> 20</p>   | <p> </p><p> 7,20,000</p> |
| idauthentication/v1/auth/${mispLicenseKey}/${rpPartnerId}/${rpApiKey} |  1000           | 7,20,000                                                              |              |                                                                             |                    |                      |                          |
| Authentication with Biometrics                                        | IDA             | idauthentication/v1/auth/${mispLicenseKey}/${rpPartnerId}/${rpApiKey} | 1000         | 25%                                                                         | 14                 | 12.5                 | 4,50,000                 |
| Authentication with Demographics                                      | IDA             | idauthentication/v1/auth/${mispLicenseKey}/${rpPartnerId}/${rpApiKey} | 1000         | 15%                                                                         | 9                  | 7.5                  | 2,70,000                 |
| Ekyc with Biometrics                                                  | IDA             | idauthentication/v1/kyc/${mispLicenseKey}/${rpPartnerId}/${rpApiKey}  | 1000         | 20%                                                                         | 11                 | 10                   | 3,60,000                 |

## **Test Result**

#### **Performance test execution results**

| **Application Name** | ID Authentication                     |
| -------------------- | ------------------------------------- |
| **Test Duration**    | 12/12/25, 1:30 AM- 11:30AM (10-Hours) |
| **Number of users**  | 50                                    |
| **Status**           | GREEN                                 |

&#x20;

### **Test Report**

| <p>Scenario<br>Name</p>                  | Transaction Name                                  | API Endpoint               | 50 TPS 55 VUsers |                                                      |                                                          |                                                              |                                                         |             |
| ---------------------------------------- | ------------------------------------------------- | -------------------------- | ---------------- | ---------------------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------- | ----------- |
| Date: 2025/12/12 (10 Hours)              |                                                   |                            |                  |                                                      |                                                          |                                                              |                                                         |             |
|                                          |                                                   |                            | **# Samples**    | <p><strong>Min</strong><br><strong>(ms)</strong></p> | <p><strong>Average</strong><br><strong>(ms)</strong></p> | <p><strong>95% Line</strong></p><p><strong>(ms)</strong></p> | <p><strong>Max</strong></p><p><strong>(ms)</strong></p> | **Error %** |
| S01 T01 Authentication with OTP          | S01 T01 Authentication with OTP Endpoint          |  idauthentication/v1/otp/  | 7,19,832         | 37.0                                                 | 87.1                                                     | 120.0                                                        | 2910.0                                                  | 1.4%        |
| S01 T02 Send OTP Endpoint                | ${mispLicenseKey}/                                |                            | 7,19,855         | 24.0                                                 | 143.0                                                    | 224.0                                                        | 3448.0                                                  | 1.4%        |
| S02 T01 Authentication with Biometrics   | S02 T01 Authentication with Biometrics Endpoint   | ${rpPartnerId}/${rpApiKey} | 4,68,075         | 74.0                                                 | 175.3                                                    | 217.0                                                        | 4344.0                                                  | 0.7%        |
| S03 T01 Authentication with Demographics | S03 T01 Authentication with Demographics Endpoint | idauthentication/v1/auth/$ | 2,87,945         | 38.0                                                 | 81.1                                                     | 112.0                                                        | 3291.0                                                  | 0.5%        |
| S04 T01 Ekyc with Biometrics             | S04 T01 Ekyc with Biometrics Endpoint             | {mispLicenseKey}/          | 3,59,781         | 76.0                                                 | 197.1                                                    | 245.0                                                        | 5183.0                                                  | 0.7%        |

### &#x20;**High Level Observations:**

* The response time was well under the 1 second target (90<sup>th</sup> percentile was less than 250ms).
* We were able to perform 50 authentications per second for 10 hours using 90,000 unique identities.
* No degradation in response time was observed throughout the long test run.
* \~1% of overall authentication failed. This was due to inaccuracies in our list of 90,000 identities and not due to performance defects.

### **Metrics**

#### **Response Time over Time**

**\[Image]**



**Observations:**

* 4 out of 5 APIs displayed consistent response time throughout the test.
* “Send OTP endpoint” exhibited some response‑time variability during the test, but it remained largely within the 1‑second target. This does not indicate a significant performance issue, though there may be opportunities for further tuning.

&#x20;

#### **Resource Utilization**

For sake of brevity of the report, resource metrics are grouped by Namespace and all services in Namespace is displayed even if they did not actively participate in the test.

**IDA**&#x20;

**\[Image]**



**BIOSDK**&#x20;

**\[Image]**

**KERNEL**&#x20;

**\[Image]**

**Observations:**

* Idgenerator service within Kernel namespace showed a brief spike (<8mins). This module is out of scope for this test. There was no measurable impact on overall system performance and throughput. &#x20;

&#x20;

### **Resource Calculator**

**(Link To MOSIP Doc)**

### **Resource level configuration**

Following configuration was used for the performance test.

| **IDA Performance Run Preparation services** |                      |               |                |                 |                |      |      |   |
| -------------------------------------------- | -------------------- | ------------- | -------------- | --------------- | -------------- | ---- | ---- | - |
| **NameSpace**                                | **Deployment**       | **Resources** |                | **Java Option** | **No.of Pods** |      |      |   |
|                                              |                      | **Limits**    | **Requests**   | **Min**         | **Max**        |      |      |   |
|                                              |                      | **CPU(m)**    | **Memory(Mi)** | **CPU(m)**      | **Memory(Mi)** |      |      |   |
| IDA                                          | IDA-AUTH             | 2000          | 5000           | 2000            | 5000           | 3000 | 3000 | 1 |
| IDA                                          | IDA-INTERNAL         | 500           | 6000           | 500             | 6000           | 2250 | 2250 |   |
| IDA                                          | IDA-OTP              | 500           | 4000           | 500             | 4000           | 3250 | 3250 | 1 |
| WEBSUB                                       | WEBSUB-CONSOOLIDATOR | 200           | 1500           | 200             | 1500           | 750  | 750  | 1 |
| WEBSUB                                       | WEBSUB               | 2000          | 6000           | 2000            | 6000           | 5250 | 5250 | 1 |
| biosdk                                       | biosdk-service       | 1500          | 4000           | 1500            | 4000           | 3250 | 3250 | 1 |
| kafka                                        | kafka                | 2000          | 5000           | 2000            | 5000           |      |      | 5 |
| KEYMANAGER                                   | KEYMANAGER           | 2000          | 6000           | 2000            | 6000           | 4250 | 4250 | 4 |
| KERNEL                                       | NOTIFIER             | 1500          | 5000           | 1500            | 5000           | 1250 | 1250 | 3 |
| KERNEL                                       | OTPMANAGER           | 500           | 3100           | 500             | 3100           | 800  | 800  | 3 |
| KERNEL                                       | AUDITMANAGER         | 2000          | 5000           | 2000            | 5000           | 3250 | 3250 | 2 |
| IDREPO                                       | CREDENTIAL           | 600           | 3800           | 600             | 3800           | 3050 | 3050 | 3 |
| IDREPO                                       | CREDENTIALREQUEST    | 1000          | 8000           | 1000            | 8000           | 2900 | 2900 | 3 |
| IDREPO                                       | IDENTITY             | 2000          | 10000          | 2000            | 10000          | 3850 | 3850 | 4 |
| IDREPO                                       | VID                  | 600           | 5001           | 600             | 5001           | 2250 | 2250 | 3 |

&#x20;

&#x20;

| **IDA Performance Run Required services** |                |            |                |                 |                |      |      |   |
| ----------------------------------------- | -------------- | ---------- | -------------- | --------------- | -------------- | ---- | ---- | - |
| **NameSpace**                             | **Deployment** |            | **Resources**  | **Java Option** | **No.of Pods** |      |      |   |
| **Limits**                                | **Requests**   | **Min**    | **Max**        |                 |                |      |      |   |
| **CPU(m)**                                | **Memory(Mi)** | **CPU(m)** | **Memory(Mi)** |                 |                |      |      |   |
| IDA                                       | IDA-AUTH       | 2000       | 5000           | 2000            | 5000           | 3000 | 3000 | 4 |
| IDA                                       | IDA-OTP        | 1000       | 4000           | 500             | 4000           | 3250 | 3250 | 2 |
| biosdk                                    | biosdk-service | 1500       | 4000           | 1500            | 4000           | 3250 | 3250 | 3 |
| KERNEL                                    | NOTIFIER       | 1500       | 5000           | 1500            | 5000           | 1250 | 1250 | 3 |
| KERNEL                                    | OTPMANAGER     | 500        | 3100           | 500             | 3100           | 800  | 800  | 3 |
| KERNEL                                    | AUDITMANAGER   | 2000       | 5000           | 2000            | 5000           | 3250 | 3250 | 2 |

&#x20;

## **Performance Analysis**

### **KPI**

The performance test was conducted to ensure this release is able to achieve stable rate of 50 authentication per second. Test achieved 50tps for 10 hours without any degradations.

Additionally, the hardware resources used to achieve this rate of transaction per second (TPS) has been shared via Resource Calculator document. It can be used to estimate resources required for higher processing as per specific needs of client countries.

### **Bottlenecks**

Based on the defined scope and the results of this performance test, no major bottlenecks or performance‑limiting issues were observed. All services operated within acceptable resource boundaries, and all API response times remained well below the 1‑second SLA target.

### **Recommendation**

* The system demonstrated stable performance throughout the 10‑hour run, successfully processing achieving a consistent throughput of 50 authentications per second during a 10-hour run without any degradation. This confirms readiness for production‑scale workloads.
* For capacity planning, it is recommended to use the Resource Calculator to estimate infrastructure requirements for higher throughput or larger population sizes.
* Any deviations between the performance test environment, workload model and the default production configuration should be considered when planning resource allocation for real‑world deployments

&#x20;

## **Conclusion**

MOSIP v1.2.1.0 is able to reliably process authentication at the rate of 50 transaction per second (TPS) for 10-hours. The Resource Calculator can be used to estimate the hardware requirements for higher TPS rate for larger population.

&#x20;
