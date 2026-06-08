# v1.2.1.1-RC.1

**Release Version:** v1.2.1.1-RC.1

**Release Type:** Release Candidate (RC)

**Release Date:** 1st June, 2026

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

<table><thead><tr><th width="183.4921875">Feature</th><th width="175.4140625">JIRA</th><th>Description</th></tr></thead><tbody><tr><td>System Performance Enhancement</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-42620">MOSIP-42620</a></td><td>Performance optimization and code-level enhancements to enable processing of approximately 500,000 packets per day.</td></tr></tbody></table>

### Repositories Released

<table><thead><tr><th width="369.12890625">Repositories</th><th>Tag Links</th></tr></thead><tbody><tr><td>mosip-config</td><td><a href="https://github.com/mosip/mosip-config/tree/v1.3.1-rc.1">v1.3.1-rc.1</a></td></tr><tr><td>mosip-data</td><td><a href="https://github.com/mosip/mosip-data/tree/v1.3.2-rc.1">v1.3.2-rc.1</a></td></tr><tr><td>commons</td><td><a href="https://github.com/mosip/commons/tree/v1.3.1-rc.1">v1.3.1-rc.1</a></td></tr><tr><td>khazana</td><td><a href="https://github.com/mosip/khazana/tree/v1.3.2-rc.1">v1.3.2-rc.1</a></td></tr><tr><td>key-manager</td><td><a href="https://github.com/mosip/keymanager/tree/v1.4.1-rc.1">v1.4.1-rc.1</a></td></tr><tr><td>registration</td><td><a href="https://github.com/mosip/registration/tree/v1.3.1-rc.1">v1.3.1-rc.1</a></td></tr><tr><td>mosip-ref-impl</td><td><a href="https://github.com/mosip/mosip-ref-impl/tree/v1.3.1-rc.1">v1.3.1-rc.1</a></td></tr><tr><td>id-repository</td><td><a href="https://github.com/mosip/id-repository/tree/v1.3.1-rc.1">v1.3.1-rc.1</a></td></tr><tr><td>id-authentication</td><td><a href="https://github.com/mosip/id-authentication/tree/v1.3.1-rc.1">v1.3.1-rc.1</a></td></tr><tr><td>mosip-mock-services</td><td><a href="https://github.com/mosip/mosip-mock-services/tree/v1.3.1-rc.1">v1.3.1-rc.1</a></td></tr><tr><td>packet -manager</td><td><a href="https://github.com/mosip/packet-manager/tree/v1.3.1-rc.1">v1.3.1-rc.1</a></td></tr><tr><td>durian</td><td><a href="https://github.com/mosip/durian/tree/v1.3.1-rc.1">v1.3.1-rc.1</a></td></tr><tr><td>audit-manager</td><td><a href="https://github.com/mosip/audit-manager/tree/v1.3.2-rc.1">v1.3.2-rc.1</a></td></tr><tr><td>biosdk-services</td><td><a href="https://github.com/mosip/biosdk-services/tree/v1.3.1-rc.1">v1.3.1-rc.1</a></td></tr><tr><td>websbub</td><td><a href="https://github.com/mosip/websub/tree/v1.3.2-rc.1">v1.3.2-rc.1</a></td></tr><tr><td>artifactory</td><td><a href="https://github.com/mosip/artifactory-ref-impl/tree/v1.3.2-rc.1">v1.3.2-rc.1</a></td></tr><tr><td>infra</td><td><a href="https://github.com/mosip/infra/tree/0.3.0-rc.1">v0.3.0-rc.1</a></td></tr><tr><td>k8s-infra</td><td><a href="https://github.com/mosip/k8s-infra/tree/v1.2.1.3-rc.1">v1.2.1.3-rc.1</a></td></tr><tr><td>mosip-infra</td><td><a href="https://github.com/mosip/mosip-infra/tree/v1.2.1.1-rc.1">v1.2.1.1-rc.1</a></td></tr></tbody></table>

### Known Issues

Below are a few key bugs marked as known issues for this release. Please click [here](https://mosip.atlassian.net/jira/software/c/projects/MOSIP/issues/?jql=project%20%3D%20MOSIP%20AND%20type%20%3D%20Bug%20AND%20labels%20%3D%20known_issue%20AND%20affectedVersion%20%3D%20PLA_1.2.1.0%20ORDER%20BY%20created%20DESC\&selectedIssue=MOSIP-43976) to view the complete list of known issues.

<table><thead><tr><th width="229.6328125">Issue</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://github.com/mosip/commons/issues/1827">MOSIP-1827</a></td><td>Composite repo environment variables hardcoded in deployment instead of ConfigMap/Secret</td></tr><tr><td><a href="https://github.com/mosip/mosip-functional-tests/issues/1951">MOSIP-1951</a></td><td>Hardcoding in DSL for checking config parameters using actator/env</td></tr></tbody></table>

### Compatible Modules

| Module/Repo                 | Compatible Version                                                             |
| --------------------------- | ------------------------------------------------------------------------------ |
| partner-management-services | [v1.2.2.3](https://github.com/mosip/partner-management-services/tree/v1.2.2.3) |
| resident-ui                 | [v0.9.0](https://github.com/mosip/resident-ui/releases/tag/v0.9.0)             |
| eSignet                     | [v1.4.1](https://github.com/mosip/esignet/tree/v1.4.1)                         |
| registration-client         | [v1.2.0.2](https://github.com/mosip/registration-client/tree/v1.2.0.2)         |
| mosip-automation-test       | [v1.4.0](https://github.com/mosip/mosip-automation-tests/tree/release-1.4.x)   |

### Dependency Matrix

<table><thead><tr><th width="305.31640625">Component</th><th>Version</th><th>Helm Chart version (If applicable)</th></tr></thead><tbody><tr><td>Key Cloak</td><td>v7.1.18</td><td></td></tr><tr><td><p>Kafka</p><p>Kafka-zookeeper</p></td><td><p>3.2.1 ( Both components deployed with the same helm chart)</p><p>3.8.0</p></td><td>18.3.1</td></tr><tr><td>Mock-SMTP</td><td>1.0.0</td><td>1.0.0</td></tr><tr><td>ActiveMQ</td><td>2.39.0</td><td>0.0.3</td></tr><tr><td>Minio</td><td>2025.2.28-debian-12-r1</td><td>15.0.6</td></tr><tr><td>Redis (used only if eSignet exists)</td><td>v7.0.5</td><td>17.3.14</td></tr><tr><td>Postgres</td><td>v16</td><td>13.1.5</td></tr><tr><td>Softhsm (Recommended to use only in sandbox environment)</td><td>v2</td><td>12.0.1</td></tr><tr><td>clamav</td><td>v1.2</td><td>3.1.0</td></tr></tbody></table>

### Documentation

* [QA Report](test-report.md)
