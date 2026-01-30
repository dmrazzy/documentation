# Core Integration Principles

### CRVS as Source of Truth

**CRVS is considered the source of truth**. MOSIP relies on CRVS to perform deduplication and treats CRVS as the authoritative source of vital event data. MOSIP does not perform de-duplication or data validation on requests from CRVS systems because of this trust relationship.

**Rationale:** CRVS is the legally authorized system for registering and certifying vital life events. It has the institutional mandate, legal authority, and statutory processes to verify that a life event occurred. As a foundational identity system, MOSIP trusts CRVS for the accuracy, uniqueness, and validity of vital event data. This establishes a clear authority boundary where MOSIP relies on CRVS-validated events and does not repeat event verification or event-level deduplication (except for the rare/manual scenarios described below).

**Assumption:** The CRVS system is legally authorized to issue official birth and death certificates and is institutionally capable of performing the required verification and deduplication in accordance with the laws of the country.

> **See Section** [**integration-principles-boundaries-and-real-world-implications**](integration-principles-boundaries-and-real-world-implications.md) for a detailed explanation of why Foundational ID and CRVS systems maintain distinct responsibilities and authorities.

### **Integration Data Exchange & Verification**

1. The initiation of the request with the data will be from CRVS to MOSIP
2. Data transmitted from CRVS to MOSIP will undergo thorough data/document verification and deduplication before any request is initiated for MOSIP to process.
3. MOSIP will trust all information provided by CRVS and honor the received requests. MOSIP will ensure that the technical and logical validations are all in place for request processing.
4. The integration defines a set of mandatory attributes to be exchanged between the two systems. By default, MOSIP specifies a core set of mandatory attributes that CRVS systems must share to enable MOSIP to initiate packet processing.

{% hint style="info" %}
Note - In addition to these default attributes, the integration **supports customization**, allowing countries to modify, extend, or tailor the set of mandatory attributes based on country-specific legal frameworks, policies, or implementation requirements.
{% endhint %}

5. **Exception for Rare Scenarios**: For sensitive cases such as identity deactivation in case of [Fraudulent Birth Registration - National ID Deactivation Request](../integration-patterns-and-workflow/rare-scenarios/fraudulent-birth-registrations-national-id-deactivation-request-from-crvs.md), [identity reactivation](../integration-patterns-and-workflow/rare-scenarios/reactivation-of-deactivated-national-id.md) , [death flag reversals](../integration-patterns-and-workflow/rare-scenarios/fraud-death-case-reversal-of-the-death-flag.md) or more, MOSIP does not automatically process requests. Instead, these are routed to manual verification queues.

### Identity Lifecycle Management

### De-duplication Strategy

As for the default integration mosip does not perforam deduplication, This essentially means MOSIP relies on CRVS to perform deduplication and treats CRVS as the source of truth.&#x20;

**Exception for Rare Scenarios**: For fraud detection, reactivation, and death reversal requests (Rare Scenarios), MOSIP enforces a **one-time request policy** per National ID. If a fraud flag or reversal request is already in progress or has been previously processed, subsequent duplicate requests are automatically rejected. This prevents repeated requests from creating endless loops and maintains system integrity.

### Authentication & Authorization Model

#### Proof of Authentication

1. For any request submitted to MOSIP, it is essential to include the biometric data of the informant or applicant, whenever feasible, to ensure the accurate verification of the individual initiating the request and confirm their legitimacy.
2. For cases where biometric data is not collected by CRVS, it is recommended for CRVS to use eSignet for the applicant or introducer authentication. Post-authentication user info token should be shared with MOSIP for validation and auditing purposes.

### Notification & Event Architecture

#### Identity Credential Sharing & Packet status

1. The identity credential sharing and packet status updates are two separate MOSIP integration points for any CRVS.
2. MOSIP will generate and share identity credentials for new registration cases via WebSub. Any partner configured according to the policy will receive these notifications automatically.
3. If CRVS requires identity credentials to be shared by MOSIP during identity generation, MOSIP can return such credentials to CRVS. By default, MOSIP recommends sharing the PSUT token to minimize exposure of sensitive identity data. While sharing UIN or VID is supported to meet country-specific requirements, it is not recommended and should be done only when legally mandated.

#### Multiple CRVS Systems

1. If a country deploys multiple CRVS systems, each system will be treated as a separate partner for MOSIP. This allows for independent management and integration with each CRVS system.
2. Credentials generated from MOSIP will be sent to all the configured credential partners subscribed to the websub as per policy. It is the responsibility of the country and CRVS systems to manage the sharing of information across multiple CRVS systems.

{% hint style="info" %}
**Note:** In this document, the term **MOSIP ID** is used to refer to the foundational or national identity issued through MOSIP. The terms **MOSIP ID**, **foundational ID**, **national ID**, and **UIN (Unique Identity Number)** are used interchangeably and denote the same identity, unless stated otherwise.
{% endhint %}

***

### Learn More

* [Core Integration Principles](core-integration-principles.md)
* [Integration Boundaries, Limitations and Implications](integration-principles-boundaries-and-real-world-implications.md)
