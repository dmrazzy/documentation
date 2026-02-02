---
description: Configuration and runbook topics for MOSIP–CRVS integration.
---

# Prerequisites, Configurations and Operations

## Overview

MOSIP, as a modular identity platform, utilizes the [Registration Client](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/registration-client) to collect essential information from individuals. This data is then used to generate a registration packet, which is uploaded to the [Registration Processor](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/registration-processor/overview). Once the packet passes all required validations, a national ID is generated for the individual.

To enhance this process and support diverse entry points for national ID requests, including those from external systems such as CRVS platforms, MOSIP exposes a set of APIs through its [Packet Manager](https://docs.mosip.io/1.2.0/id-lifecycle-management/supporting-components/packet-manager) module. These APIs enable external systems to create and upload registration packets directly, bypassing the core registration workflow, and thus offering a streamlined integration pathway.

For every registration request, an [ID Schema](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-management/id-schema) must be defined. This schema, based on a standardized JSON structure, outlines the fields and data elements to be stored in MOSIP’s Identity Repository. It applies to all incoming requests from CRVS systems, such as birth registrations, demographic updates, or death notifications. The schema is flexible and can be customized to align with the specific needs of a country’s identity program.

## Prerequisites

To successfully integrate with MOSIP’s registration process, external systems must fulfill certain prerequisites. These requirements ensure proper alignment, security, and functionality of the integration. Key pre-requisites include:

#### **Create a New Client and Assign a Role in Keycloak**&#x20;

[**Keycloak**](https://docs.mosip.io/1.2.0/id-lifecycle-management/supporting-components/keycloak) is an identity and access management tool utilized by MOSIP. Use Keycloak, to create a new client for the external system (e.g., CRVS).

Generate a unique client ID and client secret.

1. Define a specific role (e.g., `CRVS_BIRTH_REGISTRATION_ROLE`) that reflects the intended function.
2. Assign this role to the newly created client to enable permissioned access to relevant APIs.

[Read More](prerequisites/security-and-authentication.md)

#### **Obtain Access Token for API Calls**

Authenticate the CRVS system by calling the Keycloak token endpoint using the client credentials.

* Retrieve a valid access token, which must be included in all subsequent API calls to MOSIP for authentication and authorization.

[Read More](prerequisites/security-and-authentication.md)

### Operational Considerations

#### **Set Up a Registration Centre**

Define and register a unique Centre ID in the system.

* This Centre ID should represent the CRVS registration location initiating the request.
  * It will be used to track and manage packet submissions by location.

[Read More](operational-considerations/create-centre.md)

#### **Register a Machine** &#x20;

Create a unique Machine ID and corresponding key.

* This ID will identify the hardware or system used for submitting requests to MOSIP.
* The key ensures that only authorized machines can interact with MOSIP services.

[Read More](operational-considerations/create-machine.md)

#### **Create a Default Officer Profile**

Set up a default officer or operator ID who will be responsible for sending registration requests on behalf of the CRVS system.

* This officer represents the actor initiating the transaction from the CRVS interface.

[Read More](operational-considerations/create-the-default-officer.md)

#### **Map Officer to Centre and Machine**

Establish mappings to link the created Officer ID with the relevant Centre ID and Machine ID.

* This step ensures that the officer is correctly associated with a specific registration centre and the hardware device authorized to perform registrations.

[Read More](./#map-officer-to-centre-and-machine)

#### **Generate and Use a Unique Application ID (AID)**

For each registration event (e.g., birth or death), the CRVS system must generate a unique **Application ID (AID).**

* The AID should be included in the packet submitted to MOSIP.
* It will serve as the reference ID for tracking the request and receiving response events via WebSub.

[Read More](operational-considerations/create-the-application-id-aid.md)

## CRVS Onboarding for Authentication

This section outlines the **onboarding requirements that the CRVS system must complete, along with the preprequisites defined above, to be able to leverage eSignet for MOSIP ID authentication**:

* **eSignet Onboarding** — If eSignet is used for authentication, the CRVS system must complete the onboarding process and acquire the necessary keys and certificates. Refer to [**Integrate with eSignet**](https://app.gitbook.com/s/ylzvZHp30DQ3rNCClELV/esignet-authentication/test/try-it-out/integrate-with-e-signet) for detailed instructions.
* **Authentication Partner Onboarding** — The CRVS system must be registered as an authentication partner with MOSIP. This requires the necessary certificates for onboarding. Refer to the [**Partner Management Services – End User Guide**](../../../../id-lifecycle-management/support-systems/partner-management-services/functional-overview/end-user-guide.md)

For detailed configuration and step-by-step instructions for each pre-requisite, please refer here (Link to operation Consideration page)

