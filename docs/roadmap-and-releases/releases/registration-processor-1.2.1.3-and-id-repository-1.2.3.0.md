---
hidden: true
---

# Registration Processor 1.2.1.3 and ID Repository 1.2.3.0

**Version**: Registration Processor 1.2.1.3 & ID Repository 1.2.3.0

**Release Type**: Major Release

**Release Date**: Coming Soon

Steps required on how the schema and uispec to be updated for existing customers

### Overview

This patch release addresses critical functional and security issues. The fixes ensure proper processing of email handles containing underscores and strengthen biometric update validation to prevent unauthorised biometric updates against another resident’s UIN. These improvements enhance system reliability and help safeguard against potential identity misuse.

### Major Highlights / Improvements

#### Improved Handle Creation Validation

The handle creation process has been corrected to properly support email handles containing underscores (\_) as permitted by the default identity schema regex. This ensures that such handles are successfully registered and stored in the credential transaction table.

#### Strengthened Biometric Update Security

Additional validation has been introduced during biometric updates to prevent unauthorized updates to another resident’s UIN. The system now validates the biometric reference returned from the ABIS system against the UIN provided in the update request to ensure that the biometric data belongs to the correct resident before processing the update.

### Bug Fixes

<table><thead><tr><th width="374.234375">JIRA</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-34112">MOSIP-34112</a></td><td>Able to update biometric data against another resident's UIN</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-43481">MOSIP-43481</a></td><td>Handle creation fails when the email contains an underscore (_) while generating identity</td></tr></tbody></table>

### Repositories Released

<table><thead><tr><th width="373.046875">Repository</th><th>Tags Released</th></tr></thead><tbody><tr><td>id-repository</td><td>v1.2.3.0</td></tr><tr><td>registration</td><td>v1.2.1.3</td></tr></tbody></table>

### Documentation

* [Registration](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/registration-processor/overview)
* [ID Repository](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/id-repository)
