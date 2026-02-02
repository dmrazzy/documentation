# Death Registration & Identity Status Update

## When Does It Happen?

Death registration occurs when an **Informant** reports and notifies a **death event** to the concerned Civil Registration Authority (CRA). The default CRVS–MOSIP integration supports death registration and is initiated from the CRVS system after the CRA completes identity proofing and informant authentication. Upon completion, a request is submitted to MOSIP to update the identity record with the deceased status.

## **High-Level Workflow**

<figure><img src="../../../../../.gitbook/assets/death_registration.png" alt=""><figcaption><p>Death Registration</p></figcaption></figure>

Steps and required information are provided below:

## What Does MOSIP Do?

When MOSIP receives a death registration request, including the MOSIP ID (UIN/VID) of the deceased, it ensures that all mandatory attributes are present and correctly formatted, and then updates the identity status to mark the individual as deceased. The MOSIP ID (UIN/VID) itself is **not deactivated**; instead, a **“deceased” flag** is set in the identity record.

{% hint style="info" %}
**Note:** While the UIN is the actual ID of the person, the VID is a perpetual ID linked to the UIN and is recommended for use to avoid exposing the actual ID. To learn about identifiers in MOSIP, [refer here](../../../../../id-lifecycle-management/identity-management/identifiers.md).
{% endhint %}

## **Death Registration: Notifications to CRVS**

By default, MOSIP does **not** send a change acknowledgment to CRVS upon successfully updating the deceased flag.

If required, CRVS can be onboarded as a **credential partner** and subscribe to the relevant WebSub events. In this case, MOSIP can publish notifications of successful updates, allowing CRVS to track the completion of the death registration process and proceed with issuing the death certificate.

## What Is the Workflow?

A death registration request is initiated by the CRVS system based on the informant reporting the deceased. For MOSIP, the informant must be a registered user with a valid MOSIP ID. This MOSIP ID is authenticated using eSignet. During the death registration, MOSIP marks the individual as deceased using a status flag, without deactivating the ID. The date of death declaration is recorded in the system.

#### **Step 1: Death Registration Reported to CRVS**

* The Introducer visits the CRVS centre.
* The Introducer provides the required details of the deceased individual.
* The Introducer submits their own MOSIP ID for verification.
* The Introducer’s MOSIP ID is authenticated via eSignet.
* Upon successful authentication, the death registration request is ready to be submitted to MOSIP for processing, along with the details listed below.

**Information Required for MOSIP Processing:**

* **Deceased Individual Information** _(Additional fields can be included based on country requirements)_:
  * MOSIP ID (UIN/VID) of the deceased
  * Date of Death
* **Introducer Information** _(Additional fields can be included based on country requirements)_:
  * eSignet User Info Token – received as a response from eSignet upon successful authentication of informant

{% hint style="info" %}
**Note:** Updating the MOSIP ID schema is a prerequisite for supporting these workflows. Each workflow requires specific attributes to be added to the ID schema to enable successful data exchange and request submission from CRVS to MOSIP. Please [refer here](../../configurations-and-operations/configurations-details.md#id-schema-update-for-initiating-death-requests) for details.
{% endhint %}

#### **Step 2: Packet Creation**

To submit a request, the CRVS system must first **create a registration packet** using the **Packet Manager Create Packet API**. Once the packet is created, it can **pushed to MOSIP for processing**. This approach ensures that all required details are included and that the request is formally registered within MOSIP’s workflow for identity status udpate.

#### Step 3: Packet Processing

After the registration packet is created and uploaded to MOSIP’s object store using the **Packet Manager Create API**, the CRVS system must **initiate processing** by calling the **Sync and Trigger APIs** of the Packet Manager.

Once initiated, the packet moves through **several processing stages within MOSIP**, including technical validation and identity update, until the processing is completed.

#### **Step 4:** Death Registration Processing & Notifications

Once the packet is submitted to MOSIP, MOSIP checks the MOSIP ID (UIN/VID) current status to determine if it was previously marked as deceased. If not, MOSIP updates the death declaration flag to "Y – YES".

A notification is sent to the registered email or phone number to inform the resident about the successful update of the deceased status.

**Optional:** If CRVS is onboarded as a credential partner and has subscribed to the WebSub topic, the update can be shared with CRVS. This is not part of the default integration. Refer to the relevant guide for the approach to credential sharing.

#### Duplicate Requests for Death Registration

A request is considered a duplicate under the following conditions:

1. **Repeated Requests - Same AID Used for Multiple Requests:**
   * When multiple death registration requests are made using the same AID ([Application ID](../../configurations-and-operations/operational-considerations.md#id-7.-create-the-rid)) for the same individual, since the deceased flag has already been updated, subsequent requests do not result in any change to the status.
   * MOSIP does not reject repeated requests since CRVS is considered the source of truth.

{% hint style="info" %}
**Note**: MOSIP relies on CRVS to perform deduplication and treats CRVS as the source of truth. [Refer here](../../integration-overview-and-context/core-integration-principles.md#crvs-as-source-of-truth) for more details.
{% endhint %}

#### Failure Handling in this Scenario

**Technical Failures:**

1. There is a possibility that some requests may fail due to failure caused by **internal MOSIP** technical problems during processing.
2. In case of internal processing issues within MOSIP, a **retry mechanism** automatically attempts reprocessing of failed requests

**Validation Failures:**

1. Missing mandatory fields
2. Incorrect/wrong MOSIP ID (UIN/VID) of the deceased is provided.

| Scenario                        | Existing Handling / Mechanism                                                   |
| ------------------------------- | ------------------------------------------------------------------------------- |
| Invalid UIN/VID                 | For invalid or non-existent MOSIP IDs, MOSIP does not update the deceased flag. |
| Duplicate Death Registration    | Currently not rejected if submitted multiple times from CRVS.                   |
| Technical failures within MOSIP | Internal failures are logged.                                                   |

***

## Learn More

* [**Packet Manager**](../../../../../id-lifecycle-management/supporting-components/packet-manager/) - Understand packet structure, validation, encryption/decryption, and how registration packets are stored and retrieved from object storage for processing.
* [**Registration Processor**](../../../../../id-lifecycle-management/identity-issuance/registration-processor/overview/) - Explore the workflow engine that validates packets, performs deduplication, generates UINs, and orchestrates the complete packet processing lifecycle.
* [**WebSub Event System**](../../../../../id-lifecycle-management/supporting-services/websub/) - Learn about MOSIP's publish-subscribe mechanism for real-time event notifications, topic registration, and credential delivery to subscribed partners.
* [**Notifications & Event Handling**](../../notifications-and-event-handling.md) - Detailed guide on credential issuance notifications, packet status updates, WebSub subscription configuration, and error notification handling for CRVS integration.
