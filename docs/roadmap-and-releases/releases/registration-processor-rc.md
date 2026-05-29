# Registration Processor RC

**Release Version:** v1.2.2.0-RC

**Release Type:** Release Candidate (RC)

**Release Date:** Coming Soon!

### Overview

This Release Candidate (RC) introduces performance and stability enhancements to the Registration Processor ecosystem with a focus on improving packet processing throughput, optimizing resource utilization, and reducing latency across the processing pipeline.

The release includes code-level fine-tuning, infrastructure optimizations, and processing-stage improvements to improve scalability and operational stability under higher workloads.

As part of the performance validation carried out for this release, the system is now able to process approximately **3000 – 4000 packets per 10 minutes** in controlled performance test environments.

### Major Areas of Work

#### JVM & Resource Optimization

* Improved JVM memory and garbage collection tuning to reduce processing delays and improve pod stability under high throughput scenarios.
* Enhanced Kubernetes resource calibration to ensure better memory utilization and stable workload execution.

#### Packet Processing Performance Improvements

* Optimized packet validation and classification stages through reduced duplicate service calls and parallel execution of independent operations.
* Improved processing efficiency for biometric validation, document validation, and metadata retrieval.
* Reduced redundant Packet Manager and database calls across multiple stages to improve overall latency.

#### Database & Reprocessing Optimization

* Enhanced database performance for packet reprocessing through indexing improvements, reducing query execution time and improving retry handling efficiency.

#### Workflow & ABIS Throughput Improvements

* Improved ABIS middleware processing by offloading heavy response handling to worker threads.
* Optimized workflow internal actions through parallel execution of independent service operations.

#### Operational Stability Improvements

* Introduced asynchronous logging and worker pool fine-tuning capabilities to improve throughput and reduce processing overhead during peak load.
* Improved worker pool handling to allow better concurrency management across processing stages.
* Multiple code-level refinements to improve reliability and resilience during large-scale processing.

### Stories Released

| **Feature**                    | **Description**                                                                                                     | **JIRA**                                                      |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| System Performance Enhancement | Performance optimization and code-level enhancements to enable processing of approximately 500,000 packets per day. | [MOSIP-42620](https://mosip.atlassian.net/browse/MOSIP-42620) |

### Repositories Released

| Repository          | Tag Version   |
| ------------------- | ------------- |
| registration        | v1.3.1-rc.1   |
| mosip-config        | v1.3.1-rc.1   |
| packet-manager      | v1.3.1-rc.1   |
| mosip-data          | v1.3.1-rc.1   |
| commons             | v1.3.1-rc.1   |
| khazana             | v1.3.1-rc.1   |
| keymanager          | v1.4.1-rc.1   |
| mosip-ref-impl      | v1.3.1-rc.1   |
| id-repository       | v1.3.1-rc.1   |
| id-authentication   | v1.3.1-rc.1   |
| mosip-mock-services | v1.3.1-rc.1   |
| durian              | v1.3.1-rc.1   |
| audit-manager       | v1.3.1-rc.1   |
| biosdk-services     | v1.3.1-rc.1   |
| websub              | v1.3.1-rc.1   |
| artifcatory         | v1.3.1-rc.1   |
| infra               | v0.3.0-rc.1   |
| k8s-infra           | v1.2.1.3-rc.1 |
| mosip-infra         | v1.2.1.1-rc.1 |

### Known Issues

Below are a few key bugs marked as known issues for this release. Please click [here](https://mosip.atlassian.net/jira/software/c/projects/MOSIP/issues/?jql=project%20%3D%20MOSIP%20AND%20type%20%3D%20Bug%20AND%20labels%20%3D%20known_issue%20AND%20affectedVersion%20%3D%20PLA_1.2.1.0%20ORDER%20BY%20created%20DESC\&selectedIssue=MOSIP-43976) to view the complete list of fixes.

| **JIRA** | **Description** |
| -------- | --------------- |
|          |                 |
|          |                 |

### Compatible Modules

| Module/Repo                 | Compatible Version |
| --------------------------- | ------------------ |
| partner-management-services | v1.2.2.3           |
| resident-ui                 | v0.9.0             |
| eSignet                     | v1.4.1             |
| registration-client         | v1.2.0.2           |
| mosip-automation-test       | v1.4.0             |

### Dependency Matrix

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

### Documentation

* QA Report
* Performance Test Report
* Known Issues (if any)
