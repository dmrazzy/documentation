# Birth Registration & UIN Issuance

#### When Does It Happen?

Birth registration occurs when parents/informants notify vital events to the concerned civil registration authority (CRA). This process is initiated from the CRVS system after the CRA has completed identity proofing and provided informant authentication. Following this, a request is submitted to MOSIP for the registration of an infant's birth.

A high-level workflow diagram for this process is provided below.

**High-Level Workflow:**

<figure><img src="../../../../../.gitbook/assets/Infant_birth_registration.png" alt=""><figcaption><p>Infant Birth Registration</p></figcaption></figure>

Steps and required information are provided below:

#### What Does MOSIP Do?

When MOSIP receives a registration request, it validates the received packet, processes the request, and stores the identity information. A UIN is generated and added to the identity repository.

#### What does CRVS receive?

Upon successful generation of the UIN, MOSIP sends credential data to the subscribed CRVS system via WebSub. By default, MOSIP recommends sharing the PSUT token (a print token to print eUIN cards) as part of the identity credential returned to CRVS. However, the system also supports sharing UIN, VID, or the PSUT token, based on the country's specific requirements. The recommended approach is to share the PSUT token, as it allows for printing the eUIN without exposing the actual UIN, thereby protecting an individual's privacy.

#### What Is the Workflow?

For newborns who do not yet possess a national ID, the CRVS system can initiate a national ID registration request on their behalf. This request is initiated by a guardian, parent, or another authorized individual, referred to in MOSIP as the **"Introducer"** or an **"Informant"** in the CRVS system. The introducer must already be registered in MOSIP and have a valid national ID, which is used for authentication through **eSignet** to ensure the legitimacy of the registration.

To further enhance security and data accuracy, the CRVS system utilizes MOSIP's authentication mechanisms to verify the identities of parents or informants during the birth registration process. This helps prevent fraudulent registrations and strengthens overall data quality and system integrity.

> **Note**: MOSIP typically defines infants as individuals aged between 0 to 5 years. However, this classification is configurable based on the specific legal or administrative definitions of the implementing country. Anyone above the configured age threshold is categorized as either a minor or an adult.

**Step 1: Operator Login & Authentication**

1. The CRVS operator logs into the CRVS portal and enters the required details for the newborn and the parent/introducer.
2. The introducer's identity (VID/UIN) is authenticated via **eSignet**.
3. Upon successful authentication, the request proceeds for further processing.

**Required Information for MOSIP to Process the Request:**

**Newborn Information** (Additional fields can be included based on country needs)

1. Name
2. Gender
3. Date of Birth (DOB)

**Introducer/Informant Information** (Additional fields can be included based on country needs)

1. Introducer's VID/UIN
2. eSignet User Info Token

**Step 2: Packet Creation**

CRVS creates a registration packet by calling the **Packet Manager API**, using:

1. Access Token
2. Client ID
3. ID Schema Version
4. Unique Registration ID (RID) / Application ID (AID)

The packet is stored in the **Object Store** for further processing.

**Step 3: Workflow Trigger & Notification**

1. CRVS calls the **Trigger API** to initiate the processing workflow.
2. Once the packet reaches a "Completed" state, MOSIP publishes an event to a **WebSub topic**, notifying subscribers.

**Step 4: Identity Credential Sharing**

Upon successful processing:

1. The newborn's demographic data is updated in MOSIP.
2. A notification is sent to the registered email/phone number.
3. The update event with identity credentials is published to the subscribed **WebSub topic**.
4. CRVS receives this update and can proceed with issuing the birth certificate.

#### Duplicate and/or Repeated Infant Birth Registration Requests

Duplicate and/or repeated requests may arise under the following conditions:

1. **Repeated Requests - Same RID Used In Multiple Requests:**
   1. When multiple requests are made using the same RID (Request ID) for the birth registration of the same infant (with the same or different data).
   2. Currently, the request will be processed even if the same RID is used in multiple requests.
   3. MOSIP will overwrite the existing data with the **most recent values** provided in the latest request.
2. **Duplicate Requests - Same Infant Demographic Data with Different RIDs:**
   1. Multiple requests are made using identical infant demographic data but with different RIDs.
   2. Currently, the request will be processed, and an additional UIN will be issued for the infant for the additional RID.

> **Note**: MOSIP relies on CRVS to perform deduplication and treats CRVS as the source of truth. While MOSIP has its internal deduplication mechanism to detect and reject duplicate packets, the above scenarios are not currently handled for rejection of duplicate and/or repeated requests.

#### Failure Handling in this Scenario

**Technical Failures:**

1. There is a possibility that some requests may fail due to failure caused by **internal MOSIP** technical problems during processing.
2. In case of internal processing issues within MOSIP, a **retry mechanism** automatically attempts reprocessing of failed requests.

**Validation Failures:**

1. This includes requests failing due to:
   * Missing mandatory fields
   * Invalid data formats
   * Schema validation errors
2. CRVS must subscribe to the WebSub topic to receive and act on such notifications.

| **Scenario**                      | **Existing Handling / Mechanism**                                                                                              | **Improvement Required?**                                                                                            |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| **Duplicate Birth Registrations** | Currently, MOSIP does not reject duplicate birth registration requests from CRVS since CRVS is considered the source of truth. | De-duplication is expected at CRVS. Should MOSIP detect duplicates across registrations from different CRVS systems? |
| **Failed eSignet Authentication** | Authentication failures are managed by eSignet. The user token is not generated if authentication fails.                       | Enhancement: Log failure reasons and notify CRVS of authentication issues.                                           |
| **Validation Failures**           | MOSIP validates the packet structure and rejects invalid packets.                                                              | Enhancement: Provide detailed rejection reasons back to CRVS.                                                        |
| **Packet Processing Failures**    | Internal MOSIP processing failures are logged and tracked.                                                                     | Enhancement: Send failure notifications to CRVS with actionable error messages.                                      |


***

## Learn More

* [**Packet Manager**](../../../../../id-lifecycle-management/supporting-components/packet-manager/README.md) - Understand packet structure, validation, encryption/decryption, and how registration packets are stored and retrieved from object storage for processing.

* [**Registration Processor**](../../../../../id-lifecycle-management/identity-issuance/registration-processor/overview/README.md) - Explore the workflow engine that validates packets, performs deduplication, generates UINs, and orchestrates the complete packet processing lifecycle.

* [**WebSub Event System**](../../../../../id-lifecycle-management/supporting-services/websub/README.md) - Learn about MOSIP's publish-subscribe mechanism for real-time event notifications, topic registration, and credential delivery to subscribed partners.

* [**Notifications & Event Handling**](../../notifications-and-event-handling.md) - Detailed guide on credential issuance notifications, packet status updates, WebSub subscription configuration, and error notification handling for CRVS integration.


