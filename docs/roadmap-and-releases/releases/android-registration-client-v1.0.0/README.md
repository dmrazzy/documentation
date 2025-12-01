# Android Registration Client v1.0.0

**Release version**: 1.0.0

**Support**: GA Release

**Release Date**: 1st December, 2025

## **Overview**

The Android Registration Client is a tablet-based application that delivers a mobile-friendly version of the traditional desktop [Registration Client](https://docs.mosip.io/1.2.0/modules/registration-client). It is designed to work across all Android devices and supports the mobility requirements of countries adopting MOSIP.

## Features and Major Highlights

**Version 1.0.0** of the Android Registration Client is the GA release, covering features listed below:

1. [**GPS Tracking**:](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features#id-18.-gps-location) Tracks the location where a registration packet is created and measures its distance from the device's mapped registration center.
2. [**Applicant Biometric Correction**](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features#id-19.-biometrics-correction): Allows issuing a temporary ID when biometric capture fails, enabling the applicant to return for recapture before an AID is generated.
3. [**Settings**](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features#id-20.-scheduled-jobs-settings): Provides access to device details, scheduled job configurations, and global/local configuration settings within the ARC.
4. **Support for Landscape Mode:** Allows the Android Registration Client to function seamlessly in landscape orientation.
5. **Support for Phone Screens:** Optimizes the Android Registration Client for effective use on smaller mobile screens.
6. [**Auto Logout**](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features#id-17.-auto-logout): Automatically logs out the user after a configurable period of inactivity for security.

{% hint style="success" %}
**Note**: Compatible with [MOSIP version 1.2.0](https://docs.mosip.io/1.2.0/releases/release-notes)
{% endhint %}

### **Repository Released**

<table><thead><tr><th width="324.0625">Repositories</th><th>Tags Released</th></tr></thead><tbody><tr><td>android-registration-client</td><td>v1.0.0</td></tr></tbody></table>

### Stories

<table><thead><tr><th width="198.28125">JIRA</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1281">RCF-1281</a></td><td>Performance and Stability Testing of Android Registration Client Closed.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1185">RCF-1185</a></td><td>As an Operator, when I create any packet, my GPS location should also be sent as meta data.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1126">RCF-1126</a> </td><td>Security Testing</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-784">RCF-784</a></td><td>As an Operator/ Supervisor, my system should auto logout from ARC if it is idle for a long period of time.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-502">RCF-502</a></td><td>As an Operator, I should be able to correct the Applicant's Biometric.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-472">RCF-472</a></td><td>As an Operator, I should be able to run Android Registration Client on small screen like phone screen.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-471">RCF-471</a> </td><td>As an Operator, I should be able to run Android Registration Client in landscape mode.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-93">RCF-93</a></td><td>As an Operator, I should be able to access Device Settings.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-92">RCF-92</a></td><td>As an Operator, I should be able to access Global Config Settings.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-91">RCF-91</a> </td><td>As an Operator, I should be able to access Scheduled Jobs Settings.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-40">RCF-40</a> </td><td>As an Operator, I should be able to access Settings.</td></tr></tbody></table>

## **Bugs Fixed**

Below is the list of bug fixes as part of the 1.0.0 release, To get the complete list of bug fixes please refer [here](https://mosip.atlassian.net/issues/?jql=parent%20%3D%20RCF-31%20AND%20fixVersion%20%3D%20%22rcf-1.0.0%22%20AND%20issuetype%20%3D%20Bug%20ORDER%20BY%20created%20DESC).

<table><thead><tr><th width="177.3046875">JIRA</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1272">RCF-1272</a></td><td>The database migration should support a seamless schema upgrade.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1266">RCF-1266</a></td><td>In the "Scheduled Jobs Settings" screen of ARC, the page cannot be scrolled.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1262">RCF-1262</a></td><td>In ARC, packets created with PRIDs are not being processed.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1260">RCF-1260</a></td><td>In ARC, the "Authenticate" button on the authentication page is not clickable for exception packets.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1258">RCF-1258</a></td><td>In ARC, the "Global Config Settings" heading is not displayed on the global configuration page.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1246">RCF-1246</a></td><td>ARC requires license compliance updates.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1237">RCF-1237</a> </td><td>ARC does not mandate operator permissions.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1236">RCF-1236</a></td><td>ARC contains an issue involving the use of setAccessible(true).</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/RCF-1224">RCF-1224</a></td><td>In ARC, when a user logs in with an operator role, they are unable to start the ARC and encounter various error messages.</td></tr></tbody></table>

## **Documentation**

[Feature Documentation](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features)

[Developer Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-developer-guide)

[UI Specification Documentation](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/develop/ui-spec-documentation)

[Configuration Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-configuration)

[Android Registration User Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-user-guide)
