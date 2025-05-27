# Registration v1.2.1.1

**Release Version:** v1.2.1.1

**Release Type:** Patch Release

**Release Date:** Coming Soon

#### Overview

This patch release focuses on critical bug fixes related to the Credential Requestor, Registration Processor, and Notification components in the MOSIP platform. The release addresses issues encountered during Update UIN workflows, packet processing, and email notifications. These fixes aim to enhance system stability, correctness in credential generation, and proper notification dispatch.

#### Major Highlights/Features

* Ensures correct handling of signature validation during packet processing.
* Fixes issues in credential generation linked to VID in Update UIN workflows.
* Resolves failure in email notifications for CRVS\_DEATH events.

#### Bug Fixes

The following bugs have been fixed in this release:

<table><thead><tr><th width="185.921142578125">Ticket ID</th><th>Description</th></tr></thead><tbody><tr><td><strong>MOSIP-40982</strong></td><td>Credential request was not generated for VID during Update UIN flow</td></tr><tr><td><strong>MOSIP-40984</strong></td><td>Exception was thrown while updating the handle field in Update UIN flow</td></tr><tr><td><strong>MOSIP-31234</strong></td><td>Packet processing notifications were not received in SMTP after uploading RCF packets</td></tr><tr><td><strong>MOSIP-41135</strong></td><td>Packet signature validation skips during registration processing</td></tr><tr><td><strong>MOSIP-41276</strong></td><td>Packets with invalid signatures were reprocessed instead of being marked as failed</td></tr><tr><td><strong>MOSIP-41277</strong></td><td>Empty signature caused "Index 0 out of bounds" error during upload stage</td></tr><tr><td><strong>MOSIP-41403</strong></td><td>Email notifications failed for CRVS_DEATH due to incorrect process type mapping</td></tr><tr><td><strong>MOSIP-34070</strong></td><td>Credential Requestor Generator delayed in responding to websub event delivery</td></tr></tbody></table>

#### Known Issues

| **Jira Issue**                                                | **Description**                                                                                          |
| ------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| [MOSIP-41556](https://mosip.atlassian.net/browse/MOSIP-41556) | SMS notifications are not delivered via SMTP when handle is enabled for phone numbers with a '+' prefix. |

#### Repository Released

| Repository     | Tag Released |
| -------------- | ------------ |
| mosip-config   |              |
| id-repository  |              |
| packet-manager |              |
| registration   |              |

7. Compatible Modules

| **Module/Repo**             | **Compatible Version** |
| --------------------------- | ---------------------- |
| ID Authentication           |                        |
| key-manager                 |                        |
| kernel - core               |                        |
| kernel-notification-service |                        |
| kernel-idgenerator-service  |                        |
| kernel-auth-adaptor         |                        |
| kernel-auth-service         |                        |
| kernel-otp-manager          |                        |
