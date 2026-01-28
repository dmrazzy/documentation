# MOSIP - CRVS Integration

## Overview

MOSIP can integrate with CRVS (Civil Registration and Vital Statistics) systems to create a unified digital identity ecosystem that spans an individual's entire life cycle.

**Civil Registration and Vital Statistics (CRVS)** systems are essential for documenting key life events such as births, deaths, marriages, and divorces. These systems provide individuals with legal recognition and play a vital role in ensuring access to rights and services. Additionally, CRVS systems generate important demographic data that supports effective policy-making, governance, and resource planning.\

**MOSIP** is an open-source, modular identity platform designed for scalability, interoperability, and compliance with global standards. It allows countries to customize identity systems tailored to their governance needs. To learn more about MOSIP, click [here](https://docs.mosip.io/1.2.0/id-lifecycle-management).

This synergy allows for the seamless management of an individual's identity across life events, starting from birth registration, through education and employment, to end-of-life verification.

> Note: When these systems operate in isolation, it leads to administrative inefficiencies, fragmented data, and challenges in service access.

## What is covered through the sections:
Topics reorganized through - Information Architecture.


## Integration Scope/Viability
Integration can be done for following scenarios:

Must read: [Refer to 'What is in scope of integration (integration-depth) and what is kept from integration, and why!](link)

- Birth Registration and UIN Issuance
- Death Registration and Identity Update
- Demographic Updates (e.g., Name Change, Address Update)

<div align="right"><figure><img src="../../../.gitbook/assets/CRVS (1).png" alt=""><figcaption><p>Birth and Death Registration</p></figcaption></figure></div>

#### Integration Architecture and Guidelines

The following is a list of guiding principles for this integration.

#### **Guidelines**

1. **Data Verification and Trust:**
   1. The initiation of the request with the data will be from CRVS to MOSIP
   2. Data transmitted from CRVS to MOSIP will undergo thorough data/document verification and deduplication before any request is initiated for MOSIP to process.
   3. MOSIP will trust all information provided by CRVS and honor the received requests. MOSIP will ensure that the technical and logical validations are all in place for packet processing.
   4. The integration should define the mandatory attributes to be exchanged between the two systems. It is crucial to agree upon these attributes to ensure consistent and accurate data exchange.
2. **Data consistency:**
   1. It is imperative to maintain consistency in data across both platforms, MOSIP and CRVS. This ensures that accurate and up-to-date data is shared between the systems.
3. **Fallback mechanism:**
   1. CRVS should initiate the request by calling the create packet API if the failure occurs during creation, CRVS should be able to retry to create the packet.
   2. In the event of technical failures on the MOSIP side during packet processing, reprocessing of the packet should be in place to ensure that the packet is marked appropriately as rejected or approved.
4. **Proof of Authentication:**
   1. For any request submitted to MOSIP, it is essential to include the biometric data of the informant or applicant, whenever feasible, to ensure the accurate verification of the individual initiating the request and confirm their legitimacy.
   2. For cases where biometric data is not collected by CRVS, it is recommended for CRVS to use eSignet for the applicant or introducer authentication. Post-authentication user info token should be shared with MOSIP for validation and auditing purposes.
5. **Identity Credential Sharing & Packet status**
   1. The identity credential sharing and packet status updates are two separate MOSIP integration points for any CRVS.
   2. MOSIP will generate and share identity credentials for new registration cases via WebSub. Any partner configured according to the policy will receive these notifications automatically.
   3. By default, MOSIP recommends sharing the PSUT token as part of the identity credential returned to CRVS. However, the system also supports sharing UIN, VID, or the PSUT token, based on the country’s specific requirements.
   4. MOSIP will publish packet status updates to a separate WebSub. Any credential partner subscribed to this WebSub topic can filter and track specific packets based on their status.
6. **Multiple CRVS Systems:**
   1. If a country deploys multiple CRVS systems, each system will be treated as a separate partner for MOSIP. This allows for independent management and integration with each CRVS system.
   2. Credentials generated/update messages from MOSIP will be sent to all the configured credential partners subscribed to the websub as per policy. It is the responsibility of the country and CRVS systems to manage the sharing of information across multiple CRVS systems.




   ------------------------------------------------------------------------------------------------------


<!-- Theory

### Objectives of Integration

Through integration with MOSIP (Modular Open Source Identity Platform), CRVS systems can enable:

* Real-time updates between registration and ID issuance. (Also Read this [scope](scope.md) for clear guideline on Integration depth and scope and what is not in suggested for scope and integration).
* Automated processes to reduce duplication
* Improved service delivery with more accurate citizen records
* Enhanced data-driven governance and policy-making


#### Vision and Impact

The combined capabilities of MOSIP and CRVS systems have the potential to demonstrate a transformative impact across countries. Together, they can provide a holistic framework for:

* **Recognition** – Establishing a secure, legal identity from birth
* **Protection** – Ensuring privacy, security, and proper authentication mechanisms
* **Provision** – Enabling access to services through a Unique Identification Number (UIN)

This vision further aligns with global Sustainable Development Goals (SDGs), including the UN’s SDG Target 16.9: “Provide legal identity for all, including birth registration, by 2030.”

#### Goals and Objectives of the Integration

#### **Objectives**

The integration of MOSIP with CRVS systems is designed to achieve several key objectives aimed at enhancing the efficiency, security, and accuracy of national identity and civil registration systems. These objectives include:

1. **Streamlining Processes and Enhancing Information Exchange**
   The integration will create a seamless flow of information between MOSIP and CRVS systems, reducing administrative burdens and enabling faster, more efficient processing of civil registration events.
2. **Establishing Secure and Reliable Data Verification Mechanisms**
   A central goal of this integration is to establish robust verification protocols, ensuring that data exchanged between the systems is accurate, reliable, and trustworthy, thereby preventing errors and enhancing security.
3. **Issuance of Unique Identity Numbers Linked to Birth Certificates**
   The integration will facilitate the issuance of unique identification numbers (UINs) linked directly to birth certificates, providing individuals with a verified, digital identity that is consistently updated as they undergo significant life events.
4. **Improving Accuracy in Identification and Reducing Data Duplication**
   By linking civil registration events to the national identity system, the integration will eliminate the risk of duplicate records, ensuring that every individual has a single, verified identity across all government systems.
5. **Maintaining Accurate Records of Deceased Individuals**
   One of the core aims of the integration is to ensure that the identity system is promptly updated with the status of deceased individuals, preventing identity fraud or the misuse of personal data for illicit purposes.
6. **Establishing and Maintaining Accurate Spousal and Marital Status Information**
   The integration will enable accurate recording of spousal relationships by linking marriage registrations with national identity records. This ensures that marital status information is consistently updated and maintained, resulting in more reliable and up-to-date personal identity data for married individuals.
7. **Enhancing Data Quality, Integrity, and Reliability for Better Governance**
   The integration will improve the accuracy, completeness, and timeliness of vital event records across systems, enhancing overall data quality and integrity. This reliable data foundation will support better governance by equipping authorities with accurate statistics for more informed, data-driven decision-making.
8. **Ensuring Strong Security and Privacy Protections**
   The integration will adhere to stringent data protection and cybersecurity standards, ensuring that personal information is safeguarded throughout the data exchange process. Privacy-by-design principles will be embedded to prevent unauthorized access, misuse, or breaches, thereby building public trust and protecting individual rights.




#### About the Platforms

**MOSIP** is an open-source, modular identity platform designed for scalability, interoperability, and compliance with global standards. It allows countries to customize identity systems tailored to their governance needs. To learn more about MOSIP, click [here](https://docs.mosip.io/1.2.0/id-lifecycle-management).\
\
**Civil Registration and Vital Statistics (CRVS)** systems are essential for documenting key life events such as births, deaths, marriages, and divorces. These systems provide individuals with legal recognition and play a vital role in ensuring access to rights and services. Additionally, CRVS systems generate important demographic data that supports effective policy-making, governance, and resource planning.\
\
In summary, this integration represents a significant stride in building resilient, citizen-centric digital infrastructure. By combining the strengths of CRVS and MOSIP, countries can ensure inclusive access to public services and rights, starting from birth and extending throughout every life milestone.

{% hint style="info" %}
Refer here for details on the MOSIP-CRVS integration [scope](scope.md) and [approach](approach/).
{% endhint %}

{% hint style="info" %}
Amongst the various CRVS systems, MOSIP has integrated with [**OpenCRVS**](https://documentation.opencrvs.org/), a digital civil registration solution. To know more about this integration, click here. (Link to be updated)
{% endhint %}

**Assumption:** This approach assumes that the CRVS system is recognized as the authoritative source of vital event data and is legally authorized to issue official birth and death certificates in accordance with the laws of the country.



-->



# Scope

### Overview

This section defines the scope of integration between Civil Registration and Vital Statistics (CRVS) systems and Modular Open-Source Identity Platform (MOSIP). The integration supports a comprehensive set of use cases, including birth and death registrations, demographic updates, and other vital events such as marriage and divorce.

### Use Cases Supported Through CRVS-MOSIP Integration

This integration currently supports the following use cases:

1. [Birth registration](https://docs.mosip.io/1.2.0/interoperability/integrations/mosip-crvs-integration/scope#id-1.-birth-registration)
   1. [Birth registration is initiated by the CRVS system](scope.md#id-1.1-birth-registration-initiated-by-crvs-system)
   2. [Duplicate and/or repeated infant birth registration requests initiated by the CRVS system](scope.md#id-1.2-duplicate-and-or-repeated-infant-birth-registration-requests-initiated-by-crvs)
   3. [Failure handling](scope.md#id-1.3-failure-handling)
2. [Death registration](https://docs.mosip.io/1.2.0/interoperability/integrations/mosip-crvs-integration/scope#id-2.-death-registration)
   1. [New death registration initiated by CRVS](scope.md#id-2.1-new-death-registration-initiated-by-crvs)
   2. [Duplicate and/or repeated requests for death registration](scope.md#id-2.2-duplicate-and-or-repeated-requests-for-death-registration)
   3. [Failure handling](scope.md#id-2.3-failure-handling)
3. [Demographic data update](https://docs.mosip.io/1.2.0/interoperability/integrations/mosip-crvs-integration/scope#id-3.-demographic-data-update-initiated-by-crvs-system)
   1. [Infant demo data update request initiated by CRVS](scope.md#id-3.1-infant-demo-data-update-request-initiated-by-crvs)
   2. [Duplicate and/or repeated infant demo data update requests](scope.md#id-3.2-duplicate-and-or-repeated-infant-demo-data-update-requests)
   3. [Adult demo data update request initiated by CRVS](scope.md#id-3.3-adult-demo-data-update-request-initiated-by-crvs-system)


{% hint style="info" %}
**Note**: These are the currently supported scenarios. Additional use cases will be introduced as the integration evolves and expands based on country-specific requirements and feedback.
{% endhint %}

<!-- Keshav - Bring those use cases from confluence -->



### 1. Birth Registration

#### 1.1 Birth Registration Initiated by CRVS System

When a birth (of an infant) is registered in the CRVS system, the CRVS system shares the demographic data with MOSIP, which then creates an ID for the newborn. This ID can be issued as a credential, granting immediate access to services requiring identity verification, like healthcare and education.

#### Approach

For newborns who do not yet possess a national ID, the CRVS system can initiate a national ID registration request on their behalf. This request is initiated by a guardian, parent, or another authorized individual, referred to in MOSIP as the **“Introducer”** or an **“Informant”** in the CRVS system. The introducer must already be registered in MOSIP and have a valid national ID, which is used for authentication through **eSignet** to ensure the legitimacy of the registration.

To further enhance security and data accuracy, the CRVS system utilizes MOSIP’s authentication mechanisms to verify the identities of parents or informants during the birth registration process. This helps prevent fraudulent registrations and strengthens overall data quality and system integrity.

Once the request is submitted, the CRVS system shares the birth data with MOSIP. MOSIP then processes the birth registration request based on the flow defined below, and generates a unique national ID for the newborn. This process closely mirrors the standard new registration flow in MOSIP, with the key difference being that the request is initiated by CRVS rather than through the Registration Client.

{% hint style="info" %}
Note: MOSIP typically defines infants as individuals aged between 0 to 5 years. However, this classification is configurable based on the specific legal or administrative definitions of the implementing country. Anyone above the configured age threshold is categorized as either a minor or an adult.
{% endhint %}

A high-level workflow diagram for this process is provided below.

#### High-Level Workflow

<figure><img src="../../../.gitbook/assets/Infant_birth_registration.png" alt=""><figcaption><p>Infant Birth Registration</p></figcaption></figure>

Steps and required information are provided below:

#### Step 1: Operator Login & Authentication

1. The CRVS operator logs into the CRVS portal and enters the required details for the newborn and the parent/introducer.
2. The introducer’s identity (VID/UIN) is authenticated via **eSignet**.
3. Upon successful authentication, the request proceeds for further processing.

**Required Information for MOSIP to Process the Request:**
**Newborn Information** (Additional fields can be included based on country needs)

1. Name
2. Gender
3. Date of Birth (DOB)

**Introducer/Informant Information** (Additional fields can be included based on country needs)

1. Introducers' VID/UIN
2. eSignet User Info Token

#### Step 2: Packet Creation

CRVS creates a registration packet by calling the **Packet Manager API**, using:

1. Access Token
2. Client ID
3. ID Schema Version
4. Unique Registration ID (RID)

The packet is stored in the **Object Store** for further processing.

#### Step 3: Workflow Trigger & Notification

1. CRVS calls the **Trigger API** to initiate the processing workflow.
2. Once the packet reaches a "Completed" state, MOSIP publishes an event to a **WebSub topic**, notifying subscribers.

#### Step 4: Identity Credential Sharing

Upon successful processing:

1. The newborn’s demographic data is updated in MOSIP.
2. A notification is sent to the registered email/phone number.
3. The update event with identity credentials is published to the subscribed **WebSub topic**.
4. CRVS receives this update and can proceed with issuing the birth certificate.

#### 1.2 Duplicate and/or Repeated Infant Birth Registration Requests Initiated by CRVS

Duplicate and/or repeated requests may arise under the following conditions:

1. **Repeated Requests - Same RID Used In Multiple Requests:**
   1. When multiple requests are made using the same RID (Request ID) for the birth registration of the same infant (with the same or different data).
   2. Currently, the request will be processed even if the same RID is used in multiple requests.
   3. MOSIP will overwrite the existing data with the **most recent values** provided in the latest request.
2. **Duplicate Requests - Same Infant Demographic Data with Different RIDs:**
   1. Multiple requests are made using identical infant demographic data but with different RIDs.
   2. Currently, the request will be processed, and an additional UIN will be issued for the infant for the additional RID.

{% hint style="info" %}
**Note**: MOSIP relies on CRVS to perform deduplication and treats CRVS as the source of truth. While MOSIP has its internal deduplication mechanism to detect and reject duplicate packets, the above scenarios are not currently handled for rejection of duplicate and/or repeated requests.
{% endhint %}

#### **1.3 Failure Handling**

1. **Technical Failures**
   1. There is a possibility that some requests may fail due to failure caused by **internal MOSIP** technical problems during processing.
   2. In case of internal processing issues within MOSIP, a **retry mechanism** automatically attempts reprocessing of failed requests.
2. **Validation Failures**
   1. This includes requests failing due to:
      1. Missing or invalid data
      2. Uniqueness conflicts
      3. Schema mismatches
   2. MOSIP will publish a **failure event** to the **WebSub topic**, detailing the rejection reason.
   3. CRVS must subscribe to the WebSub topic to receive and act on such notifications.

### 2. Death Registration

#### 2.1 New Death Registration Initiated by CRVS

When a death (infant or adult) is registered in the CRVS system, the information is shared with MOSIP, including the deceased’s National ID number. Once received, the individual’s ID undergoes a regulated update process. This ensures timely updates to social security systems and other related services, promoting consistency across systems and easing administrative burdens for bereaved families.

This integrated approach to ID lifecycle management supports the accuracy of records within the national ID system, enabling secure and uninterrupted access to essential services throughout an individual's life.

#### **Approach**

A death registration request is initiated by the CRVS system based on the informant reporting the deceased. For MOSIP, the informant must be a registered user with a valid national ID. This ID is authenticated using eSignet. During the death registration, MOSIP marks the individual as deceased using a status flag, without deactivating the ID. The date of death declaration is recorded in the system.

To facilitate this process, the following fields can be added to the ID schema:

1. declaredAsDeceased
2. deceasedDeclarationDate
3. typeOfDeath
4. deceasedInformer

It is up to each country to determine which fields should be included in the ID schema, based on their specific use case.

**High-Level Workflow:**

<figure><img src="../../../.gitbook/assets/death_registration.png" alt=""><figcaption><p>Death Registration</p></figcaption></figure>

Steps and required information are provided below:

#### Step 1: Registration Initiation & Authentication

1. The informant visits the CRVS Registration Centre.
2. Provides necessary details.
3. The CRVS system verifies the informant's identity via eSignet.
4. On successful authentication, CRVS adds the eSignet user info token to the ID schema and proceeds.

**Required Information for MOSIP to Process the Request:**

1. **Deceased Information** (Extendable as per country needs):
   1. Name
   2. Date & Time of Death
   3. UIN/VID
2. **Informant Information** (Extendable as per country needs):
   1. Informant's UIN/VID
   2. eSignet User Info Token

#### Step 2: Packet Creation

1. CRVS sends a request to the Packet Manager API.
2. Includes access token, client ID, ID schema version, and RID.
3. The packet is stored in the object store.

**Step 3: Workflow Trigger:**

1. CRVS triggers the registration workflow by calling the "trigger" API using the access token.
2. This ensures the death registration is processed within MOSIP.

**Step 4: Validation:**

1. MOSIP checks the UINs/VIDs' current status to determine if it was previously marked as deceased.
2. If not, MOSIP updates the death declaration flag to "Y - YES".

**Step 5: Notifications:**

1. Once the packet is processed and approved, a notification is sent to the registered email and phone number regarding the update.
2. Update is also shared with CRVS through a WebSub event.

#### 2.2 Duplicate and/or Repeated Requests for Death Registration

1. A request is considered a duplicate under the following conditions:
   1. **Repeated Requests - Same AID Used for Multiple Requests:**
      1. Multiple requests are made using the same AID for the same deceased individual.
      2. Currently, the request will be processed even if the same AID is used in multiple requests.
      3. MOSIP will overwrite the existing data with the **most recent values** provided in the latest request.
   2. **Duplicate Requests - Same Data with Different AIDs:**
      1. Multiple requests are made for the same individual using identical data but with different AIDs.
      2. Currently, the request will be processed, and data will be updated based on the latest request.

{% hint style="info" %}
**Note**: MOSIP relies on CRVS to perform deduplication and treats CRVS as the source of truth. While MOSIP has its internal deduplication mechanism to detect and reject duplicate packets, the above scenarios are not currently handled for rejection of duplicate and/or repeated requests.
{% endhint %}

#### 2.3 Failure Handling

1. **Technical Failures**
   1. Failures due to internal MOSIP issues.
   2. MOSIP includes a retry mechanism to reprocess such requests.
2. **Validation Failures**
   1. Failures due to data validation errors or uniqueness conflicts.
   2. MOSIP returns a failure response and rejects the request if required fields are missing or data is incomplete.
   3. MOSIP will publish a **failure event** to the **WebSub topic**, detailing the rejection reason.
   4. CRVS must subscribe to the WebSub topic to receive and act on such notifications.

#### 3. Demographic Data Update Initiated by CRVS System

Demographic updates may be required throughout an individual’s lifecycle due to various life events or corrections. These updates can originate from different authoritative sources, including CRVS, depending on country policy.

In the MOSIP–CRVS integration context, demographic updates received from CRVS fall into **two broad categories**, based on whether biometric data is linked to the individual’s ID:

**1. IDs not having biometrics linked  (Infant or Pre-Biometric Stage)**

These are typically records created at birth registration, where no biometrics are captured.\
Demographic details for such individuals may need updates due to:

* Corrections in the birth record (name, DOB, gender, etc.)
* Changes arising from adoption or guardianship
* Amendments identified by CRVS during vital event updates

**2. IDs having Biometric Linkages (Adults or Individuals Enrolled With Biometrics)**

Once an individual has completed biometric enrolment, the identity becomes anchored to irreproducible biometric traits.\
Demographic updates at this stage carry higher identity-related risks and may involve legal implications.

Updates may be needed for:

* Marriage-related name changes
* Divorce or separation-related updates
* Gender changes as legally recognized
* Corrections to previously entered data
* Changes in parent/guardian details after legal processes

Even though CRVS may identify or certify these life events, **biometric re-authentication** becomes essential to ensure that the correct individual is tied to the update request.

Country policy may decide whether:

* CRVS submits these updates directly to MOSIP **after authentication via eSignet**, or
* Individuals must visit a MOSIP Registration Center for updates.

MOSIP technically supports both models, but the choice must align with country legal and operational policy.&#x20;

#### Supported Demographic Fields for Update

The following fields are commonly updated as part of life events or corrections:

* **Name**
* **Date of Birth (DOB)**
* **Gender**

{% hint style="info" %}
**Important Note:**\
This list is suggestive **and not restrictive**.\
Countries can configure **any demographic attributes** they wish to expose for update through CRVS–MOSIP integration, based on local laws, governance, and operational requirements.
{% endhint %}

{% hint style="info" %}
**Note:** Updates to biometrics of the individuals beyond the demographic fields are not supported via CRVS. For such changes, individuals must visit the National ID department and initiate the request directly through the National ID system (MOSIP).
{% endhint %}

**High-Level Workflow:**

<figure><img src="../../../.gitbook/assets/Infant_Demo _Data_Update.png" alt=""><figcaption><p>Infant Demo Data Update</p></figcaption></figure>

The various scenarios for demographic data updates are outlined below, along with the basic workflow and the expected behavior from MOSIP in each case.

#### 3.1 CRVS-Initiated Demographic Updates for Individuals Without Biometric Records

This includes scenarios such as:

1. Corrections to inaccurate information submitted during birth registration
2. Changes resulting from life events like adoption or a change in guardianship

Steps and required information are provided below:

#### Step 1: Update Request Initiation & Authentication

1. The parent, guardian, or introducer visits the CRVS registration centre and provides updated demographic details.
2. CRVS authenticates the individual's identity using their national ID via eSignet.
3. Upon successful authentication, the request is sent to MOSIP along with the eSignet user info token.

**Required Information for MOSIP to Process the Request:**

1. **Individual’s Update Information** (Any or all of the following fields):
   1. Name
   2. Date of Birth (DOB)
   3. Gender
2. **Informant Information** (Extendable as per country needs):
   1. eSignet User Info Token

#### Step 2: Packet Creation

1. CRVS creates a registration packet by calling the Packet Manager API with an access token, client ID, ID schema version, and RID.
2. The packet is stored in the Object Store.

#### Step 3: Workflow Trigger

1. CRVS triggers processing by calling the "trigger" API with the access token.
2. This ensures the update packet is processed within MOSIP.

#### Step 4: Update & Notification

1. Once approved, the demo data is updated in MOSIP.
2. Notifications are sent to the registered email and phone number.
3. Update status is also shared with CRVS via a WebSub event.

#### 3.2 Duplicate and/or Repeated Infant Demo Data Update Requests

An update request is considered a duplicate under the following conditions:

1. **Repeated Requests - Same RID Used for Multiple Requests:**
   1. Multiple requests are made using the same RID (Request ID) for the update of the same individual.
   2. Currently, the request will be processed even if the same RID is used in multiple requests.
   3. MOSIP will overwrite the existing data with the **most recent values** provided in the latest request.

{% hint style="info" %}
**Note**: MOSIP relies on CRVS to perform deduplication and treats CRVS as the source of truth. While MOSIP has its internal deduplication mechanism to detect and reject duplicate packets, the above scenarios are not currently handled for rejection of duplicate/repeated requests.
{% endhint %}

2. **Duplicate Requests - Multiple Update Requests with Different RIDs for the Same Infant (with Same or Different Demo Data):**
   1. When MOSIP receives multiple update requests for the same infant with different RIDs, whether the demographic data fields contain the same values or have been modified, each request is treated as a new submission.
   2. Currently, the request will be processed, and data will be updated based on the latest request.

#### 3.3 CRVS-Initiated Demographic Data Update for IDs With Biometric Linkage

While adults may experience life events (e.g., marriage, divorce) that require updates to their name or address, **such demographic updates should be handled directly through the National ID system**, not via CRVS. Since adult records already have biometrics linked to their National ID, any changes must follow the official National ID update process. Individuals should therefore visit the National ID department to initiate these updates.

#### Rationale

1. Biometric authentication is the most secure and reliable way to verify an individual, reducing the risk of incorrect or fraudulent updates being made on behalf of someone else.
2. The MOSIP National ID is a foundational, connected identity system used by multiple downstream services for authentication. Even a small demographic error (e.g., name change) can disrupt access to essential services. Ensuring updates occur through a biometric-authenticated channel protects the integrity of the ID and safeguards citizens’ interests.

### Integration Limitations

Although the integration scope includes scenarios for birth, death, and updates, there are still some cases where limitations exist. While some of these limitations have been outlined above as part of the scope, this section will cover all current limitations of the integration.

1. **No Support for New Adult Birth Registrations**:\
   MOSIP does not support new adult birth registrations when the request comes from CRVS.
2. **No Support for Demographic Updates When Biometrics Are Already Linked to a National ID:**\
   MOSIP will not support demographic updates from CRVS for individuals whose biometrics are already linked to a National ID. For such updates, individuals are expected to visit the National ID registration centre (MOSIP).
3. **Integration for Birth and Death Registrations**:\
   The integration works seamlessly for birth and death registrations. However, updates to demographic data are still a work in progress.
4. **Duplicate Request Rejection**:\
   Since the CRVS system is considered the source of truth, MOSIP currently does not reject duplicate birth/death registration requests received from the CRVS system. This can result in multiple UINs for the same infant or an update of the death flag for the same deceased. Deduplication is expected to be handled by CRVS.
5. **No Support for Rejected Packets, Status Updates, and Reason**\
   If a request fails due to validation issues in MOSIP, there is currently no mechanism to send detailed rejection reasons back to CRVS.
6. **No support for Offline Integration**:\
   This integration works only when online connectivity is available as eSignet authentication is a necessary step before a request is submitted to CRVS. Offline (no-connectivity) support is under development.
7. **Use of VID/UIN for Death Registration and Demo Data Updates**:\
   Only VID or UIN can be used to register a death or submit requests for the demo data updates. Currently, MOSIP does not support death updates with any other identifier.

While these are the currently supported scenarios, additional use cases will be introduced as the integration evolves and expands based on country-specific requirements and feedback.

In summary, by embracing open-source principles, MOSIP remains committed to transparency, collaboration, and adaptability. The integration of CRVS systems with MOSIP marks a significant step toward the global adoption of Open Standards, promoting interoperability and flexibility across diverse digital ecosystems. Through these efforts, we aim to advance societal well-being and expand equitable access to essential services at scale.



# Approach

MOSIP, as a modular identity platform, utilizes the [Registration Client](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/registration-client) to collect essential information from individuals. This data is then used to generate a registration packet, which is uploaded to the [Registration Processor](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/registration-processor/overview). Once the packet passes all required validations, a national ID is generated for the individual.

To enhance this process and support diverse entry points for national ID requests, including those from external systems such as CRVS platforms, MOSIP exposes a set of APIs through its [Packet Manager](https://docs.mosip.io/1.2.0/id-lifecycle-management/supporting-components/packet-manager) module. These APIs enable external systems to create and upload registration packets directly, bypassing the core registration workflow, and thus offering a streamlined integration pathway.

For every registration request, an [ID Schema](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-management/id-schema) must be defined. This schema, based on a standardized JSON structure, outlines the fields and data elements to be stored in MOSIP’s Identity Repository. It applies to all incoming requests from CRVS systems, such as birth registrations, demographic updates, or death notifications. The schema is flexible and can be customized to align with the specific needs of a country’s identity program.

## Pre-requisites

To successfully integrate with MOSIP’s registration process, external systems must fulfill certain prerequisites. These requirements ensure proper alignment, security, and functionality of the integration. Key pre-requisites include:

1.  **Create** a New Client and Assign a Role in Keycloak

    [**Keycloak**](https://docs.mosip.io/1.2.0/id-lifecycle-management/supporting-components/keycloak) is an identity and access management tool utilized by MOSIP. Use Keycloak, to create a new client for the external system (e.g., CRVS).

    1. Generate a unique client ID and client secret.
    2. Define a specific role (e.g., `CRVS_BIRTH_REGISTRATION_ROLE`) that reflects the intended function.
    3. Assign this role to the newly created client to enable permissioned access to relevant APIs.
2. **Obtain Access Token for API Calls**\
   Authenticate the CRVS system by calling the Keycloak token endpoint using the client credentials.
   * Retrieve a valid access token, which must be included in all subsequent API calls to MOSIP for authentication and authorization.
3. **Set Up a Registration Centre**\
   Define and register a unique Centre ID in the system.
   * This Centre ID should represent the CRVS registration location initiating the request.
   * It will be used to track and manage packet submissions by location.
4. **Register a Machine**\
   Create a unique Machine ID and corresponding key.
   * This ID will identify the hardware or system used for submitting requests to MOSIP.
   * The key ensures that only authorized machines can interact with MOSIP services.
5. **Create a Default Officer Profile**\
   Set up a default officer or operator ID who will be responsible for sending registration requests on behalf of the CRVS system.
   * This officer represents the actor initiating the transaction from the CRVS interface.
6. **Map Officer to Centre and Machine**\
   Establish mappings to link the created Officer ID with the relevant Centre ID and Machine ID.
   * This step ensures that the officer is correctly associated with a specific registration centre and the hardware device authorized to perform registrations.
7. **Generate and Use a Unique Application ID (AID)**\
   For each registration event (e.g., birth or death), the CRVS system must generate a unique **Application ID (AID).**
   * The AID should be included in the packet submitted to MOSIP.
   * It will serve as the reference ID for tracking the request and receiving response events via WebSub.

## Steps

Once the prerequisites are met, the integration between a CRVS system and MOSIP can be carried out by following these steps:

1. **Collect Vital Event Information**\
   The CRVS system should begin by capturing all necessary information related to the vital event (e.g., birth, death, marriage, or divorce). This includes personal identification details, event-specific data, and any required supporting documents.
2. **Submit Data Using Create Packet API**\
   Use MOSIP’s `Create Packet API` (provided by the Packet Manager module) to submit the collected information. This API call will initiate the creation of a packet containing the event data.
3. **Upload the Packet to MOSIP**\
   Ensure the packet is successfully uploaded to MOSIP’s registration system. The packet should include all required data elements to support identity issuance or event registration.
4. **Trigger Packet Processing**\
   Invoke the `Sync & Trigger API` from the Workflow Manager Service to start the validation and processing of the uploaded packet according to the configured workflow.
5. **Validate and Process the Packet**\
   MOSIP will subsequently perform validation checks to confirm that the packet is complete, consistent, and compliant with defined schema and business rules. Only validated packets will proceed further.
6. **Generate Identity Credentials**\
   On successful validation, MOSIP will generate the identity credentials (e.g., national ID) for the individual and/or update the required data. This ID will be linked to the vital event captured by the CRVS system.
7. **Listen for WebSub Event Publication**\
   MOSIP will publish an event to a pre-defined WebSub topic, indicating the status of the packet processing and including the generated national ID, if applicable.
8. **Subscribe and Monitor WebSub Updates**\
   Ensure the CRVS system is subscribed to the relevant WebSub topic. It should listen for and consume event notifications to receive real-time updates from MOSIP.
9. **Issue Certificates and Update Records**\
   Upon receiving the national ID and processing status, the CRVS system should proceed to issue the corresponding official certificate (e.g., birth, death, marriage). Additionally, update the national records to reflect the new identity credentials. This step ensures formal recognition and accurate documentation of the citizen's identity and vital event.

For detailed technical specifications, API documentation, and schema definitions, please refer to the subsequent section [here](technical-details.md). For details on configuration changes, please refer [here](../../../../id-lifecycle-management/identity-issuance/registration-processor/deploy/configurations-details.md).



## Other keys and certificates which are required for the integration

1. eSignet integration - If eSignet is used for authentication, refer here for detailed info on the onboarding process and the key required for onboarding to eSignet - [Integrate with eSignet](https://docs.esignet.io/esignet-authentication/test/try-it-out/integrate-with-e-signet).
2. Auth Partner Onboarding - (Certificates required Auth Partner Onboarding) - Refer to the [Partner Management Services - End User Guide](../../../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/end-user-guide.md#authentication-partner-workflow).




# Technical Details

This section outlines the technical requirements for integrating a Civil Registration and Vital Statistics (CRVS) system with MOSIP, applicable to any country implementation. It covers the necessary prerequisites and API specifications to facilitate a seamless and secure connection between the two systems, enabling smooth data exchange and processing of birth registration requests. Depending on the country's implementation model and agreements, these steps may be carried out by the System Integrator (SI) or the CRVS team.

## ID schema Configuration

To initiate any registration request, the country must define an ID schema based on the specific requirements for CRVS integration. The sample ID schema can be referred to [here](../../../../_files/id-schema/id-schema-sample.json) and should be customized to include all required fields for packet generation per the country’s requirements. This schema governs the structure of the data submitted to MOSIP for processing and storage in the Identity Repository.

{% hint style="info" %}
**Note:** MOSIP advises adopting and customizing the latest released ID schema version to meet country-specific needs.
{% endhint %}

For comprehensive guidance on defining and customizing the ID schema in MOSIP, please refer to the [documentation here](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-management/id-schema).

### 1. Create Client ID/Role for the CRVS

As part of the integration approach, two specific API’s are exposed:

1. Create a packet API from the MOSIP packet manager module to create a packet.
2. Trigger the API from the registration processor module to process the packet.

allowing external systems (in this case, CRVS) to use these APIs to initiate requests.

To facilitate this, the external system must be assigned a specific new client ID and secret, ensuring secure and authenticated communication. Additionally, a new, specific role should be created for the external user, which will be associated with the API request in subsequent calls for packet creation and processing.

This role helps MOSIP validate and verify that the request is coming from an authorized and authentic source, ensuring secure and accurate handling of the registration process. By associating the role with the API request, MOSIP can properly authenticate the external system and manage permissions for the request flow.

Read on to learn more about the specific steps involved

#### Create the Client

* **Log in to Keycloak Admin Console**
  * Access the keycloak admin console.
  * Ensure you have the necessary administrative privileges to create clients.
* **Select Your Realm**
  * If you are not already in the desired realm, switch to it from the top-left drop-down menu. The realm should be the one where you want to create the client.
* **Create a New Client**
  * In the left-hand menu, go to **Clients** and click on **Create**.
* **Enter the Client Details**
  * **Client ID**: Enter `mosip-crvs1-client` as the client ID (or a relevant name based on your deployment).
  * **Client Protocol**: Select `openid-connect`.
  * **Root URL**: Leave this field blank or enter the URL if required.
* **Save the Client**
  * After entering the necessary details, click **Save** to create the client.

Once the client is created, please update the properties in the locations below:

1. `auth.server.admin.allowed.audience` In the [Packet manager default properties](https://github.com/mosip/mosip-config/blob/v1.2.4.0/packet-manager-default.properties#L24).
2. `auth.server.admin.allowed.audience` In the [Registration processor default properties](https://github.com/mosip/mosip-config/blob/v1.2.4.0/registration-processor-default.properties#L996).

{% hint style="info" %}
**Note:** The client name specified here is a placeholder and can be customised to suit the specific requirements of the System Integrator SI/CRVS.
{% endhint %}

#### Configuring the Client

* **Access the Settings Tab**
  * After creating the client, navigate to the **Settings** tab.
* **Configure Client Settings**
  * **Access Type**: Set this to **confidential** if you intend to use client credentials for authentication.
  * **Service Accounts Enabled**: Turn this option **ON** if you are using client credentials flow for secure communication.
  * **Valid Redirect URIs**: Enter `*` (or specify specific URLs if known and necessary).
* **Save the Changes**
  * Once the configuration is complete, click **Save** to apply the changes.

#### Generate and note the Secret key

* Navigate to the **Credentials** tab.
* If you selected the **confidential** access type, keycloak will generate a **Secret Key**. Note this secret as it will be used for authentication in subsequent API calls.

#### Creating the Role

* **Go to the Roles Section**
  * In the Keycloak Admin Console, under your realm, navigate to **Roles**.
* **Create a New Role**
  * Click on **Add Role**.
  * Enter the following details:
    * **Role Name**: `ONLINE_REGISTRATION_CLIENT`
  * Click **Save** to create the role.

#### Assigning the Role to the Client

* **Assign the Role to the Client**
  * Go back to the **Clients** section and select the client `mosip-crvs1-client` that you previously created.
* **Navigate to Service Account Roles**
  * Under the **Service Account Roles** tab (this tab is visible only if **Service Accounts Enabled** is turned on), click on **Add Role**.
* **Select the Role**
  * From the **Client Roles** dropdown, select either `realm-management` or your specific desired client role (if the role is specific to a client).
  * Add the `ONLINE_REGISTRATION_CLIENT` role to the selected client.

### 2. Fetch Access Token to Call the APIs

Once the role is created and mapped to the client ID. As a follow-up step, below keycloak API is to be called to authenticate the CRVS associated with the new role. In the response of the API, there is an access token returned in the response header. This is the access token that should be used when initiating any request using the packet manager API.

**Authenticate Endpoint:** {domainname}/v1/authmanager/authenticate/clientidsecretkey

**Method:** POST

**API Request Structure:**

```json
{
  "id": "string",
  "metadata": {},
  "request": {
    "appId": {{appId}},
    "clientId": {{clientId}},
    "secretKey": {{secretKey}}
  },
  "requesttime": "{{requestTime}}",
  "version": "string"
```

In the API above, the fields Client ID and Secret key are the values created in the previous steps, as mentioned above. Once the authentication is successful, in the response header, we will receive an access token, which is to be noted and used for the subsequent packet manager API request.

#### 3. Create the Default Officer

To process the request coming from CRVS, a default officer is to be registered and assigned to the CRVS. The officer is to be added from keycloak by the admin.

* **Log in to Keycloak**
  * Open the Keycloak admin portal.
  * Log in using your admin credentials.
* **Navigate to the Users Section**
  * In the left-hand menu, click on Users.
* **Add a New User**
  * Click on the Add user button to create a new user.
  * Fill in the required fields under the details tab:
    * **Username**: Enter a unique username for the officer (this will become the Officer ID).
    * **First Name**: Enter the officer's first name.
    * **Last Name**: Enter the officer's last name.
* **Set Temporary Password**
  * Under the **Credentials** tab, set the password for the officer.
  * Toggle **Temporary** to **Off** to disable the "temporary password" feature.
  * Click **Set Password**.
* **Enable the User**
  * Under the **User Enabled** section, ensure that the user is **enabled**.
  * Click **Save** to create the user.
* **Assign Roles to the User**
  * Navigate to the **Role Mappings** tab.
  * Under **Available Roles**, select the role **Registration\_officer**.
  * Click the **Add selected** button to assign the role.
* **Finalize the Officer Creation**
  * After saving, the officer's **username** from step 3 will serve as the **Officer ID**.
  * This **Officer ID** will be used in the subsequent create packet API request. Ensure you pass this ID correctly in the request.

#### 4. Create Centre

A unique default centre will be assigned to the CRVS to process requests. This centre can be created through the Admin Portal. For detailed instructions on how to create a centre, refer to the [**Admin Portal Center Creation Guide**.](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/administration/test/admin-portal-user-guide#create-center)

**Fetching the Centre ID**
As of now, there is no direct support for fetching the specific centre ID in MOSIP. To retrieve the Centre ID, use the API below to get a list of all centres in the system. From this list, manually search for the Centre ID associated with the newly created centre for CRVS. The “id“ attribute in the response will be the centre ID.

**Get List of All Centres Endpoint:** `{domain}/v1/admin/masterdata/registrationcenters`

**Method:** GET

Once the **Centre ID** is identified, ensure it is saved securely for future reference. This **Centre ID** will be required in subsequent interactions with the CRVS system for processing requests.

#### 5. Create Machine

A unique default machine will be assigned to the CRVS to process requests. This machine can be created through the Admin Portal.

Before creating a new machine, it is required that a public key is fetched using the below API. The public key received in the response of this API is to be used as the public key & signing public key, while adding the details in the admin portal, to create and onboard the machine.

**URL**: `{domain}/v1/keymanager/tpmsigning/publickey`

**Method**: POST

**API Request Structure:**

```json
{     "request" : 
    {         
        "serverProfile" : "Prod"    
     } 
}
```

**Sample Response:**

```json
{
    "id": null,
    "version": null,
    "responsetime": "2025-04-15T14:04:31.683Z",
    "metadata": null,
    "response": 
    {
        "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzWOM81oggDiy26SPzASVCLpEf0sJC-81j7GCpDHVcHCAfQVIxaBP9K6u2R19mgiakVY92Nlb5y4PUKV1EpLbZKQxaK14gU4ks7hroCM_gbEssaon7lCFCu8uobKriXlC9RI1ZY9HF9QFfilCuGC9q58gZ_YC-VMGZOB9YtN_5QRbXvI9XQr-d1eODtOVuVCpsPz6FkEHOSWdj0HPLeGTLZO7Ac7dfMyksNJzfmad6PQ7i2GHQ1ZqK6aVTZOt37_kuGkUz7CzVhBNhsdYVeK-qG331dI66XMxSVYNK5O9poDzH1mAAIG_2MdxAEWHcDstZl6YvmWn5-JEhS6QdB6mFwIDAQAB"
    },
    "errors": null
}
```

For detailed instructions on how to create a machine, refer to the [**Admin Portal Machine Creation Guide**](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/administration/test/admin-portal-user-guide#machines)

{% hint style="info" %}
**Note: “/home/mosip/.mosipkeys“** location must be volume mounted and kept secure as per the best security practices to prevent unauthorised access.
{% endhint %}

**Fetching the Machine ID**

As of now, there is no direct support for fetching the specific machine ID in MOSIP. To retrieve the machine ID, use the API below to get a list of all centres in the system. From this list, manually search for the machine ID associated with the newly created centre for CRVS. The “id“ attribute in the response will be the machine ID.

**Get a List of All Machine Endpoints:** `{domain}/v1/masterdata/machines`

**Method:** GET

Once the **Machine ID** is identified, ensure it is saved securely for future reference. This **Machine ID** will be required in subsequent interactions with the CRVS system for processing requests.

#### 6. Map Officer to Centre and Machine

Once the officer, centre, and machine are created for CRVS, the next step is to map the user to the centre. This ensures the user is properly associated with the correct zone and centre for their operations.

1. **Log in to Admin UI**
   1. Log in to the Admin UI using the **admin** user created in Keycloak if it is a fresh environment.
2. **Map Zone to User**
   1. Navigate to **Resources** → **User Zone Mapping** → **Map Zone**.
   2. Select the **username** for which the zone is to be mapped.
   3. Choose the appropriate **zone** from the dropdown.
   4. Click **Save**.
   5. Refer to the accompanying images for further guidance.
3. **Activate the User**
   1. After mapping the zone, activate the user.
4. **Map Centre to User**
   1. Navigate to **Resources** → **User Centre Mapping**.
   2. Select the **username** for which the centre needs to be mapped.
   3. Choose the appropriate **centre** from the dropdown.
   4. Click **Save**.
   5. Activate the user, as outlined in Step 3.

#### 7. Create the AID

The Application ID (AID) refers to the unique identifier assigned to track the packet that is being processed for events such as birth or death registration. It can be used by MOSIP or CRVS to track the progress and status of the specific event.

**AID Structure(Recommended):**

1. **Centre ID** (First 5 digits): The first 5 digits of the RID represent the **Centre ID**.
2. **Machine ID** (Next 5 digits): The next 5 digits of the RID represent the **Machine ID**.
3. **Random Sequence** (next N digits): The next N digits can be a randomly generated sequence based on the length that the country wants to use for the RID.

**Example of AID:**

For the AID `10001100771006920220128223618`The breakdown is as follows:

1. **Centre ID**: `10001` (First 5 digits)
2. **Machine ID**: `10077` (Next 5 digits)
3. **Random Sequence**: `1006920220128223618` (Remaining 16 digits)

The AID format mentioned above is the recommendation to be followed, but not mandatory. CRVS can generate the AID in any specified format as per their requirement and include it in the Create Packet API Request to ensure proper packet identification and mapping.

Once all the above pre-requisites are in place, the next step is to initiate a request(birth/death/update) by calling the create packet API of MOSIP’s packet manager module. API structure, required fields, and other details are mentioned further in this document.

#### 8. Create Packet API (Packet Manager Module)

**Create Packet Endpoint:** {domain}/commons/v1/packetmanager/createPacket

**Method:** PUT

**API request Structure**:

```json
{
    "id": "string",
    "version": "string",
    "requesttime": "2025-02-25T11:14:17.667Z",
    "request": {
        "id": {{aid}},
        "refId": "10007_10007",
        "offlineMode": false,
        "process": "CRVS_NEW",
        "source": "CRVS1",
        "schemaVersion": "0.3",
        "fields": {
           "fullName": "[ {\n  \"language\" : \"eng\",\n  \"value\" : \"test_update\"\n}]",
         "dateOfBirth": "2023/12/30",
         "gender": "[ {\n  \"language\" : \"eng\",\n  \"value\" : \"Male\"\n}]",
         "addressLine1": "[ {\n  \"language\" : \"eng\",\n  \"value\" : \"addressLine1\"\n}]",
         "addressLine2": "[ {\n  \"language\" : \"eng\",\n  \"value\" : \"addressLine2\"\n}]",
         "city": "[ {\n  \"language\" : \"eng\",\n  \"value\" : \"Bangalore\"\n}]",
         "state": "[ {\n  \"language\" : \"eng\",\n  \"value\" : \"Karnataka\"\n}]",
         "postalCode": "14022",
         "email": "wwe@gmail.com",
         "phone": "7790075085",
         "zone" : "[ {\n  \"language\" : \"eng\",\n  \"value\" : \"Ben Mansour\"\n}]",
         "region" : "[ {\n  \"language\" : \"eng\",\n  \"value\" : \"Rabat Sale Kenitra\"\n}]",
         "province":"[ {\n  \"language\" : \"eng\",\n  \"value\" : \"Kenitra\"\n}]",
      },
       
      "metaInfo": {
         "metaData": "[{\n  \"label\" : \"registrationType\",\n  \"value\" : \"CRVS_NEW\"\n}, {\n  \"label\" : \"machineId\",\n  \"value\" : \"10007\"\n}, {\n  \"label\" : \"centerId\",\n  \"value\" : \"10007\"\n}]",
         "registrationId": {{rid}},
         "operationsData": "[ {\n  \"label\" : \"officerId\",\n  \"value\" : \"crvs\"\n}, {\n  \"label\" : \"officerBiometricFileName\",\n  \"value\" : null\n}, {\n  \"label\" : \"supervisorId\",\n  \"value\" : null\n}, {\n  \"label\" : \"supervisorBiometricFileName\",\n  \"value\" : null\n}, {\n  \"label\" : \"supervisorPassword\",\n  \"value\" : \"false\"\n}, {\n  \"label\" : \"officerPassword\",\n  \"value\" : \"true\"\n}, {\n  \"label\" : \"supervisorPIN\",\n  \"value\" : null\n}, {\n  \"label\" : \"officerPIN\",\n  \"value\" : null\n}, {\n  \"label\" : \"supervisorOTPAuthentication\",\n  \"value\" : \"false\"\n}, {\n  \"label\" : \"officerOTPAuthentication\",\n  \"value\" : \"false\"\n} ]",
         "capturedRegisteredDevices": "[]",
         "creationDate": "20250225110733"			
      },
      
      
      "audits": [
         {
            "uuid": "c75s4521-87d6-6a4x-balw-2432e2355440",
            "createdAt": "2025-02-25T13:22:49.214Z",
            "eventId": "REG-EVT-066",
            "eventName": "PACKET_CREATION_SUCCESS",
            "eventType": "USER",
            "hostName": "DESKTOP-JL4BAEV",
            "hostIp": "localhost",
            "applicationId": "REG",
            "applicationName": "REGISTRATION",
            "sessionUserId": "crvs",
            "sessionUserName": "crvs",
            "id": {{aid}},
            "idType": "REGISTRATION_ID",
            "createdBy": "crvs",
            "moduleName": "Packet Handler",
            "moduleId": "REG-MOD-117",
            "description": "Packet Successfully Created",
            "actionTimeStamp": "2025-02-25T07:52:49.214Z"
         }
        ],   
      "schemaJson":"{\"$schema\": \"http://json-schema.org/draft-07/schema#\",\"description\": \"MOSIP Sample identity\",\"additionalProperties\": false,\"title\": \"MOSIP identity\",\"type\": \"object\",\"definitions\": {\"simpleType\": {\"uniqueItems\": true,\"additionalItems\": false,\"type\": \"array\",\"items\": {\"additionalProperties\": false,\"type\": \"object\",\"required\": [\"language\",\"value\"],\"properties\": {\"language\": {\"type\": \"string\"},\"value\": {\"type\": \"string\"}}}},\"documentType\": {\"additionalProperties\": false,\"type\": \"object\",\"properties\": {\"format\": {\"type\": \"string\"},\"type\": {\"type\": \"string\"},\"value\": {\"type\": \"string\"},\"refNumber\": {\"type\": [\"string\",\"null\"]}}},\"biometricsType\": {\"additionalProperties\": false,\"type\": \"object\",\"properties\": {\"format\": {\"type\": \"string\"},\"version\": {\"type\": \"number\",\"minimum\": 0},\"value\": {\"type\": \"string\"}}}},\"properties\": {\"identity\": {\"additionalProperties\": false,\"type\": \"object\",\"required\": [\"IDSchemaVersion\",\"fullName\",\"dateOfBirth\",\"gender\",\"addressLine1\",\"addressLine2\",\"addressLine3\",\"region\",\"province\",\"city\",\"zone\",\"postalCode\",\"phone\",\"email\",\"proofOfIdentity\",\"individualBiometrics\"],\"properties\": {\"proofOfAddress\": {\"bioAttributes\": [],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/documentType\"},\"gender\": {\"bioAttributes\": [],\"fieldCategory\": \"pvt\",\"format\": \"\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/simpleType\"},\"city\": {\"bioAttributes\": [],\"validators\": [{\"validator\": \"^(?=.{0,50}$).*\",\"arguments\": [],\"type\": \"regex\"}],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/simpleType\"},\"postalCode\": {\"bioAttributes\": [],\"validators\": [{\"validator\": \"^[(?i)A-Z0-9]{5}$|^NA$\",\"arguments\": [],\"type\": \"regex\"}],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"type\": \"string\",\"fieldType\": \"default\"},\"proofOfException-1\": {\"bioAttributes\": [],\"fieldCategory\": \"evidence\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/documentType\"},\"referenceIdentityNumber\": {\"bioAttributes\": [],\"validators\": [{\"validator\": \"^([0-9]{10,30})$\",\"arguments\": [],\"type\": \"regex\"}],\"fieldCategory\": \"pvt\",\"format\": \"kyc\",\"type\": \"string\",\"fieldType\": \"default\"},\"individualBiometrics\": {\"bioAttributes\": [\"leftEye\",\"rightEye\",\"rightIndex\",\"rightLittle\",\"rightRing\",\"rightMiddle\",\"leftIndex\",\"leftLittle\",\"leftRing\",\"leftMiddle\",\"leftThumb\",\"rightThumb\",\"face\"],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/biometricsType\"},\"province\": {\"bioAttributes\": [],\"validators\": [{\"validator\": \"^(?=.{0,50}$).*\",\"arguments\": [],\"type\": \"regex\"}],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/simpleType\"},\"zone\": {\"bioAttributes\": [],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/simpleType\"},\"proofOfDateOfBirth\": {\"bioAttributes\": [],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/documentType\"},\"addressLine1\": {\"bioAttributes\": [],\"validators\": [{\"validator\": \"^(?=.{0,50}$).*\",\"arguments\": [],\"type\": \"regex\"}],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/simpleType\"},\"addressLine2\": {\"bioAttributes\": [],\"validators\": [{\"validator\": \"^(?=.{3,50}$).*\",\"arguments\": [],\"type\": \"regex\"}],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/simpleType\"},\"residenceStatus\": {\"bioAttributes\": [],\"fieldCategory\": \"kyc\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/simpleType\"},\"addressLine3\": {\"bioAttributes\": [],\"validators\": [{\"validator\": \"^(?=.{3,50}$).*\",\"arguments\": [],\"type\": \"regex\"}],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/simpleType\"},\"email\": {\"bioAttributes\": [],\"validators\": [{\"validator\": \"^[A-Za-z0-9_\\\\-]+(\\\\.[A-Za-z0-9_]+)*@[A-Za-z0-9_-]+(\\\\.[A-Za-z0-9_]+)*(\\\\.[a-zA-Z]{2,})$\",\"arguments\": [],\"type\": \"regex\"}],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"type\": \"string\",\"fieldType\": \"default\"},\"introducerRID\": {\"bioAttributes\": [],\"fieldCategory\": \"evidence\",\"format\": \"none\",\"type\": \"string\",\"fieldType\": \"default\"},\"introducerValidationToken\": {\"bioAttributes\": [],\"fieldCategory\": \"evidence\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/simpleType\"},\"introducerValidationTokenDetails\": {\"bioAttributes\": [],\"fieldCategory\": \"evidence\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/documentType\"},\"introducerBiometrics\": {\"bioAttributes\": [\"leftEye\",\"rightEye\",\"rightIndex\",\"rightLittle\",\"rightRing\",\"rightMiddle\",\"leftIndex\",\"leftLittle\",\"leftRing\",\"leftMiddle\",\"leftThumb\",\"rightThumb\",\"face\"],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/biometricsType\"},\"fullName\": {\"bioAttributes\": [],\"validators\": [{\"validator\": \"^(?=.{3,50}$).*\",\"arguments\": [],\"type\": \"regex\"}],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/simpleType\"},\"dateOfBirth\": {\"bioAttributes\": [],\"validators\": [{\"validator\": \"^(1869|18[7-9][0-9]|19[0-9][0-9]|20[0-9][0-9])/([0][1-9]|1[0-2])/([0][1-9]|[1-2][0-9]|3[01])$\",\"arguments\": [],\"type\": \"regex\"}],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"type\": \"string\",\"fieldType\": \"default\"},\"individualAuthBiometrics\": {\"bioAttributes\": [\"leftEye\",\"rightEye\",\"rightIndex\",\"rightLittle\",\"rightRing\",\"rightMiddle\",\"leftIndex\",\"leftLittle\",\"leftRing\",\"leftMiddle\",\"leftThumb\",\"rightThumb\",\"face\"],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/biometricsType\"},\"introducerUIN\": {\"bioAttributes\": [],\"fieldCategory\": \"evidence\",\"format\": \"none\",\"type\": \"string\",\"fieldType\": \"default\"},\"proofOfIdentity\": {\"bioAttributes\": [],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/documentType\"},\"IDSchemaVersion\": {\"bioAttributes\": [],\"fieldCategory\": \"none\",\"format\": \"none\",\"type\": \"number\",\"fieldType\": \"default\",\"minimum\": 0},\"proofOfException\": {\"bioAttributes\": [],\"fieldCategory\": \"evidence\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/documentType\"},\"phone\": {\"bioAttributes\": [],\"validators\": [{\"validator\": \"^[+]*([0-9]{1})([0-9]{9})$\",\"arguments\": [],\"type\": \"regex\"}],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"type\": \"string\",\"fieldType\": \"default\"},\"introducerName\": {\"bioAttributes\": [],\"fieldCategory\": \"evidence\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/simpleType\"},\"proofOfRelationship\": {\"bioAttributes\": [],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/documentType\"},\"UIN\": {\"bioAttributes\": [],\"fieldCategory\": \"none\",\"format\": \"none\",\"type\": \"string\",\"fieldType\": \"default\"},\"preferredLang\": {\"bioAttributes\": [],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"type\": \"string\",\"fieldType\": \"dynamic\"},\"region\": {\"bioAttributes\": [],\"validators\": [{\"validator\": \"^(?=.{0,50}$).*\",\"arguments\": [],\"type\": \"regex\"}],\"fieldCategory\": \"pvt\",\"format\": \"none\",\"fieldType\": \"default\",\"$ref\": \"#/definitions/simpleType\"}}}}}"
    }
}
```

{% hint style="info" %}
**Note**: The API request shared above is only a sample and is not to be used for any implementation.
{% endhint %}

#### **Field Descriptions**

#### **Request Object**

1. `source`: Specifies the source of the registration request. This will be the same for any request that comes to MOSIP for birth or death.
2. `process`: Identifies the specific process for the registration.
   * `CRVS_NEW` - When initiating an infant birth request
   * `CRVS_DEATH` - When initiating a death registration request
3. `id`The unique identifier for the registration request(AID).

{% hint style="info" %}
**Note**: As per the current implementation, if the same AID is used twice, the record will be updated with the latest request data.
{% endhint %}

* `ref_id:` Combination of centre ID and machine ID.
  * Ex - “centerid\_machineid”
* `schema_version`The version of the ID schema that the country is using in production.

**Field object:**

1. `fullName`: The full name of the individual.
2. `dateOfBirth`: The date of birth of the individual.
3. `gender`: Gender of the individual.
4. `addressLine1`: First line of the address.
5. `addressLine2`: Second line of the address.
6. `city`: City of residence.
7. `state`: State of residence.
8. `postalCode`: Postal code of the address.
9. `email`: Email address.
10. `phone`: Contact phone number.
11. `zone`: Geographic zone.
12. `region`: Region of the address.
13. `province`: Province of residence.
14. Additional fields
    1. Birth registration field:
       1. `introducerInfoToken`: Introducer’s eSignet user info jwt token, to be passed when sending a request for the birth registration
    2. Death registration field:
       1. `deceasedInformer`: Informant’s eSignet user info jwt token, to be passed when sending a request for the death registration
       2. `deceasedDeclarationDate`: The date on which the individual was declared deceased.
       3. `declaredAsDeceased`: A flag indicating that the individual has been officially marked as deceased.
       4. `typeOfDeath`: Specifies the nature of the death, such as _natural_ or _jurisdictional_.

**MetaInfo Object (Center and Operator Information):**

1. `centerId`: Unique identifier for the centre where the registration is processed.
2. `machineId`: Unique identifier for the machine used for registration.
3. `operationsData`: Contains fields such as officer ID, officer password, supervisor ID, etc.
4. `registration_type`: This is the value same as the process field for birth(`CRVS_NEW`) or death(`CRVS_DEATH`).

`centerId`, `machineId`, `officerId` must be provided along with any additional relevant operational information for the request to be processed.

**Audit Object:**

1. `uuid`: Unique uuid to be sent with any request coming from CRVS.
2. Please add the values for the other fields in the audit object as per the details of the machine and the person who is registering the request from the CRVS.

It is required that at least one attribute in the audit object is populated with valid data before making the request.

`schemaJson`: JSON schema (in stringified format) used by the country

**Sample Response:**

```json
{
    "id": "mosip.registration.packet.writer",
    "version": "v1",
    "responsetime": "2025-04-07T13:38:09.184Z",
    "metadata": null,
    "response": [
        {
            "id": "8239749062",
            "packetName": "8239749062_evidence",
            "source": "CRVS1",
            "process": "CRVS_NEW",
            "refId": "10018_10084",
            "schemaVersion": "0.3",
            "signature": "RGvN6tYDSvwxP6LAs1Qng1jLkRi1MASxRkB5Vu-EtnQx2R2VGVInhysPWaBrILi_bMKcHFCV8JI8FlQaf4NuKJ313wsAs_jVJSMCgUjaPoeyaEV0mUW90-BulREHqNfSuLzYY1cBmomiSNf1qRMkxwKOG2phWoJuigFEa2UjCWZffldMAGtNs5eXMWuglcjLCJFw7KPxDo3tGD6u92JN35F4te0R5_fN46XaaLDgkfWhBRuCDS0mJKprYXOnQaXQPGdNorxuNE7_emFxWKafu59TDowa--DDv6c1sbOG3mePOG4_QW8RhKOt8r9oLrU1nhkEuksAAW8zL3fZ7FmTPQ",
            "encryptedHash": "u7BUoTU5CqCQTe1BeLReRaWMcB7-FsfoxLoj68FMUhw",
            "providerName": "PacketWriterImpl",
            "providerVersion": "v1.0",
            "creationDate": "2025-04-07T13:38:07.769757Z"
        },
        {
            "id": "8239749062",
            "packetName": "8239749062_optional",
            "source": "OPENCRVS",
            "process": "CRVS_NEW",
            "refId": "10018_10084",
            "schemaVersion": "0.3",
            "signature": "fShMATi2V0oTtk6qhjJxn-XXbzEfQttavH676-kyjF90WLos6cwA9oyAv-gvRAScNoREUjT-Zj6z-RltTwCoRVkU6jeqzWFODnfoPtxp13Adsj2tHhp94gaerzBDvKxUxjn7ecM2I9FJb4kt5M-a0YSI_VtlTFfhTTfIvxXBnDEPGT6FxpDho_wlsKwXs1uigRPYKnXDeP7Sxy3AgvVkAkcNKCmBF5skzOqJ18bibpQU8q2l9dW5Anxy54CwRsvXhJRq43sWTm5CP3xHa5dBFDrS2qa3BZQ8Igt9BzEJoLBptTLm5wyA3aCJ_GHe_8ekWxkTFChDJX9-5UwPmxfWeA",
            "encryptedHash": "-kMmslv2AWMMMdmXykoFP3kr0mW0m3X1eFMEl_2LQRk",
            "providerName": "PacketWriterImpl",
            "providerVersion": "v1.0",
            "creationDate": "2025-04-07T13:38:08.283129Z"
        },
        {
            "id": "8239749062",
            "packetName": "8239749062_id",
            "source": "CRVS1",
            "process": "CRVS_NEW",
            "refId": "10018_10084",
            "schemaVersion": "0.3",
            "signature": "hK_BiKShkXyRn6Pv3PRXLNkagF4WQYwX-9INbD8u45vEPIIdXmaz1o6Z-8SKx6VvEpUYePFDuhpRZJMLg3giNYvpRIM1U8T4tAXp_P6XEgOvfUivqXgILMkQWNkM-Bk6ygzNV1GN5RrTUoi2vTxuljlyN94IOCpQNFGap9l1Ge_Ab278Y0ofCCzmXM9vWnew-sJ5w4hxagKvIn2DIsoNfb8RbMD2n6ebm8LP1uKAdpwTRx0HOTJyxW-svTjdFzYP_n5qi0Ggqa6V7x28DD8b43XPGusNJAlG3PJy06aB6IVhq9P1GwFn2QMm_-2U60mJlvq7VsoBrx7WWEa8rree4w",
            "encryptedHash": "qBaCdSxAlOtuhcwnG7JojI4WzloHhe_9h2nk6b6LM64",
            "providerName": "PacketWriterImpl",
            "providerVersion": "v1.0",
            "creationDate": "2025-04-07T13:38:08.687475Z"
        }
    ],
    "errors": []
}

```

9. **Trigger API (Registration Processor Module)**

In MOSIP, after a packet is created, it is processed for validations and verification of the information in the **Registration Processor**. Inside the Registration Processor, each packet follows a specific workflow defined by the **Camel route**.

For the integration with CRVS, the newly created packet is uploaded to the Object Store. To pick up this new packet and trigger the processing, we have developed a new API. This API ensures to trigger the appropriate workflow is triggered for further processing of the registration packet. For this integration, the camel route workflow to be executed is determined by the values provided for the **source** and **process**.
\
API Documentatio&#x6E;**:** [Create workflow instance for packet processing. | Registration processor](https://mosip.stoplight.io/docs/registration-processor/branches/main/d56c892cfa950-create-workflow-instance-for-packet-processing)
