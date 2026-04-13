# Android Registration Client v1.0.1

**Release Name**: Android Registration Client v1.0.1

**Release version**: 1.0.1

**Support**: Beta Release

**Release Date**: 10th April, 2026

### **Overview**

Android Registration Client v1.0.1 introduces significant improvements to system flexibility, auditability, and security. This release focuses on three key areas: enhanced configurability that reduces hardcoded values for easier deployment across different environments, comprehensive audit logging for improved transparency and compliance tracking, and operator biometric validation to prevent unauthorized data capture during applicant registration. These enhancements strengthen the system's maintainability, accountability, and data integrity while supporting diverse implementation requirements.

1.  **Configurability Improvements:** This release improves configurability by moving several previously fixed system values into configuration settings. As a result, the platform can adapt more easily to different deployment environments and operational requirements without requiring source code changes.

    With these updates, administrators can easily adjust system behavior through configuration settings, making it quicker to respond to changing needs. This reduces reliance on development teams for minor updates and helps streamline operational workflows.

    Overall, this improvement increases system flexibility, scalability, and maintainability, ensuring smoother deployments and easier long-term management across diverse implementations.
2.  [**Operator Biometric Restriction**](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features#id-11.-match-sdk)**:** A new validation has been implemented to ensure that an operator’s biometrics are not accepted during applicant registration. This prevents scenarios where operator data could be mistakenly or intentionally captured as part of an applicant’s record.

    By enforcing this rule, the system ensures that only the applicant’s biometric information is recorded, maintaining the accuracy and authenticity of identity data. This is critical for preserving the integrity of the registration process.

    This enhancement also strengthens security and compliance by reducing the risk of misuse and aligning with best practices for handling sensitive biometric information.
3.  **Audit Enhancements:** Audit logging has been introduced for key system actions and workflows, improving overall transparency and traceability. Critical operations are now recorded, providing clear visibility into system usage and user activities. These logs enable faster debugging and issue resolution by offering a detailed history of events. Teams can track what actions were performed, when they occurred, and by whom, making it easier to identify and resolve problems.

    In addition, the audit capability supports compliance and governance requirements by ensuring that all significant actions are documented. This strengthens accountability and builds trust in the system.

**Note**: Compatible with[ MOSIP version 1.2.0](https://docs.mosip.io/1.2.0/releases/release-notes)

**Note**: **NOT** Compatible with MOSIP Version 1.3.0

### Stories

<table><thead><tr><th width="188.359375">JIRA</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://mosip.atlassian.net/browse/RCF-351">RCF-351</a></td><td>As an Operator, my biometrics should not be accepted when I am registering an applicant.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1301">RCF-1301</a></td><td>We had a few values that were hardcoded which has now made to be configurable.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1278">RCF-1278</a></td><td>Editable Cron Job in Scheduled Job Settings.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1277">RCF-1277</a></td><td>Audits have been added to be in parity with Desktop Registration Client.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1275">RCF-1275</a></td><td>Added syncs and batch jobs.</td></tr></tbody></table>

### Bugs Fixed

Below is the list of bug fixes as part of the 1.0.1 release, To get the complete list of bug fixes please refer [**here**](https://mosip.atlassian.net/issues?jql=text%20~%20%22parent%3Drcf-31%20and%20fixVersion%3Drcf-1.0.1%20and%20issuetype%20%3D%20bug%22%20OR%20summary%20~%20%22parent%3Drcf-31%20and%20fixVersion%3Drcf-1.0.1%20and%20issuetype%20%3D%20bug%22%20OR%20\(parent%20%3D%20RCF-31%20AND%20fixVersion%20%3D%20rcf-1.0.1%20AND%20issuetype%20%3D%20Bug\)%20ORDER%20BY%20created%20DESC).



<table><thead><tr><th width="199.8515625" valign="top">JIRA</th><th valign="top">Description</th></tr></thead><tbody><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-1268">RCF-1268</a></td><td valign="top">ARC: While packet creating Threshold values and score values are giving same, continue button is not enable in biometric page.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-1264">RCF-1264</a></td><td valign="top">ARC: In the ARC Smtp Notifications are not displaying for all the packets.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-1256">RCF-1256</a></td><td valign="top">ARC: In the auto logged out, while in the offline mode the warning pop up time start after 5 minutes.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-1254">RCF-1254</a></td><td valign="top">ARC: In the global config settings page all the pop up messages are not getting in logged in languages.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-1251">RCF-1251</a></td><td valign="top">ARC: In the global settings page after changing the local values incorrect prompt message is displaying.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-1249">RCF-1249</a></td><td valign="top">ARC: Device is not getting detected when device status is "busy/not-ready/not-registered" in device settings page.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-1220">RCF-1220</a></td><td valign="top">ARC: In the local values when we edit/update the unlimited allowed values should get the prompt message.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-1218">RCF-1218</a></td><td valign="top">ARC: Search/Filter option is not displaying in the Global Config settings screen.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-1002">RCF-1002</a></td><td valign="top">ARC: Not receiving any error message when trying to fetch, also able to generate UIN multiple times using an already consumed packet.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/MOSIP-34477">MOSIP-34477</a></td><td valign="top">Security testing: ARC fake Identity Generation issue.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/MOSIP-34400">MOSIP-34400</a></td><td valign="top">Security testing: ARC- user verification for certain actions.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/MOSIP-34361">MOSIP-34361</a></td><td valign="top">Security testing: ARC- CBC issue.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/MOSIP-34359">MOSIP-34359</a></td><td valign="top">Security testing: ARC- utilization of local authentication mechanisms.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/MOSIP-34357">MOSIP-34357</a></td><td valign="top">Security testing: ARC - data protection in case of loss of device.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/MOSIP-34355">MOSIP-34355</a></td><td valign="top">Security testing: ARC- session logout issue.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-755">RCF-755</a></td><td valign="top">ARC: Server status is not moving further after "Packet receiver" even after UIN generation.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-665">RCF-665</a></td><td valign="top">RCF: Getting "IDA Response fetch failed" error message while onboarding/Updating biometrics user.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-418">RCF-418</a></td><td valign="top">RCF: After entered the age still validation message not removing in DOB like address/phone number.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-373">RCF-373</a></td><td valign="top">RCF - Incorrect error message on login screen.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-183">RCF-183</a></td><td valign="top">RCF: Should be getting an appropriate error message, If the device/user is not onboarded.</td></tr><tr><td valign="top"><a href="https://mosip.atlassian.net/browse/RCF-119">RCF-119</a></td><td valign="top">Add machine name in the auth request.</td></tr></tbody></table>

### Known Issues

To view the list of known issues, refer [**here**](https://mosip.atlassian.net/issues/?jql=parent%3Drcf-31%20and%20issuetype%3Dbug%20and%20status%20not%20in%20%28closed%2C%20Canceled%29%20and%20labels%21%3DARC_Real_Device).

### Repository Released

<table><thead><tr><th width="308.890625">Repositories</th><th>Tags Released</th></tr></thead><tbody><tr><td>android-registration-client</td><td><a href="https://github.com/mosip/android-registration-client/tree/v1.0.1"><strong>v1.0.1</strong></a></td></tr></tbody></table>

### Compatibile Platform/Module

The following table outlines the tested and certified compatibility of Inji Verify 0.10.0 with other modules.

<table><thead><tr><th width="313.71484375">Platform/Module</th><th>Version</th></tr></thead><tbody><tr><td>MOSIP</td><td><a href="https://docs.mosip.io/1.2.0/releases/release-notes">1.2.0</a></td></tr></tbody></table>

{% hint style="warning" %}
**Note**: **NOT** Compatible with MOSIP Version 1.3.0
{% endhint %}

### Build and Deploy

To access the build and read through the deployment instructions, refer to the[ Developer Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-developer-guide).

### Configurations

For details related to Android Registration Client configurations, refer to the[ Configuration Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-configuration).

### User Guide

To learn more about the available features, processes, and user interface, refer[ Android Registration User Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-user-guide) for further information.
