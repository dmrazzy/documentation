# 0.1.0-beta Infra

**Release Version:** v0.1.0 beta

**Release Type:** Beta Release

**Release Date:** Coming Soon

This **v0.1.0 beta release** marks the launch of **MOSIP Rapid Deployment Infrastructure**, introducing a **unified infrastructure repository** that simplifies, accelerates, and secures MOSIP deployments. This release shifts from a fragmented, manual, and multi-repository model to a single repository-driven, fully automated deployment approach.

It prioritizes enhancements in **deployment velocity, automation, reproducibility, and security**, while significantly lowering operational complexity.

Click [here](https://github.com/mosip/infra/tree/release-0.1.0) to know more about this new enhancement.

### Major Areas of Work

* **Unified Infrastructure Repository**
  * End-to-end MOSIP deployment via a single repository.
  * Three-step simplified deployment model.
  * GitHub Actions-driven CI/CD automation.
* **Infrastructure as Code (IaC)**
  * Terraform modules with multi-cloud support.
  * GPG-encrypted state management for secure, reproducible infrastructure.
* **Application Management**
  * Declarative deployment with Helmsman DSF.
  * Consistent state management and easy environment replication.
* **Automation Enhancements**
  * GitHub Actions workflows with retries and rollbacks.
  * One-click deployment reducing time from days to hours.
* **Security Enhancements**
  * WireGuard VPN, encryption, secret integration with GitHub Actions.
  * Built-in zero-trust model and best practices embedded.
* **Infrastructure Modernization**
  * RKE2 Kubernetes for scalable, modern orchestration.
  * Reusable Terraform modules and declarative deployments.

### User Story Released

* **Rapid Deployment Model** \
  A streamlined infrastructure approach designed to automated deployment of MOSIP components across environments. [#MOSIP-42197](https://mosip.atlassian.net/browse/MOSIP-42197)

### Repository Released

| Repositories | Tags Released |
| ------------ | ------------- |
| infra        | 0.1.0-beta    |

### Known Issues

| JIRA                                                                                                                                                       | Description                                                  |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| [MOSIP-43157](https://mosip.atlassian.net/browse/MOSIP-43157)                                                                                              | Terraform apply fails when branch protection rule is enabled |
| [MOSIP-41863](https://mosip.atlassian.net/browse/MOSIP-41863)                                                                                              | Partner onboarding failing at first run                      |
| [MOSIP-43160](https://mosip.atlassian.net/browse/MOSIP-43160?atlOrigin=eyJpIjoiYThkODdlYTZmNWZhNDhlZWI1ZGZhZmE4M2Q1YzkwNDMiLCJwIjoiamlyYS1zbGFjay1pbnQifQ) | Docker unauthenticated pull rate limit blocks deployment     |

### Documentation

* Documentation for Deployment (To be updated)
* QA Report (To be updated)

