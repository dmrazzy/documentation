# Android Registration Client v1.1.0

#### Release Name: Android Registration Client v1.1.0

**Release Version:** 1.1.0

**Release Type:** Minor Release

**Release Date:** <mark style="color:red;">**Coming Soon!**</mark>

### Overview

Android Registration Client v1.1.0 focuses on platform modernization, security hardening, and interoperability improvements. This release introduces Java 21 support across Android Registration Client services, ensuring compatibility with the latest MOSIP platform releases and providing a foundation for future enhancements.

In addition, the release addresses critical security findings identified during security assessments and improves biometric capture request handling by ensuring mandatory transaction metadata is consistently transmitted. These updates strengthen platform reliability, security, and standards compliance while improving integration with biometric devices and backend services.

#### 1. Java 21 Migration

This release upgrades Android Registration Client services from Java 11 to Java 21, aligning the application with the latest MOSIP platform requirements and technology stack.

The migration enables the platform to leverage long-term support (LTS) improvements, enhanced performance, improved memory management, and modern Java language capabilities. It also ensures continued compatibility with future MOSIP releases and supported infrastructure components.

By adopting Java 21, the Android Registration Client benefits from improved maintainability, enhanced security updates, and a more future-ready architecture, reducing technical debt and simplifying long-term support.

#### 2. Capture Request Improvements

Enhancements have been made to biometric capture request processing to ensure that mandatory fields such as Transaction ID and Capture Time are consistently included in requests sent by the Android Registration Client.

Previously, under certain scenarios, null values could be transmitted for these fields, resulting in interoperability issues with downstream biometric services and integrations. This update ensures complete and standards-compliant capture requests.

The enhancement improves transaction traceability, strengthens auditability, and ensures reliable communication between registration devices and biometric service providers.

#### 3. Security Enhancements

Several security vulnerabilities identified during security testing have been addressed as part of this release.

The fixes include improvements to SDK matching validation mechanisms and the elimination of clear-text traffic vulnerabilities. These updates strengthen communication security, reduce exposure to potential attacks, and improve overall platform resilience.

With these enhancements, the Android Registration Client provides a more secure operating environment while maintaining compliance with MOSIP security standards and best practices.

### Story Progress & Updates

| JIRA        | Description                                                                                                                             |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| MOSIP-44993 | Android Registration Client now ensures Transaction ID and Capture Time are populated and sent correctly in biometric capture requests. |
| #828        | Upgrade Java version from Java 11 to Java 21 for Android Registration Client services.                                                  |

### Bugs Fixed

Below is the list of bug fixes included as part of the Android Registration Client v1.1.0 release.

| JIRA        | Description                                                                                                        |
| ----------- | ------------------------------------------------------------------------------------------------------------------ |
| MOSIP-44993 | Android Registration Client was sending null Transaction ID and Capture Time values in biometric capture requests. |
| MOSIP-42301 | Security testing: ARC – Match SDK validation issue fixed.                                                          |
| MOSIP-42281 | Security testing: ARC – Clear text traffic vulnerability fixed.                                                    |

### Known Issues

To view the list of known issues, refer [**here**](https://github.com/mosip/android-registration-client/issues?q=is%3Aissue%20state%3Aopen%20label%3Abug).

### Repository Released

| Repositories                | Tags Released |
| --------------------------- | ------------- |
| android-registration-client | v1.1.0        |

### Compatible Platform/Module

The following table outlines the tested and certified compatibility of Android Registration Client v1.1.0 with other modules.

| Platform/Module | Version |
| --------------- | ------- |
| MOSIP           | 1.2.1.0 |

### Build and Deploy

To access the build and deployment instructions, refer to the [Developer Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-developer-guide).

### Configurations

For details related to Android Registration Client configurations, refer to the [Configuration Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-configuration).

### User Guide

To learn more about the available features, workflows, and user interface, refer to the [Android Registration Client User Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-user-guide).
