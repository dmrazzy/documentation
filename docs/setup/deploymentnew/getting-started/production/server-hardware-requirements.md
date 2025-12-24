# Server Hardware Requirements

## **Overview**

This document provides a step-by-step guide for using the Resource Calculator to estimate the required server hardware for a country’s MOSIP deployment. The calculator delivers a high-level overview of the hardware components needed to support MOSIP services across Registration and Authentication.

{% hint style="info" %}
**Note**: Pre-Registration is not included in this version and will be enabled in an upcoming release.
{% endhint %}

### **MOSIP Deployment Components**

A typical MOSIP deployment includes the following components:

* ID Lifecycle Management (Issuance, Verification and Management modules/components)
* Pre-Registration
* Registration
* ID Authentication

The Resource Calculator focuses on estimating hardware capacity from the Registration through to Authentication.

### **Out of Scope**

The following areas are **not covered** by this version of the calculator:

* **KYC with OTP Verification** in the Authentication flow
* **Pre-Registration** processes and related hardware requirements
* Registration Packet processing once packet is uploaded

These will be considered in future enhancements of the calculator.

### **Key Assumptions**

To generate accurate estimates, the calculator is based on the following assumptions:

#### **Weightage Calculation**

* Weightage represents the percentage distribution of how often specific scenarios occur in real time.
  * Examples:
    * 40% of authentication requests use OTP.
    * 90% of registration requests are for new packet uploads.
    * 5% of sync requests once registration packet is created and ready to be uploaded
* These weightage play a critical role in performance estimation and calculator outputs.

{% hint style="info" %}
**Note:** Users are advised to define the weightage based on their specific requirements, re-run the performance tests, and then use the calculator to derive accurate resource estimates.
{% endhint %}

#### **Performance Metrics**

Performance testing is measured using the following metrics:

* **Sync Data:** Number of packet uploads per day (based on 22 working hours per day).
* **IDA:** Transactions per second (TPS) or authentications per hour.

#### **Scenario Scope**

* Only positive scenarios are considered.
* Negative scenarios requiring human involvement (e.g., Manual Adjudication) are excluded.

#### **External System Response Time**

* Systems such as ABIS are assumed to have a maximum response time of **300 ms**.

#### **Calculation Flexibility**

* The calculator can scale per-hour processing to:
  * Full-day operations
  * Limited working hours
  * Custom durations

#### **Load Multiplication Testing**

* Packet processing can be evaluated at **2 times** and **3 times** load to validate hardware scaling behaviour.

### **Important Considerations**

* Default values in the calculator are derived from insights gathered during MOSIP deployments in multiple countries.\
  Users may modify these values to match their actual or expected usage.
* The calculations provide approximate estimates based on internal testing.\
  Actual performance may vary depending on:
  * Workload characteristics
  * Infrastructure capacity
  * Hardware performance
  * Data complexity

### **Accessing the Calculator**

Refer to the link below to download and access the **Resource Calculator**.

{% file src="../../../../.gitbook/assets/resource-calculator-platform-release-1.3.x.xlsx" %}

The calculator provides two primary outputs:

1. **Total Resources Required**
2. **Total Time Required to Complete Registrations**

### **Calculator Structure**

Upon opening the calculator, users will find the following three sheets as shown in the below screen:

* **Summary:**\
  Primary input sheet where users provide parameters and view the overall resource requirement.
* **Registration Upload & SyncData:**\
  Displays resource requirements and calculations for packet upload and synchronization processes.
* **ID Authentication:**\
  Contains resource requirement details and calculations for handling authentication requests.

### **User Input**

Users can update values in the **Key Inputs** section as highlighted in the below screen, based on their deployment needs.\
All resource estimates will automatically adjust according to the data entered.



<!--

Old Content Below:

***



## Overview

MOSIP deployment is split into two distinct parts:

1. [ID lifecycle management](../../../../id-lifecycle-management/)
   * Pre-registration
   * Registration
2. [ID Authentication](../../../../id-lifecycle-management/identity-verification/id-authentication.md)

The server-side hardware estimates for the above are specified at a high level in terms of **compute** (Virtual CPU, RAM) and **storage** requirements. We provide estimates for [MOSIP core modules](https://github.com/mosip/mosip-infra/tree/release-1.2.0/deployment/v3/mosip) only. [External components](https://github.com/mosip/mosip-infra/tree/release-1.2.0/deployment/v3/external) are not in the scope. See [Exclusions](server-hardware-requirements.md#exclusions).

The variables that largely determine the hardware requirements are:

1. The population of the country
2. Rate of enrolment
3. Usage of foundation ID by various services

## Pre-registration

Refer to [Pre-registration Resource Calculator XLS](../../../../_files/pre-reg-resource-calculator-v2.xlsx)

Allow for 20% additional compute and storage for monitoring and any overheads.

## Registration (enrolment)

The registration compute resources are related to the max rate of enrolment desired. The processing throughput must match the enrolment rate to avoid a pile-up of pending registration packets.

The data here is based on actual field data of a MOSIP deployment.

Assumptions:

* Rate of enrolment: 216000 per day
* Average packet size: 2MB
* Biometric modalities: Finger, iris, face
* Pod replication as given here. (TBD)

### Compute requirements for registration

* Configuration of compute node: 12 VCPU, 64GB RAM, 64GB disk store.
* Number of nodes: 21

| Resource       | Per node | Nodes |    Total |
| -------------- | -------: | ----: | -------: |
| VCPU           |       12 |    21 |  **252** |
| RAM (GB)       |       64 |    21 | **1344** |
| Node disk (GB) |       64 |    21 | **1344** |

### Storage requirements for registration

Storage is dependent on the population of a country (i.e. the number of UINs to be issued). Storage requirements for various types of data are listed below.

| Data                                                                                                                                                                     |                                                                                               Storage                                                                                               | Comments                                                                                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Object Store ](https://github.com/mosip/mosip-infra/tree/1.2.0.1/deployment/v3/external/object-store/minio)(S3/Minio)                                                   |                                                                                 3200 GB/million packets/replication                                                                                 | Replication factor to be applied based on replication strategy                                                                                                                                                   |
| Postgres storage                                                                                                                                                         |                                                                                        30 GB/million packets                                                                                        | Includes all databases                                                                                                                                                                                           |
| [Landing zone](https://github.com/mosip/registration/blob/release-1.2.0/registration-processor/init/registration-processor-packet-receiver-stage/README.md#landing-zone) |                                                                                Unprocessed packets X avg packet size                                                                                | The size of landing zone depends on the estimated lag in packet processing and packet uploads. Once UINs are issued, the packets may be removed from the landing zone as a copy is already saved in Object Store |
| Logs (Elasticsearch)                                                                                                                                                     |                                                                                              80 GB/day                                                                                              | Logs maybe archived after, say, 2 weeks                                                                                                                                                                          |
| Monitoring (Prometheus)                                                                                                                                                  |                                                                                              1.2 GB/day                                                                                             |                                                                                                                                                                                                                  |
| Kafka                                                                                                                                                                    |                                                                                                  NA                                                                                                 | Resource allocation is part of cluster node                                                                                                                                                                      |
| ActiveMQ                                                                                                                                                                 |                                                                                                  NA                                                                                                 | Resource allocation depends on the deployment - standalone or part of cluster                                                                                                                                    |
| Redis                                                                                                                                                                    | <p>Single VM with,<br><strong>RAM</strong> = Cache size * 1.5<br><strong>VCPU</strong> = 4 to 16 depending on number of packets getting processed per min<br><strong>Hardware</strong>: Minimum</p> | Cache size = Avg. packet size \* No. of packets processed in a min \* Packet to be stored in cache for X mins                                                                                                    |

Allow for 20% additional compute and storage for monitoring and any overheads.

## ID authentication

Refer to [IDA Resource Calculator XLS](../../../../_files/ida-resource-calculator.xlsx)

Allow for 20% additional compute and storage for monitoring and any overheads.

## Exclusions

The compute and storage estimates for the following components are not included:

| Component                                                                            | Comments                                                                                                 |
| ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
| Postgres                                                                             | Only storage estimated above.                                                                            |
| Object store                                                                         | Only storage estimated above.                                                                            |
| Bio SDK                                                                              |                                                                                                          |
| [HSM](../../../../id-lifecycle-management/supporting-components/keymanager/hsm.md)   |                                                                                                          |
| [ABIS](../../../../id-lifecycle-management/supporting-components/biometrics/abis.md) |                                                                                                          |
| Antvirus (AV)                                                                        | Default scanner (ClamAV) in included, however, if you integrate your AV, the same needs to be estimated. |
| Load balancers                                                                       |                                                                                                          |
| External IAM (for Rancher)                                                           |                                                                                                          |
| Disaster recovery(DR)                                                                |                                                                                                          |

{% hint style="warning" %}
DR would significantly increase compute and storage requirements. It is expected that System Integrator works out the appropriate DR strategy and arrives at an estimate.
{% endhint %}

-->