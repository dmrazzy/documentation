---
hidden: true
---

# Roadmap 2026

Here we present the product roadmap for MOSIP Identity for the calendar year 2026. This roadmap outlines the planned features, progress, and release details for 'MOSIP Identity and Modules'. Annual product cycle of MOSIP commences in January and concludes in December.

## MOSIP Identity

<details>

<summary>Vision</summary>

In 2026, MOSIP will be a **secure, intelligent, and high-performance digital identity platform** that empowers countries to manage citizen identities efficiently while enabling innovation in biometrics, cryptography, and handle-based identity management. Alongside delivering advanced features such as adaptive biometric learning, policy-driven UIN updates, and quantum-safe cryptography, the platform will prioritize **clearing technical debts and enhancing maintainability**, addressing infrastructure, code quality, security, and operational improvements. This dual focus ensures MOSIP remains **reliable, scalable, and future-ready**, providing countries with a robust foundation for inclusive and secure identity services.

</details>



<table><thead><tr><th width="98.71875">Priority 🗓️</th><th width="148.859375">Module</th><th width="253.83203125">Features 🛠️</th><th width="107.47265625">Details📝</th><th width="128.671875">Status 📊</th><th>Details 📌</th></tr></thead><tbody><tr><td>P1</td><td><strong>ID Repository</strong></td><td>Array of Handle - Phase 1</td><td>TBA</td><td></td><td></td></tr><tr><td>P1</td><td><strong>Registration Client &#x26; Processor</strong></td><td>Enabling Custom Handles feature</td><td>TBA</td><td></td><td></td></tr><tr><td>P1</td><td><strong>Key Manager</strong></td><td>Support for ECC Algorithm during encryption &#x26; decryption</td><td>TBA</td><td></td><td></td></tr><tr><td>P1</td><td><strong>Registration Processor/ Biometrics</strong></td><td>Enhancing the quality classifier stage</td><td>TBA</td><td></td><td></td></tr><tr><td>P1</td><td><strong>Registration Processor</strong></td><td>Biometric Adaptive Learning</td><td>TBA</td><td></td><td></td></tr><tr><td>P1</td><td><strong>Registration Processor</strong></td><td>Draft API Re implementation</td><td>TBA</td><td></td><td></td></tr><tr><td>P1</td><td><strong>ID Repository</strong></td><td>Enhancement of ID Repository</td><td>TBA</td><td></td><td></td></tr><tr><td>P1</td><td>Registration Processor</td><td>Update UIN through policy</td><td>TBA</td><td></td><td></td></tr><tr><td>P1</td><td><strong>Registration Processor</strong></td><td>Enhancing Manual Adjudication</td><td>TBA</td><td></td><td></td></tr><tr><td>P1</td><td><strong>Key Manager</strong></td><td>Supporting Quantum Safe Algorithms</td><td>TBA</td><td></td><td></td></tr><tr><td>P1</td><td><strong>Admin</strong></td><td>Enhancement of Admin features</td><td>TBA</td><td></td><td></td></tr><tr><td>P2</td><td><p><strong>Platform/ Biometrics</strong></p><p><strong>(Reg Client &#x26; Reg Proc)</strong></p></td><td>Biometric Quality Enhancement during Registration</td><td>TBA</td><td></td><td></td></tr><tr><td>P2</td><td>Reporting/Dashboard</td><td>Anonymous Profile &#x26; Reporting Enhancement</td><td>TBA</td><td></td><td></td></tr><tr><td>P2</td><td><strong>Platform</strong></td><td>Deactivation of SBI/FTM Partner</td><td>TBA</td><td></td><td></td></tr><tr><td>P2</td><td><strong>Registration Client</strong></td><td>Auto upgrade during the slow / low network</td><td>TBA</td><td></td><td></td></tr><tr><td>P2</td><td><strong>Platform/ Notification Services</strong></td><td>Notification Service Enhancement</td><td>TBA</td><td></td><td></td></tr><tr><td>P3</td><td><strong>Registration Client</strong></td><td>Support for Document Scanner</td><td>TBA</td><td></td><td></td></tr><tr><td>P3</td><td>ID Authentication</td><td>Data Insertion for IDA</td><td>TBA</td><td></td><td></td></tr><tr><td>P3</td><td><strong>Admin</strong></td><td>General ID issuance features</td><td>TBA</td><td></td><td></td></tr><tr><td>P3</td><td><strong>ID Authentication</strong></td><td>Age-Based Credential Attributes:</td><td>TBA</td><td></td><td></td></tr><tr><td>P3</td><td><strong>Key Manager</strong></td><td>Support for BBS+ in Key Manager</td><td>TBA</td><td></td><td></td></tr><tr><td>P3</td><td><strong>Key Manager</strong></td><td>Key manager to support for software based key stores/vaults</td><td>TBA</td><td></td><td></td></tr><tr><td>P4</td><td><strong>Registration Client</strong></td><td>Metadata Enhancement</td><td>TBA</td><td></td><td></td></tr><tr><td>wishlist</td><td><strong>ID Repository</strong></td><td>Array of Handle - Phase 2</td><td>TBA</td><td></td><td></td></tr></tbody></table>

### Resident Portal

<details>

<summary><strong>Vision</strong></summary>

The Resident Portal will become a modern, secure, and universally accessible gateway for residents to manage their identity services. A refreshed UI will deliver a cleaner, more intuitive experience, while alternative login options will ensure access even when all biometric modalities are locked. Strengthened by performance testing, automation, and security enhancements, the portal will offer faster, safer, and more reliable service delivery. With added USSD support, it will extend access to residents in low-connectivity and feature-phone environments, ensuring inclusivity for all.

</details>



<table><thead><tr><th width="99.82421875">Priority 🗓️</th><th width="272.6015625">Feature 🛠️</th><th width="142.5625">Details 📝</th><th>Status 📊</th><th>Release 📌</th></tr></thead><tbody><tr><td>P1</td><td>Revamp of Resident Portal UI</td><td><a href="https://github.com/mosip/resident-ui/issues?q=state%3Aopen%20label%3AResident_UI_Revamp">Revamp of Resident UI</a></td><td>Planned</td><td></td></tr><tr><td>P1</td><td>Feature: Allow resident services to log in even when all the modalities are locked</td><td><a href="https://github.com/mosip/resident-services/issues/1534">1534</a></td><td>Planned</td><td></td></tr><tr><td>P1</td><td>Performance testing + Automation + Security fixes</td><td> <a href="https://mosip.atlassian.net/browse/MOSIP-32001">32001</a></td><td>Planned</td><td></td></tr><tr><td>P1</td><td>Feature: As a resident, I should be able to use all the services offered by resident portal using USSD services</td><td><a href="https://github.com/mosip/resident-services/issues/1535">1535</a></td><td>Planned</td><td></td></tr></tbody></table>



### Android Registration Client

<details>

<summary>Vision</summary>

The Android Registration Client will provide a faster, more accurate, and reliable registration experience. With integrated third-party Quality SDKs, unified quality scoring, and combined match and biometric scores, the client will ensure high-assurance biometric capture. Streamlined demographic, block-listed words, acknowledgment, and packet approval pages will reduce errors and simplify operator workflows. OCR will speed up data entry, and telemetry will offer real-time operational insights.\
Overall, the client will evolve into a smart, efficient, and quality-driven registration platform.

</details>

<table><thead><tr><th width="99.9609375">Priority 🗓️</th><th width="302.140625">Feature 🛠️</th><th width="110.73046875">Details 📝</th><th width="135.484375">Status 📊</th><th>Release 📌</th></tr></thead><tbody><tr><td>P1</td><td>Quality SDK: Support for 3rd party SDK to measure quality of biometrics captured</td><td><a href="https://github.com/mosip/android-registration-client/issues/643">643</a></td><td>Planned</td><td></td></tr><tr><td>P1</td><td><p></p><p>Restrict Block listed Words</p><ul><li>Demographic details</li><li>Acknowledgement</li><li>Packet Approval</li></ul></td><td><a href="https://github.com/mosip/android-registration-client/issues/644">644</a></td><td>Planned</td><td></td></tr><tr><td>P1</td><td>Calculation of Quality Score</td><td><a href="https://github.com/mosip/android-registration-client/issues/645">645</a></td><td>Planned</td><td></td></tr><tr><td>P1</td><td>SDK match score along with biometric score</td><td><a href="https://github.com/mosip/android-registration-client/issues/646">646</a></td><td>Planned</td><td></td></tr><tr><td>P1</td><td>OCR Integration</td><td><a href="https://github.com/mosip/android-registration-client/issues/647">647</a></td><td>Planned</td><td></td></tr><tr><td>P2</td><td>Telemetry</td><td><a href="https://github.com/mosip/android-registration-client/issues/648">648</a></td><td>Planned</td><td></td></tr></tbody></table>



### Partner Management System

<table><thead><tr><th width="98.65234375">Priority 🗓️</th><th width="288.7109375">Features 🛠️</th><th width="105.1796875">Details 📝</th><th>Status 📊</th><th>Release 📌</th></tr></thead><tbody><tr><td>P1</td><td><p><strong>OIDC Client</strong> additional requirements:</p><p>a) additional configurable fields</p><p>b) all key types should be accepted in public key input</p><p>c) Support multi language</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38711">38711</a></td><td>In Progress</td><td>1.3.0-beta.4</td></tr><tr><td>P1</td><td><p><strong>ABIS Partner</strong> flow by Partner Admin</p><p>a) Partner creation</p><p>b) Partner Certificate Management</p><p>c) Partner Policy Mapping</p><p>d) Encryption key certificate upload OR API Key generation</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38721">38721</a></td><td>Planned</td><td>1.3.0-beta.4</td></tr><tr><td>P2</td><td>Change in Partner Admin registration userflow</td><td></td><td>Planned</td><td></td></tr><tr><td>P1</td><td><p><strong>Manual Adjudication Partner</strong> flow by Partner Admin</p><p>a) Partner creation</p><p>b) Partner Certificate Management</p><p>c) Partner Policy Mapping</p><p>d) Encryption key certificate upload OR API Key generation</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38731">38731</a></td><td>Planned</td><td><a href="http://1.3.0-beta.ga">1.3.0-beta GA</a> release</td></tr><tr><td>P1</td><td><p><strong>Print/ Credential Partner</strong> flow by Partner Admin</p><p>a) Partner creation</p><p>b) Partner Certificate Management</p><p>c) Partner Policy Mapping</p><p>d) Encryption key certificate upload</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38741">38741</a></td><td>Planned</td><td><a href="http://1.3.0-beta.ga">1.3.0-beta GA</a> release</td></tr><tr><td>P2</td><td><p><strong>IDA</strong> flow by Partner Admin</p><p>a) Partner creation</p><p>b) Partner Certificate Management</p><p>c) Partner Policy Mapping</p><p>d) Encryption key certificate upload</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38751">38751</a></td><td>Planned</td><td>1.3.0 GA release</td></tr><tr><td>P2</td><td><strong>SDK Partner</strong> flow by Partner Admin</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38761">38761</a></td><td>Planned</td><td>1.3.0 GA release</td></tr><tr><td></td><td></td><td></td><td></td><td></td></tr><tr><td>P1</td><td>Multiple partner type selection and management by a partner user.</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38824">38824</a></td><td>Planned</td><td></td></tr><tr><td>P3</td><td><p>Organisation specific data is available across all users of a given partner organisation - Multi-tenancy</p><p>Also, In Partner Admin portal,</p><p>i) Partner Type Management- Approve/ Reject/ add new partner type</p><p>ii) User Management - Approve/ Reject/ Add new user</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-38835">38835</a></td><td>Planned</td><td></td></tr><tr><td>P2</td><td>Legacy PMS API cleanup</td><td></td><td>Planned</td><td></td></tr><tr><td>P3</td><td><p><strong>Backlog items:</strong></p><p>a) PMS landing page</p><p>b) Help section in each page</p><p>c) UI enhancements</p><p>d) Deactivate partner policy request</p><p>e) Partner admin per partner type</p><p>f) Partner and admin comments for each approval request sent</p><p>g) Impact of OIDC Client, API key, SBI, Device, FTM deactivation on other modules etc</p></td><td><a href="https://mosip.atlassian.net/browse/MOSIP-32081">32081</a> &#x26; <a href="https://mosip.atlassian.net/browse/MOSIP-34175">34175</a></td><td>Planned</td><td></td></tr><tr><td>P4</td><td>UI Design for mobile devices</td><td><a href="https://mosip.atlassian.net/browse/MOSIP-36567">36567</a></td><td>Planned</td><td></td></tr></tbody></table>











