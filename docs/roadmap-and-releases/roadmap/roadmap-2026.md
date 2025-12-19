---
description: >-
  This roadmap outlines the planned features, progress, and release details for
  MOSIP throughout the calendar year 2026.
hidden: true
---

# Roadmap 2026

<mark style="color:red;">**In Progress, Coming Soon!**</mark>

Here we present the product roadmap for MOSIP Identity for the calendar year 2025. Annual product cycle of MOSIP commences in January and concludes in December.

Explore the detailed product roadmaps below for ......

## MOSIP Identity

'Content





### Resident Portal

<details>

<summary><strong>Vision</strong></summary>

The Resident Portal will become a modern, secure, and universally accessible gateway for residents to manage their identity services. A refreshed UI will deliver a cleaner, more intuitive experience, while alternative login options will ensure access even when all biometric modalities are locked. Strengthened by performance testing, automation, and security enhancements, the portal will offer faster, safer, and more reliable service delivery. With added USSD support, it will extend access to residents in low-connectivity and feature-phone environments, ensuring inclusivity for all.

</details>

<table data-header-hidden><thead><tr><th width="95.328125">Priority</th><th width="233.65625">Feature</th><th width="180.015625">Details</th><th>Status</th><th>Release</th></tr></thead><tbody><tr><td>P1</td><td>Revamp of Resident Portal UI</td><td><a href="https://github.com/mosip/resident-ui/issues?q=state%3Aopen%20label%3AResident_UI_Revamp">Revamp of Resident UI</a></td><td>Planned</td><td>Coming Soon</td></tr><tr><td>P1</td><td>Feature: Allow resident services to log in even when all the modalities are locked</td><td><a href="https://github.com/mosip/resident-services/issues/1534">https://github.com/mosip/resident-services/issues/1534</a></td><td>Planned</td><td>Coming Soon</td></tr><tr><td>P1</td><td>Performance testing + Automation + Security fixes</td><td> <a href="https://mosip.atlassian.net/browse/MOSIP-32001">MOSIP-32001</a></td><td>Planned</td><td>Coming Soon</td></tr><tr><td>P1</td><td>Feature: As a resident, I should be able to use all the services offered by resident portal using USSD services</td><td><a href="https://github.com/mosip/resident-services/issues/1535">https://github.com/mosip/resident-services/issues/1535</a></td><td>Planned</td><td>Coming Soon</td></tr></tbody></table>



### Android Registration Client

<details>

<summary>Vision</summary>

The Android Registration Client will provide a faster, more accurate, and reliable registration experience. With integrated third-party Quality SDKs, unified quality scoring, and combined match and biometric scores, the client will ensure high-assurance biometric capture. Streamlined demographic, block-listed words, acknowledgment, and packet approval pages will reduce errors and simplify operator workflows. OCR will speed up data entry, and telemetry will offer real-time operational insights.\
Overall, the client will evolve into a smart, efficient, and quality-driven registration platform.

</details>

<table data-header-hidden><thead><tr><th width="77.5703125">Priority</th><th width="211.12890625">Feature</th><th width="211.9453125">Details</th><th width="116.1640625">Status</th><th>Release</th></tr></thead><tbody><tr><td>P1</td><td>Quality SDK: Support for 3rd party SDK to measure quality of biometrics captured</td><td><a href="https://github.com/mosip/android-registration-client/issues/643">https://github.com/mosip/android-registration-client/issues/643</a></td><td>Planned</td><td>Coming Soon</td></tr><tr><td>P1</td><td><p></p><p>Restrict Block listed Words</p><ul><li>Demographic details</li><li>Acknowledgement</li><li>Packet Approval</li></ul></td><td><a href="https://github.com/mosip/android-registration-client/issues/644">https://github.com/mosip/android-registration-client/issues/644</a></td><td>Planned</td><td>Coming Soon</td></tr><tr><td>P1</td><td>Calculation of Quality Score</td><td><a href="https://github.com/mosip/android-registration-client/issues/645">https://github.com/mosip/android-registration-client/issues/645</a></td><td>Planned</td><td>Coming Soon</td></tr><tr><td>P1</td><td>SDK match score along with biometric score</td><td><a href="https://github.com/mosip/android-registration-client/issues/646">https://github.com/mosip/android-registration-client/issues/646</a></td><td>Planned</td><td>Coming Soon</td></tr><tr><td>P1</td><td>OCR Integration</td><td><a href="https://github.com/mosip/android-registration-client/issues/647">https://github.com/mosip/android-registration-client/issues/647</a></td><td>Planned</td><td>Coming Soon</td></tr><tr><td>P2</td><td>Telemetry</td><td><a href="https://github.com/mosip/android-registration-client/issues/648">https://github.com/mosip/android-registration-client/issues/648</a></td><td>Planned</td><td>Coming Soon</td></tr></tbody></table>





### Partner Management System



<table><thead><tr><th width="89.66015625">Priority</th><th width="246.79296875">Features</th><th width="188.5625">Details</th><th>Status</th><th>Release</th></tr></thead><tbody><tr><td>P1</td><td><p><strong>OIDC Client</strong> additional requirements:</p><p>a) additional configurable fields</p><p>b) all key types should be accepted in public key input</p><p>c) Support multi language</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38711">https://mosip.atlassian.net/browse/MOSIP-38711</a></td><td>In Progress</td><td>1.3.0-beta.4</td></tr><tr><td>P1</td><td><p><strong>ABIS Partner</strong> flow by Partner Admin</p><p>a) Partner creation</p><p>b) Partner Certificate Management</p><p>c) Partner Policy Mapping</p><p>d) Encryption key certificate upload OR API Key generation</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38721">https://mosip.atlassian.net/browse/MOSIP-38721</a></td><td>Planned</td><td>1.3.0-beta.4</td></tr><tr><td>P2</td><td>Change in Partner Admin registration userflow</td><td></td><td>Planned</td><td></td></tr><tr><td>P1</td><td><p><strong>Manual Adjudication Partner</strong> flow by Partner Admin</p><p>a) Partner creation</p><p>b) Partner Certificate Management</p><p>c) Partner Policy Mapping</p><p>d) Encryption key certificate upload OR API Key generation</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38731">https://mosip.atlassian.net/browse/MOSIP-38731</a></td><td>Planned</td><td><a href="http://1.3.0-beta.ga">1.3.0-beta GA</a> release</td></tr><tr><td>P1</td><td><p><strong>Print/ Credential Partner</strong> flow by Partner Admin</p><p>a) Partner creation</p><p>b) Partner Certificate Management</p><p>c) Partner Policy Mapping</p><p>d) Encryption key certificate upload</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38741">https://mosip.atlassian.net/browse/MOSIP-38741</a></td><td>Planned</td><td><a href="http://1.3.0-beta.ga">1.3.0-beta GA</a> release</td></tr><tr><td>P2</td><td><p><strong>IDA</strong> flow by Partner Admin</p><p>a) Partner creation</p><p>b) Partner Certificate Management</p><p>c) Partner Policy Mapping</p><p>d) Encryption key certificate upload</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38751">https://mosip.atlassian.net/browse/MOSIP-38751</a></td><td>Planned</td><td>1.3.0 GA release</td></tr><tr><td>P2</td><td><strong>SDK Partner</strong> flow by Partner Admin</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38761">https://mosip.atlassian.net/browse/MOSIP-38761</a></td><td>Planned</td><td>1.3.0 GA release</td></tr><tr><td></td><td></td><td></td><td></td><td></td></tr><tr><td>P1</td><td>Multiple partner type selection and management by a partner user.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38824">https://mosip.atlassian.net/browse/MOSIP-38824</a></td><td>Planned</td><td></td></tr><tr><td>P3</td><td><p>Organisation specific data is available across all users of a given partner organisation - Multi-tenancy</p><p>Also, In Partner Admin portal,</p><p>i) Partner Type Management- Approve/ Reject/ add new partner type</p><p>ii) User Management - Approve/ Reject/ Add new user</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38835">https://mosip.atlassian.net/browse/MOSIP-38835</a></td><td>Planned</td><td></td></tr><tr><td>P2</td><td>Legacy PMS API cleanup</td><td></td><td>Planned</td><td></td></tr><tr><td>P3</td><td><p><strong>Backlog items:</strong></p><p>a) PMS landing page</p><p>b) Help section in each page</p><p>c) UI enhancements</p><p>d) Deactivate partner policy request</p><p>e) Partner admin per partner type</p><p>f) Partner and admin comments for each approval request sent</p><p>g) Impact of OIDC Client, API key, SBI, Device, FTM deactivation on other modules etc</p></td><td><p><a href="https://mosip.atlassian.net/browse/MOSIP-32081">https://mosip.atlassian.net/browse/MOSIP-32081</a></p><p><a href="https://mosip.atlassian.net/browse/MOSIP-34175">https://mosip.atlassian.net/browse/MOSIP-34175</a></p></td><td>Planned</td><td></td></tr><tr><td>P4</td><td>UI Design for mobile devices</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-36567">https://mosip.atlassian.net/browse/MOSIP-36567</a></td><td>Planned</td><td></td></tr></tbody></table>











