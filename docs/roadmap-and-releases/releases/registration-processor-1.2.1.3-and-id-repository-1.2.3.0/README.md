# Registration Processor 1.2.1.3 & ID Repository 1.2.3.0

**Version**: Registration Processor 1.2.1.3 & ID Repository 1.2.3.0

**Release Type**: Major Release

**Release Date**: 22nd April, 2026

### Overview

This patch release fixes critical functional and security defects. It ensures that email handles containing underscores are processed correctly. It also strengthens biometric-update validation so that biometric data can be updated only for the authenticated resident’s own UIN. These improvements enhance system reliability and help safeguard against potential identity misuse.

### Major Highlights / Improvements

#### Improved Handle Creation Validation

The handle creation process has been corrected to properly support email handles containing underscores (\_) as permitted by the default identity schema regex. This ensures that such handles are successfully registered and stored in the credential transaction table.

#### Strengthened Biometric Update Security

Additional validation has been introduced during biometric updates to prevent unauthorized updates to another resident’s UIN. The system now validates the biometric reference returned from the ABIS system against the UIN provided in the update request to ensure that the biometric data belongs to the correct resident before processing the update.

{% hint style="info" %}
This fix includes a schema update; therefore, the schema must be updated when adopting this release. Please refer [here](https://github.com/mosip/mosip-data/blob/v1.2.2.1/mosip_master_csv/csv/identity_schema.csv) for the updated schema.
{% endhint %}

### Bug Fixes

| JIRA                                                          | Description                                                                                |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| [MOSIP-34112](https://mosip.atlassian.net/browse/MOSIP-34112) | Able to update biometric data against another resident's UIN                               |
| [MOSIP-43481](https://mosip.atlassian.net/browse/MOSIP-43481) | Handle creation fails when the email contains an underscore (\_) while generating identity |

### Repositories Released

| Repository    | Tags Released                                                    |
| ------------- | ---------------------------------------------------------------- |
| id-repository | [v1.2.3.0](https://github.com/mosip/id-repository/tree/v1.2.3.0) |
| registration  | [v1.2.1.3](https://github.com/mosip/registration/tree/v1.2.1.3)  |
| mosip-config  | [v1.2.5.0](https://github.com/mosip/mosip-config/tree/v1.2.5.0)  |
| mosip-data    | [v1.2.2.1](https://github.com/mosip/mosip-data/tree/v1.2.2.1)    |

#### Compatibility Matrix

| Module                     | Version                                                                        |
| -------------------------- | ------------------------------------------------------------------------------ |
| Partner Management Service | [v1.2.2.2](https://github.com/mosip/partner-management-services/tree/v1.2.2.2) |
| Registration Client        | [v1.2.0.2](https://github.com/mosip/registration-client/tree/v1.2.0.2)         |
| mosip-automation-tests     | [v1.2.2.0](https://github.com/mosip/mosip-automation-tests/tree/v1.2.2.0)      |
| admin-ui                   | [v1.2.0.1](https://github.com/mosip/admin-ui/tree/v1.2.0.1/)                   |
| admin-services             | [v1.2.1.2](https://github.com/mosip/admin-services/tree/v1.2.1.2)              |
| pre-registration           | [v1.2.0.3](https://github.com/mosip/pre-registration/tree/v1.2.0.3)            |
| resident-services          | [v1.2.1.2](https://github.com/mosip/resident-services/tree/v1.2.1.2)           |
| mosip-data                 | [v1.2.0.1](https://github.com/mosip/mosip-data/tree/v1.2.0.1/)                 |
| id-authentication          | [v1.2.1.2](https://github.com/mosip/id-authentication/tree/v1.2.1.2)           |
| Resident Portal - UI       | [v0.9.1](https://github.com/mosip/resident-ui/tree/v0.9.1)                     |
| pre-registration-ui        | [v1.2.0.1](https://github.com/mosip/pre-registration/tree/v1.2.0.1/)           |

### Documentation

* [Test Report](./)
* [Registration](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/registration-processor/overview)
* [ID Repository](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/id-repository)
