# MOSIP-CRVS Integration Guide

**Document Version**: 1.0  
**Last Updated**: 22 January 2026  
**Status**: Reorganized per Information Architecture v2.0

---

This has missing content added.

## Table of Contents

1. [Introduction & Context](#1-introduction--context)
2. [Integration Overview](#2-integration-overview)
3. [Core Integration Principles](#3-core-integration-principles)
4. [Integration Patterns & Workflows](#4-integration-patterns--workflows)
5. [Technical Architecture](#5-technical-architecture)
6. [API Reference & Data Models](#6-api-reference--data-models)
7. [Security & Authentication](#7-security--authentication)
8. [Notifications & Event Handling](#8-notifications--event-handling)
9. [Error Handling & Reconciliation](#9-error-handling--reconciliation)
10. [Policy Configuration & Customization](#10-policy-configuration--customization)
11. [Operational Considerations](#11-operational-considerations)
12. [Testing & Validation](#12-testing--validation)
13. [Code Examples & Reference Implementations](#13-code-examples--reference-implementations)
14. [Troubleshooting Guide](#14-troubleshooting-guide)
15. [Appendices & References](#15-appendices--references)

---

# 1. Introduction & Context

## 1.1 Purpose & Audience

### Purpose

This document provides comprehensive technical guidance for integrating Civil Registration and Vital Statistics (CRVS) systems with the MOSIP platform. It covers architecture, APIs, workflows, security, and operational procedures required to establish a secure, scalable integration that enables automated identity lifecycle management from birth to death.

### Target Audience

This guide is intended for:

- **System Integrators & Developers**: Responsible for implementing the technical integration between CRVS and MOSIP systems
- **Technical Architects**: Designing the integration architecture and making technology decisions
- **CRVS Application Developers**: Building or customizing CRVS applications to integrate with MOSIP
- **MOSIP Administrators**: Configuring and managing MOSIP for CRVS integration
- **Project Managers**: Understanding scope, timelines, and dependencies
- **Security Teams**: Implementing authentication, authorization, and compliance requirements

### Prerequisites

Readers should have:

- Working knowledge of MOSIP platform and its core modules
- Understanding of RESTful APIs and JSON data formats
- Familiarity with OAuth2 authentication and Keycloak
- Basic understanding of CRVS workflows and vital event registration
- Experience with integration design patterns and event-driven architectures


## 1.2 Integration Value Proposition

MOSIP can integrate with CRVS (Civil Registration and Vital Statistics) systems to create a unified digital identity ecosystem that spans an individual's entire life cycle.

**Civil Registration and Vital Statistics (CRVS)** systems are essential for documenting key life events such as births, deaths, marriages, and divorces. These systems provide individuals with legal recognition and play a vital role in ensuring access to rights and services. Additionally, CRVS systems generate important demographic data that supports effective policy-making, governance, and resource planning.

**MOSIP** is an open-source, modular identity platform designed for scalability, interoperability, and compliance with global standards. It allows countries to customize identity systems tailored to their governance needs. To learn more about MOSIP, click [here](https://docs.mosip.io/1.2.0/id-lifecycle-management).

This synergy allows for the seamless management of an individual's identity across life events, starting from birth registration, through education and employment, to end-of-life verification.

> Note: When these systems operate in isolation, it leads to administrative inefficiencies, fragmented data, and challenges in service access.

## 1.3 Document Navigation Guide

### How to Use This Document

This guide follows a progressive disclosure structure, organized from conceptual understanding to detailed implementation.

**Reading Paths:**

1. **Quick Start Path** (For experienced MOSIP developers):
   - Section 1.2: Value Proposition
   - Section 2.3: Supported Use Cases
   - Section 4: Integration Workflows
   - Section 6: API Reference
   - Section 7: Security Setup

2. **Comprehensive Path** (For new integrators):
   - Read sections 1-4 for context and principles
   - Study sections 5-7 for technical architecture and security
   - Follow sections 11-12 for setup and testing
   - Reference sections 6, 14, 15 as needed

3. **Architecture Path** (For architects and decision-makers):
   - Sections 1-3: Context and principles
   - Section 5: Technical Architecture
   - Section 9: Error Handling
   - Section 11.3: Performance & Scalability

4. **Operations Path** (For administrators):
   - Section 7: Security & Authentication
   - Section 11: Operational Considerations
   - Section 14: Troubleshooting

### Document Conventions

- **Bold text**: Important terms, UI elements, or emphasis
- `Code formatting`: API endpoints, field names, technical values
- > **Note**: Additional information or clarifications
- ⚠️ **Warning**: Critical information about limitations or risks
- ✅ **Best Practice**: Recommended approaches

### Cross-References

- Links in blue navigate to related sections within this document
- External links direct to MOSIP documentation or specifications
- See Section 15.2 for related MOSIP documentation

## 1.4 Terminology & Glossary

### CRVS Terms

| **Term** | **Definition** |
|---------|---------------|
| **CRVS** | Civil Registration and Vital Statistics - System for recording vital events (birth, death, marriage, divorce) |
| **CRA** | Civil Registration Authority - Government body responsible for vital event registration |
| **Informant** | Person reporting a vital event (birth, death) to civil authorities |
| **Vital Event** | Key life occurrence requiring official registration (birth, death, marriage, divorce) |
| **Birth Certificate** | Official document issued by CRVS confirming birth registration |
| **Death Certificate** | Official document issued by CRVS confirming death registration |

### MOSIP Terms

| **Term** | **Definition** |
|---------|---------------|
| **UIN** | Unique Identification Number - Permanent identifier assigned to individuals |
| **VID** | Virtual ID - Temporary, revocable identifier linked to UIN for privacy protection |
| **AID/RID** | Application ID / Registration ID - Unique identifier for tracking registration packets |
| **Packet** | Data structure containing demographic/biometric information for identity registration |
| **eSignet** | MOSIP's authentication service for identity verification |
| **Introducer** | Registered individual with valid UIN who vouches for someone without biometrics (e.g., parent for infant) |
| **PSUT** | Print Secure UIN Token - Secure token for printing eUIN cards without exposing actual UIN |
| **WebSub** | Publish-subscribe protocol for real-time event notifications |
| **ID Schema** | JSON schema defining structure and validation rules for identity data |
| **Object Store** | Storage system for registration packets and documents |
| **Registration Processor** | MOSIP module that validates and processes registration packets |
| **Packet Manager** | MOSIP module that creates and manages registration packets |

### Integration-Specific Terms

| **Term** | **Definition** |
|---------|---------------|
| **Source of Truth** | System considered authoritative for specific data (CRVS for vital events) |
| **Deduplication** | Process of identifying and preventing duplicate identity records |
| **Credential Sharing** | Secure transmission of UIN/VID/PSUT token from MOSIP to CRVS |
| **Partner Policy** | Configuration defining data sharing rules between MOSIP and partners |
| **Centre ID** | Unique identifier for registration centers in MOSIP |
| **Machine ID** | Unique identifier for devices used in registration process |
| **Officer ID** | Unique identifier for registration officers in MOSIP |
| **Camel Route** | Workflow definition for packet processing in MOSIP |

### Status and Lifecycle Terms

| **Term** | **Definition** |
|---------|---------------|
| **Infant** | Individual aged 0-5 years (configurable) without biometric enrollment |
| **Minor** | Individual below legal age of majority (country-specific) |
| **Adult** | Individual above configured age threshold with biometric enrollment |
| **Deceased Flag** | Status indicator marking an identity as deceased (UIN remains active) |
| **Packet Status** | Processing state of registration packet (Received, Processing, Completed, Rejected) |



---

# 2. Integration Overview

## 2.1 CRVS and MOSIP Ecosystem

<div align="right"><figure><img src="../../../.gitbook/assets/CRVS (1).png" alt=""><figcaption><p>Birth and Death Registration</p></figcaption></figure></div>

The MOSIP-CRVS integration creates a unified identity ecosystem where vital event registration in CRVS automatically triggers identity lifecycle management in MOSIP. This integration bridges two critical government systems:

### System Components

**CRVS System:**
- **Frontend**: Registration portals for CRA officers to capture vital event data
- **Backend**: Business logic for vital event validation and certificate issuance
- **Database**: Repository of birth, death, and other vital event records
- **Certificate Generation**: Module for producing official birth/death certificates

**MOSIP Platform:**
- **Partner Management**: Onboards and manages CRVS as trusted partner
- **Packet Manager**: Creates registration packets from CRVS data
- **Registration Processor**: Validates and processes packets through workflows
- **Identity Repository**: Stores demographic and biometric identity data
- **eSignet**: Authenticates informants/introducers
- **WebSub Hub**: Publishes real-time notifications to CRVS
- **Credential Service**: Generates and shares UIN/VID/PSUT tokens

### Data Flow

1. **Birth/Death Event Occurs** → Informant reports to CRA
2. **CRVS Captures Data** → Officer validates and enters information
3. **Authentication** → eSignet verifies informant/introducer identity
4. **Packet Creation** → CRVS calls Packet Manager API with event data
5. **Workflow Trigger** → CRVS initiates processing via Trigger API
6. **Validation & Processing** → Registration Processor validates packet
7. **Identity Update** → MOSIP creates UIN (birth) or updates status (death)
8. **Credential Sharing** → WebSub notifies CRVS of completion
9. **Certificate Issuance** → CRVS issues certificate with UIN reference

### Integration Touchpoints

**Synchronous APIs:**
- Create Packet API (CRVS → MOSIP)
- Trigger API (CRVS → MOSIP)
- Authentication API (CRVS → eSignet)

**Asynchronous Events:**
- Credential Issuance Notification (MOSIP → CRVS via WebSub)
- Packet Status Updates (MOSIP → CRVS via WebSub)

**Configuration Interfaces:**
- Partner Management Portal (onboarding)
- Admin Portal (centre/machine setup)
- Keycloak Admin Console (OAuth2 clients)

### System Boundaries

**CRVS Responsibilities:**
- Vital event data collection and validation
- Deduplication before sending to MOSIP
- eSignet authentication integration
- WebSub subscription and notification handling
- Certificate generation and issuance

**MOSIP Responsibilities:**
- Identity data validation and storage
- UIN generation and lifecycle management
- Credential generation and secure sharing
- Technical validation and processing
- Notification publishing via WebSub

**Shared Responsibilities:**
- Field mapping agreement
- Security and encryption standards
- Error handling and reconciliation
- Performance monitoring

## 2.2 Integration Scope & Boundaries

### What's Included in This Integration

This integration covers the **bidirectional exchange of identity and vital event data** between CRVS and MOSIP systems:

**✅ In Scope:**

1. **Birth Registration Flow**
   - Infant birth registration (0-5 years, configurable)
   - UIN generation and issuance
   - Credential sharing via WebSub
   - Introducer authentication via eSignet

2. **Death Registration Flow**
   - Death event reporting using UIN/VID
   - Identity status update (deceased flag)
   - Informant authentication via eSignet
   - Status notification to CRVS

3. **Demographic Update Flow** (Limited)
   - Infant demographic updates (pre-biometric)
   - Adult demographic updates (limited scenarios)

4. **Technical Integration**
   - OAuth2 authentication for API access
   - WebSub notifications for events
   - Packet creation and processing APIs
   - Error handling and status tracking

### Integration Depth

**MOSIP consumes from CRVS:**
- Vital event data (birth/death)
- Demographic information
- eSignet authentication tokens
- Introducer/Informant identifiers

**CRVS consumes from MOSIP:**
- UIN/VID/PSUT tokens
- Identity credentials
- Packet processing status
- Error notifications

### System Boundaries

**CRVS Retains:**
- Vital event record management
- Certificate generation and printing
- Vital statistics reporting
- Historical vital event data
- Marriage/divorce registration (not integrated)

**MOSIP Retains:**
- Identity data storage and lifecycle
- Biometric enrollment and deduplication
- Authentication services
- Credential management
- Adult registration workflows

### Out-of-Scope Elements

**Not Included:**
- Marriage registration integration
- Divorce registration integration
- CRVS statistical reporting to MOSIP
- Biometric capture from CRVS centers
- Direct database synchronization
- Offline/disconnected mode integration
- Bulk migration of historical data

## 2.3 Supported Use Cases

Integration can be done for following scenarios:

- Birth Registration and UIN Issuance
- Death Registration and Identity Update
- Demographic Updates (e.g., Name Change, Address Update)

Must read: [Refer to 'What is in scope of integration (integration-depth) and what is kept from integration, and why!](link)

### Use Cases Supported Through CRVS-MOSIP Integration

This integration currently supports the following use cases:

1. [Birth registration](#42-birth-registration--uin-issuance)
  1. Birth registration is initiated by the CRVS system
  2. Duplicate and/or repeated infant birth registration requests
  3. Handling failures
2. [Death registration](#43-death-registration--identity-status-update)
  1. New death registration initiated by CRVS
  2. Duplicate and/or repeated requests for death registration
  3. Handling failures
3. [Demographic data update](#44-demographic-data-updates)
  1. Infant demo data update request initiated by CRVS
  2. Duplicate and/or repeated infant demo data update requests
  3. Adult demo data update request initiated by CRVS

> **Note**: These are the currently supported scenarios. Additional use cases will be introduced as the integration evolves and expands based on country-specific requirements and feedback.

## 2.4 What's NOT in Scope (Out-of-Scope)

### Integration Limitations

Although the integration scope includes scenarios for birth, death, and updates, there are still some cases where limitations exist.

1. **No Support for New Adult Birth Registrations**: MOSIP does not support new adult birth registrations when the request comes from CRVS.

2. **No Support for Demographic Updates When Biometrics Are Already Linked to a National ID**: MOSIP will not support demographic updates from CRVS for individuals whose biometrics are already linked to a National ID. For such updates, individuals are expected to visit the National ID registration centre (MOSIP).

3. **Integration for Birth and Death Registrations**: The integration works seamlessly for birth and death registrations. However, updates to demographic data are still a work in progress.

4. **Duplicate Request Rejection**: Since the CRVS system is considered the source of truth, MOSIP currently does not reject duplicate birth/death registration requests received from the CRVS system. This can result in multiple UINs for the same infant or an update of the death flag for the same deceased. Deduplication is expected to be handled by CRVS.

5. **No Support for Rejected Packets, Status Updates, and Reason**: If a request fails due to validation issues in MOSIP, there is currently no mechanism to send detailed rejection reasons back to CRVS.

6. **No support for Offline Integration**: This integration works only when online connectivity is available as eSignet authentication is a necessary step before a request is submitted to CRVS.

7. **Use of VID/UIN for Death Registration and Demo Data Updates**: Only VID or UIN can be used to register a death or submit requests for the demo data updates. Currently, MOSIP does not support death updates with any other identifier.

## 2.5 System Roles & Responsibilities

### CRVS System Responsibilities

| **Area** | **Responsibility** |
|---------|-------------------|
| **Data Collection** | Capture vital event data from informants/applicants |
| **Data Validation** | Validate completeness and accuracy before sending to MOSIP |
| **Deduplication** | Prevent duplicate registrations for same vital event |
| **Authentication** | Integrate eSignet for informant/introducer authentication |
| **API Integration** | Call MOSIP APIs (Create Packet, Trigger) with proper authentication |
| **Notification Handling** | Subscribe to WebSub and process credential/status notifications |
| **Certificate Issuance** | Generate and issue birth/death certificates |
| **Error Management** | Handle API errors and retry failed requests |
| **Audit Trail** | Maintain logs of all MOSIP interactions |
| **Data Security** | Secure transmission and storage of identity data |

### MOSIP Platform Responsibilities

| **Area** | **Responsibility** |
|---------|-------------------|
| **Packet Validation** | Validate structure, schema, and mandatory fields |
| **Identity Processing** | Process registration packets through workflows |
| **UIN Generation** | Generate unique identifiers for new births |
| **Status Management** | Update deceased status for death registrations |
| **Credential Generation** | Create UIN/VID/PSUT tokens |
| **Notification Publishing** | Publish events to WebSub for credential/status updates |
| **Authentication** | Validate eSignet tokens for audit purposes |
| **Technical Validation** | Enforce schema compliance and data integrity |
| **Error Reporting** | Send validation errors via WebSub notifications |
| **Partner Management** | Manage CRVS as trusted partner with appropriate policies |

### Shared Responsibilities

| **Area** | **CRVS Contribution** | **MOSIP Contribution** |
|---------|----------------------|------------------------|
| **Field Mapping** | Define CRVS field names and formats | Define ID schema structure |
| **Error Handling** | Implement retry logic | Provide detailed error codes |
| **Security** | Secure OAuth2 client credentials | Issue and validate access tokens |
| **Monitoring** | Monitor API response times | Monitor packet processing times |
| **Reconciliation** | Track pending requests | Publish status updates |
| **Testing** | Provide test data and scenarios | Provide sandbox environment |

### Role-Based Access Control

**CRVS Operator:**
- Access CRVS portal
- Capture vital event data
- Initiate authentication flows
- Cannot directly access MOSIP APIs

**CRVS System (OAuth2 Client):**
- Call Packet Manager API
- Call Trigger API
- Receive WebSub notifications
- Cannot access admin functions

**MOSIP Administrator:**
- Configure partner policies
- Create centres/machines/officers
- Monitor integration health
- Cannot access CRVS data directly

**System Integrator:**
- Configure OAuth2 clients
- Set up WebSub subscriptions
- Map fields between systems
- Test end-to-end flows


---

# 3. Core Integration Principles

## 3.1 CRVS as Source of Truth

**CRVS is considered the source of truth**. MOSIP relies on CRVS to perform deduplication and treats CRVS as the authoritative source of vital event data. MOSIP does not perform de-duplication or data validation on requests from CRVS systems because of this trust relationship.

**Assumption:** This approach assumes that the CRVS system is recognized as the authoritative source of vital event data and is legally authorized to issue official birth and death certificates in accordance with the laws of the country.

## 3.2 Trust & Data Verification Model

### Data Verification and Trust

1. The initiation of the request with the data will be from CRVS to MOSIP
2. Data transmitted from CRVS to MOSIP will undergo thorough data/document verification and deduplication before any request is initiated for MOSIP to process.
3. MOSIP will trust all information provided by CRVS and honor the received requests. MOSIP will ensure that the technical and logical validations are all in place for packet processing.
4. The integration should define the mandatory attributes to be exchanged between the two systems. It is crucial to agree upon these attributes to ensure consistent and accurate data exchange.

## 3.3 Identity Lifecycle Management

### Identity Lifecycle Stages

The CRVS-MOSIP integration manages identity through distinct lifecycle stages:

```
Birth → Infant (0-5 yrs) → Minor (5-18 yrs) → Adult (18+ yrs) → Deceased
```

### Stage 1: Birth & Initial Registration

**Trigger**: Birth event reported to CRVS

**Process**:
1. CRVS captures newborn demographic data
2. Parent/introducer authenticated via eSignet
3. MOSIP creates identity record without biometrics
4. UIN generated and stored
5. PSUT token shared with CRVS
6. Birth certificate issued with UIN reference

**Identity State**:
- UIN: Active
- Biometric: Not collected
- Authentication: Via introducer
- Credential: PSUT token available

### Stage 2: Infant Updates (0-5 years)

**Trigger**: Demographic changes (name correction, address update)

**Process**:
1. CRVS submits update request
2. MOSIP validates and updates demographic data
3. UIN remains same
4. Updated credential shared if needed

**Identity State**:
- UIN: Active (unchanged)
- Biometric: Still not collected
- Updates: Allowed for all demographic fields

### Stage 3: Minor Enrollment (5-18 years)

**Trigger**: Individual reaches configured age threshold

**Process** (Outside CRVS integration):
1. Individual visits MOSIP enrollment center
2. Biometrics collected
3. Existing UIN linked to biometrics
4. Deduplication performed
5. Full identity activation

**Identity State**:
- UIN: Active (same as birth-issued)
- Biometric: Enrolled
- Authentication: Biometric-based
- Updates: Require in-person verification

### Stage 4: Adult Identity Management

**Trigger**: Demographic updates needed

**Process**:
- **Via CRVS** (Limited): Only specific fields, requires eSignet authentication
- **Via MOSIP** (Preferred): Individual visits center for biometric-verified updates

**Identity State**:
- UIN: Active
- Biometric: On file
- Updates: Restricted for CRVS, full access via MOSIP centers

### Stage 5: Death Registration

**Trigger**: Death event reported to CRVS

**Process**:
1. Informant reports death with UIN/VID
2. Informant authenticated via eSignet
3. MOSIP updates deceased flag
4. Death date recorded
5. UIN remains active (not deactivated)
6. Status notification sent to CRVS
7. Death certificate issued

**Identity State**:
- UIN: Active (technically)
- Deceased Flag: Set to "Y"
- Death Date: Recorded
- Authentication: Blocked
- Transactions: Prevented

### Lifecycle Transition Rules

| **Transition** | **Triggered By** | **CRVS Involvement** | **MOSIP Action** |
|---------------|------------------|---------------------|------------------|
| No Identity → Infant | Birth registration | Initiates request | Creates UIN |
| Infant → Minor | Age threshold | No involvement | Biometric enrollment |
| Minor → Adult | Age threshold | No involvement | Status update |
| Active → Deceased | Death registration | Initiates request | Sets deceased flag |
| Infant → Deceased | Death registration | Initiates request | Sets deceased flag |

### Data Persistence

**Throughout Lifecycle:**
- UIN: Permanent, never changes
- Demographic data: Updated as needed
- Biometric data: Added once, updated rarely
- Transaction history: Maintained for audit
- Deceased status: Permanent once set

**After Death:**
- Identity data: Retained for legal/historical purposes
- UIN: Not reused
- Credentials: Invalidated
- Records: Archived per retention policy

## 3.4 De-duplication Strategy

> **Note**: MOSIP relies on CRVS to perform deduplication and treats CRVS as the source of truth. While MOSIP has its internal deduplication mechanism to detect and reject duplicate packets, duplicate and repeated request scenarios are not currently handled for rejection by MOSIP.

## 3.5 Authentication & Authorization Model

### Proof of Authentication

1. For any request submitted to MOSIP, it is essential to include the biometric data of the informant or applicant, whenever feasible, to ensure the accurate verification of the individual initiating the request and confirm their legitimacy.
2. For cases where biometric data is not collected by CRVS, it is recommended for CRVS to use eSignet for the applicant or introducer authentication. Post-authentication user info token should be shared with MOSIP for validation and auditing purposes.

## 3.6 Notification & Event Architecture

### Identity Credential Sharing & Packet status

1. The identity credential sharing and packet status updates are two separate MOSIP integration points for any CRVS.
2. MOSIP will generate and share identity credentials for new registration cases via WebSub. Any partner configured according to the policy will receive these notifications automatically.
3. By default, MOSIP recommends sharing the PSUT token as part of the identity credential returned to CRVS. However, the system also supports sharing UIN, VID, or the PSUT token, based on the country's specific requirements.
4. MOSIP will publish packet status updates to a separate WebSub. Any credential partner subscribed to this WebSub topic can filter and track specific packets based on their status.

### Multiple CRVS Systems

1. If a country deploys multiple CRVS systems, each system will be treated as a separate partner for MOSIP. This allows for independent management and integration with each CRVS system.
2. Credentials generated/update messages from MOSIP will be sent to all the configured credential partners subscribed to the websub as per policy. It is the responsibility of the country and CRVS systems to manage the sharing of information across multiple CRVS systems.

---

# 4. Integration Patterns & Workflows

## 4.1 High-Level Integration Flow

### Overview

The MOSIP-CRVS integration follows a **request-response-notification pattern** where CRVS initiates requests, MOSIP processes them, and both systems exchange status updates.

### Integration Flow Diagram

```
┌─────────┐                    ┌──────────┐                  ┌─────────┐
│  CRVS   │                    │ eSignet  │                  │  MOSIP  │
│ System  │                    │          │                  │Platform │
└────┬────┘                    └─────┬────┘                  └────┬────┘
     │                               │                            │
     │ 1. Authenticate Informant     │                            │
     ├──────────────────────────────>│                            │
     │<─────────────────────────────┤                            │
     │   Auth Token                  │                            │
     │                               │                            │
     │ 2. Create Packet API                                       │
     ├───────────────────────────────────────────────────────────>│
     │   (with auth token, demographic data)                      │
     │<───────────────────────────────────────────────────────────┤
     │   Packet ID                                                │
     │                                                            │
     │ 3. Trigger API                                             │
     ├───────────────────────────────────────────────────────────>│
     │   (Packet ID)                                              │
     │<───────────────────────────────────────────────────────────┤
     │   Workflow Triggered                                       │
     │                                                            │
     │                               │         4. Processing      │
     │                               │         (Validation,       │
     │                               │          UIN Gen, etc.)    │
     │                                                            │
     │ 5. WebSub Notification (Credential Issued)                 │
     │<───────────────────────────────────────────────────────────┤
     │   UIN/VID/PSUT Token                                       │
     │                                                            │
     │ 6. Issue Certificate                                       │
     │   (with UIN reference)                                     │
     │                                                            │
```

### Step-by-Step Flow

**Phase 1: Pre-Request (CRVS)**
1. Informant visits CRVS center and provides vital event details
2. CRVS officer validates and enters data into CRVS system
3. CRVS performs internal deduplication check
4. System validates mandatory fields and data formats

**Phase 2: Authentication (eSignet)**
5. CRVS redirects to eSignet for informant/introducer authentication
6. User completes authentication (OTP, biometric, etc.)
7. eSignet returns authentication token to CRVS
8. CRVS includes token in MOSIP request

**Phase 3: Packet Creation (MOSIP)**
9. CRVS calls Create Packet API with:
   - OAuth2 access token (for CRVS client)
   - eSignet user info token (for informant)
   - Demographic data per ID schema
   - Centre/Machine/Officer IDs
10. MOSIP Packet Manager validates request structure
11. Packet created and stored in Object Store
12. Packet ID (AID) returned to CRVS

**Phase 4: Workflow Trigger (MOSIP)**
13. CRVS calls Trigger API with Packet ID
14. Registration Processor picks up packet
15. Packet routed through Camel workflow based on source/process
16. Async processing begins

**Phase 5: Processing (MOSIP)**
17. Schema validation against ID schema
18. Mandatory field validation
19. eSignet token validation
20. Deduplication check (internal MOSIP)
21. For birth: UIN generation
22. For death: Status update
23. Identity data stored in repository

**Phase 6: Credential Generation (MOSIP)**
24. Credential service generates UIN/VID/PSUT token
25. Policy determines what to share with CRVS
26. Credential data prepared for notification

**Phase 7: Notification (WebSub)**
27. MOSIP publishes event to WebSub hub
28. WebSub delivers notification to subscribed CRVS endpoint
29. CRVS receives and acknowledges notification
30. CRVS extracts credential data (UIN/PSUT)

**Phase 8: Certificate Issuance (CRVS)**
31. CRVS generates birth/death certificate
32. Certificate includes UIN reference
33. Certificate issued to informant/applicant
34. CRVS updates internal records

### Error Handling Flow

```
Error Occurs → MOSIP logs error → WebSub notification sent →
CRVS receives error → CRVS retries (if transient) or
CRVS alerts operator (if validation error)
```

### Key Integration Points

1. **Synchronous APIs**: Immediate request-response
   - OAuth2 token endpoint
   - Create Packet API
   - Trigger API
   - eSignet authentication

2. **Asynchronous Notifications**: Event-driven updates
   - Credential issuance via WebSub
   - Status updates via WebSub
   - Error notifications via WebSub

3. **Configuration Touchpoints**: Setup and management
   - Partner onboarding
   - Policy configuration
   - OAuth2 client setup
   - WebSub subscription

## 4.2 Birth Registration & UIN Issuance

### When Does It Happen?

Birth registration occurs when parents/informants notify vital events to the concerned civil registration authority (CRA). This process is initiated from the CRVS system after the CRA has completed identity proofing and provided informant authentication. Following this, a request is submitted to MOSIP for the registration of an infant's birth.

### What Does MOSIP Do?

When MOSIP receives a registration request, it validates the received packet, processes the request, and stores the identity information. A UIN is generated and added to the identity repository.

### What does CRVS receive?

Upon successful generation of the UIN, MOSIP sends credential data to the subscribed CRVS system via WebSub. By default, MOSIP recommends sharing the PSUT token (a print token to print eUIN cards) as part of the identity credential returned to CRVS. However, the system also supports sharing UIN, VID, or the PSUT token, based on the country's specific requirements. The recommended approach is to share the PSUT token, as it allows for printing the eUIN without exposing the actual UIN, thereby protecting an individual's privacy.

### What Is the Workflow?

For newborns who do not yet possess a national ID, the CRVS system can initiate a national ID registration request on their behalf. This request is initiated by a guardian, parent, or another authorized individual, referred to in MOSIP as the **"Introducer"** or an **"Informant"** in the CRVS system. The introducer must already be registered in MOSIP and have a valid national ID, which is used for authentication through **eSignet** to ensure the legitimacy of the registration.

To further enhance security and data accuracy, the CRVS system utilizes MOSIP's authentication mechanisms to verify the identities of parents or informants during the birth registration process. This helps prevent fraudulent registrations and strengthens overall data quality and system integrity.

> **Note**: MOSIP typically defines infants as individuals aged between 0 to 5 years. However, this classification is configurable based on the specific legal or administrative definitions of the implementing country. Anyone above the configured age threshold is categorized as either a minor or an adult.

<figure><img src="../../../.gitbook/assets/Infant_birth_registration.png" alt=""><figcaption><p>Infant Birth Registration</p></figcaption></figure>

#### Step 1: Operator Login & Authentication

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

#### Step 2: Packet Creation

CRVS creates a registration packet by calling the **Packet Manager API**, using:

1. Access Token
2. Client ID
3. ID Schema Version
4. Unique Registration ID (RID) / Application ID (AID)

The packet is stored in the **Object Store** for further processing.

#### Step 3: Workflow Trigger & Notification

1. CRVS calls the **Trigger API** to initiate the processing workflow.
2. Once the packet reaches a "Completed" state, MOSIP publishes an event to a **WebSub topic**, notifying subscribers.

#### Step 4: Identity Credential Sharing

Upon successful processing:

1. The newborn's demographic data is updated in MOSIP.
2. A notification is sent to the registered email/phone number.
3. The update event with identity credentials is published to the subscribed **WebSub topic**.
4. CRVS receives this update and can proceed with issuing the birth certificate.

### Duplicate and/or Repeated Infant Birth Registration Requests

Duplicate and/or repeated requests may arise under the following conditions:

1. **Repeated Requests - Same RID Used In Multiple Requests:**
   1. When multiple requests are made using the same RID (Request ID) for the birth registration of the same infant (with the same or different data).
   2. Currently, the request will be processed even if the same RID is used in multiple requests.
   3. MOSIP will overwrite the existing data with the **most recent values** provided in the latest request.
2. **Duplicate Requests - Same Infant Demographic Data with Different RIDs:**
   1. Multiple requests are made using identical infant demographic data but with different RIDs.
   2. Currently, the request will be processed, and an additional UIN will be issued for the infant for the additional RID.

> **Note**: MOSIP relies on CRVS to perform deduplication and treats CRVS as the source of truth. While MOSIP has its internal deduplication mechanism to detect and reject duplicate packets, the above scenarios are not currently handled for rejection of duplicate and/or repeated requests.

### Failure Handling in this Scenario

**Technical Failures:**
1. There is a possibility that some requests may fail due to failure caused by **internal MOSIP** technical problems during processing.
2. In case of internal processing issues within MOSIP, a **retry mechanism** automatically attempts reprocessing of failed requests.

**Validation Failures:**
1. This includes requests failing due to:
   - Missing mandatory fields
   - Invalid data formats
   - Schema validation errors
2. CRVS must subscribe to the WebSub topic to receive and act on such notifications.

| **Scenario** | **Existing Handling / Mechanism** | **Improvement Required?** |
|-------------|----------------------------------|--------------------------|
| **Duplicate Birth Registrations** | Currently, MOSIP does not reject duplicate birth registration requests from CRVS since CRVS is considered the source of truth. | De-duplication is expected at CRVS. Should MOSIP detect duplicates across registrations from different CRVS systems? |
| **Failed eSignet Authentication** | Authentication failures are managed by eSignet. The user token is not generated if authentication fails. | Enhancement: Log failure reasons and notify CRVS of authentication issues. |
| **Validation Failures** | MOSIP validates the packet structure and rejects invalid packets. | Enhancement: Provide detailed rejection reasons back to CRVS. |
| **Packet Processing Failures** | Internal MOSIP processing failures are logged and tracked. | Enhancement: Send failure notifications to CRVS with actionable error messages. |

## 4.3 Death Registration & Identity Status Update

### When Does It Happen?

Death registration is initiated by the next of kin or an informant through the CRVS system. The registrar collects necessary details and submits a death registration request to MOSIP.

### What Does MOSIP Do?

MOSIP receives the death registration request (including the UIN/VID of the deceased), validates the request, and updates the identity status to mark the individual as deceased. The UIN itself is NOT deactivated; instead, a "deceased" flag is set.

### What does CRVS Receive?

MOSIP sends a status update notification via WebSub confirming the successful update of the deceased flag. This allows CRVS to complete the death registration process and issue the death certificate.

### What Is the Workflow?

A death registration request is initiated by the CRVS system based on the informant reporting the deceased. For MOSIP, the informant must be a registered user with a valid national ID. This ID is authenticated using eSignet. During the death registration, MOSIP marks the individual as deceased using a status flag, without deactivating the ID. The date of death declaration is recorded in the system.

To facilitate this process, the following fields can be added to the ID schema:
1. declaredAsDeceased
2. deceasedDeclarationDate
3. typeOfDeath
4. deceasedInformer

It is up to each country to determine which fields should be included in the ID schema, based on their specific use case.

<figure><img src="../../../.gitbook/assets/death_registration.png" alt=""><figcaption><p>Death Registration</p></figcaption></figure>

#### Step 1: Registration Initiation & Authentication

1. The informant visits the CRVS Registration Centre.
2. Provides necessary details.
3. The CRVS system verifies the informant's identity via eSignet.
4. On successful authentication, CRVS adds the eSignet user info token to the ID schema and proceeds.

**Required Information for MOSIP to Process the Request:**

1. **Deceased Information** (Extendable as per country needs):
   1. Name
   2. Date of Death
   3. UIN/VID
2. **Informant Information** (Extendable as per country needs):
   1. Informant's UIN/VID
   2. eSignet User Info Token

#### Step 2: Packet Creation

1. CRVS sends a request to the Packet Manager API.
2. Includes access token, client ID, ID schema version, and RID/AID.
3. The packet is stored in the object store.

#### Step 3: Workflow Trigger

1. CRVS triggers the registration workflow by calling the "trigger" API using the access token.
2. This ensures the death registration is processed within MOSIP.

#### Step 4: Validation

1. MOSIP checks the UINs/VIDs' current status to determine if it was previously marked as deceased.
2. If not, MOSIP updates the death declaration flag to "Y - YES".

#### Step 5: Notifications

1. Once the packet is processed and approved, a notification is sent to the registered email and phone number regarding the update.
2. Update is also shared with CRVS through a WebSub event.

### Duplicate and/or Repeated Requests for Death Registration

A request is considered a duplicate under the following conditions:

1. **Repeated Requests - Same AID Used for Multiple Requests:**
   - When multiple death registration requests are made using the same AID for the same individual.
   - Currently, the request will be processed and the deceased flag will be updated each time.
   - MOSIP does not reject repeated requests since CRVS is considered the source of truth.

> **Note**: MOSIP relies on CRVS to perform deduplication and treats CRVS as the source of truth. While MOSIP has its internal deduplication mechanism to detect and reject duplicate packets, the above scenarios are not currently handled for rejection of duplicate and/or repeated requests.

### Failure Handling in this Scenario

**Technical Failures:**
1. Failures due to internal MOSIP issues.
2. MOSIP includes a retry mechanism to reprocess such requests.

**Validation Failures:**
1. Failures due to data validation errors or uniqueness conflicts.
2. Invalid UIN/VID provided.
3. Missing mandatory fields.
4. CRVS must subscribe to the WebSub topic to receive and act on such notifications.

| **Scenario** | **Existing Handling / Mechanism** | **Improvement Required?** |
|-------------|----------------------------------|--------------------------|
| **Invalid UIN/VID** | MOSIP rejects requests with invalid or non-existent identifiers. | Enhancement: Provide specific error codes for different validation failures. |
| **Duplicate Death Registration** | Currently not rejected if submitted multiple times from CRVS. | Should MOSIP prevent multiple death registrations for the same UIN? |
| **Status Update Failures** | Internal failures are logged. | Enhancement: Send failure notifications to CRVS with detailed error messages. |

## 4.4 Demographic Data Updates

### When Does It Happen?

Demographic updates can occur when there are changes to an individual's personal information (e.g., name change, address update). This is more common for infants (pre-biometric stage) but can also apply to adults in specific cases.

### What Does MOSIP Do?

MOSIP receives the update request from CRVS, validates the request, and updates the demographic information in the identity repository. For infants (without biometrics), the update is straightforward. For adults (with biometric records), updates are more restricted.

### What does CRVS Receive?

MOSIP sends a status update notification via WebSub confirming the successful demographic update.

### What Is the Workflow?

<div align="left"><figure><img src="../../../.gitbook/assets/03_demographic-update.png" alt=""><figcaption><p>Demographic Update Workflow</p></figcaption></figure></div>

1. Applicant submits demographic update request through CRVS
2. CRVS validates the information and submits update request to MOSIP
3. MOSIP validates and processes the update
4. MOSIP sends status update via WebSub to CRVS
5. CRVS confirms update to the applicant

### Handling Failures in this Scenario

| **Scenario** | **Existing Handling / Mechanism** | **Improvement Required?** |
|-------------|----------------------------------|--------------------------|
| **Update Rejected (Biometric Linked)** | MOSIP currently does not support demographic updates for individuals with biometric-linked UINs from CRVS. | Document this limitation clearly. Should there be exceptions? |
| **Invalid Update Data** | MOSIP validates update requests and rejects invalid data. | Enhancement: Provide detailed validation error messages to CRVS. |

## 4.5 Sequence Diagrams & Decision Trees

<!-- Missing Content - Add it --> (Add detailed sequence diagrams for each workflow)

---

# 5. Technical Architecture

## 5.1 System Architecture Overview

<!-- Missing Content - Add it -->

## 5.2 MOSIP Components Involved

<!-- Missing Content - Add it -->

## 5.3 CRVS Integration Points

<!-- Missing Content - Add it -->

## 5.4 Communication Protocols & Standards

<!-- Missing Content - Add it -->

## 5.5 Deployment Architecture

<!-- Missing Content - Add it -->

---

# 6. API Reference & Data Models

## 6.1 API Endpoints & Request Structure

### Overview

Once all the prerequisites are in place, the next step is to initiate a request (birth/death/update) by calling the create packet API of MOSIP's packet manager module, followed by the trigger API to process the packet.

### Application ID (AID) Structure

The Application ID (AID) refers to the unique identifier assigned to track the packet that is being processed for events such as birth or death registration. It can be used by MOSIP or CRVS to track the progress and status of the specific event.

#### AID Structure (Recommended):

1. **Centre ID** (First 5 digits): The first 5 digits of the AID represent the **Centre ID**.
2. **Machine ID** (Next 5 digits): The next 5 digits of the AID represent the **Machine ID**.
3. **Random Sequence** (next N digits): The next N digits can be a randomly generated sequence based on the length that the country wants to use for the AID.

**Example of AID:**

For the AID `10001100771006920220128223618`

The breakdown is as follows:
1. **Centre ID**: `10001` (First 5 digits)
2. **Machine ID**: `10077` (Next 5 digits)
3. **Random Sequence**: `1006920220128223618` (Remaining 16 digits)

The AID format mentioned above is the recommendation to be followed, but not mandatory. CRVS can generate the AID in any specified format as per their requirement and include it in the Create Packet API Request to ensure proper packet identification and mapping.

### Create Packet API (Packet Manager Module)

**Create Packet Endpoint:** `{domain}/commons/v1/packetmanager/createPacket`

**Method:** PUT

**API Request Structure:**

```json
{
  "id": "string",
  "version": "string",
  "requesttime": "2025-02-25T11:14:17.667Z",
  "request": {
    "source": "CRVS",
    "process": "CRVS_NEW",
    "id": "10001100771006920220128223618",
    "ref_id": "10001_10077",
    "schema_version": "0.1",
    "fields": {
      "fullName": "John Doe",
      "dateOfBirth": "2025-01-01",
      "gender": "Male",
      "addressLine1": "123 Main Street",
      "addressLine2": "Apt 4B",
      "city": "Capital City",
      "state": "State Name",
      "postalCode": "12345",
      "email": "john.doe@example.com",
      "phone": "+1234567890",
      "zone": "Zone1",
      "region": "Region1",
      "province": "Province1"
    },
    "metaInfo": {
      "centerId": "10001",
      "machineId": "10077",
      "operationsData": {
        "officerId": "officer123",
        "officerPassword": "password",
        "supervisorId": "supervisor456"
      },
      "registration_type": "CRVS_NEW"
    },
    "audit": {
      "uuid": "unique-uuid-1234",
      "createdBy": "CRVS_OPERATOR",
      "createdDateTime": "2025-02-25T11:14:17.667Z",
      "updatedBy": "CRVS_OPERATOR",
      "updatedDateTime": "2025-02-25T11:14:17.667Z"
    },
    "schemaJson": "{\"$schema\":\"http://json-schema.org/draft-07/schema#\"}"
  }
}
```

> **Note**: The API request shared above is only a sample and is not to be used for any implementation. Customize based on country-specific ID schema and requirements.

#### Field Descriptions

**Request Object:**

1. `source`: Specifies the source of the registration request. This will be the same for any request that comes to MOSIP for birth or death.
2. `process`: Identifies the specific process for the registration.
   - `CRVS_NEW` - When initiating an infant birth request
   - `CRVS_DEATH` - When initiating a death registration request
   - `CRVS_UPDATE` - When initiating a demographic update request
3. `id`: The unique identifier for the registration request (AID).

> **Note**: As per the current implementation, if the same AID is used twice, the record will be updated with the latest request data.

4. `ref_id`: Combination of centre ID and machine ID.
   - Ex - "centerid_machineid"
5. `schema_version`: The version of the ID schema that the country is using in production.

**Field Object:**

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
14. Additional fields can be added based on country requirements and ID schema.

**MetaInfo Object (Center and Operator Information):**

1. `centerId`: Unique identifier for the centre where the registration is processed.
2. `machineId`: Unique identifier for the machine used for registration.
3. `operationsData`: Contains fields such as officer ID, officer password, supervisor ID, etc.
4. `registration_type`: This is the value same as the process field for birth (`CRVS_NEW`) or death (`CRVS_DEATH`).

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
  "version": "1.0",
  "responsetime": "2025-02-25T11:14:17.667Z",
  "metadata": null,
  "response": {
    "status": "SUCCESS",
    "packetId": "10001100771006920220128223618",
    "message": "Packet created successfully"
  },
  "errors": []
}
```

### Trigger API (Registration Processor Module)

In MOSIP, after a packet is created, it is processed for validations and verification of the information in the **Registration Processor**. Inside the Registration Processor, each packet follows a specific workflow defined by the **Camel route**.

For the integration with CRVS, the newly created packet is uploaded to the Object Store. To pick up this new packet and trigger the processing, we have developed a new API. This API ensures to trigger the appropriate workflow is triggered for further processing of the registration packet. For this integration, the camel route workflow to be executed is determined by the values provided for the **source** and **process**.

**API Documentation:** [Create workflow instance for packet processing | Registration processor](https://mosip.stoplight.io/docs/registration-processor/branches/main/d56c892cfa950-create-workflow-instance-for-packet-processing)

**Trigger Endpoint:** `{domain}/registrationprocessor/v1/registration-processor/workflow/trigger`

**Method:** POST

**API Request Structure:**

```json
{
  "id": "mosip.registration.processor.workflow.trigger",
  "version": "1.0",
  "requesttime": "2025-02-25T11:14:17.667Z",
  "request": {
    "registrationId": "10001100771006920220128223618"
  }
}
```

**Sample Response:**

```json
{
  "id": "mosip.registration.processor.workflow.trigger",
  "version": "1.0",
  "responsetime": "2025-02-25T11:14:17.667Z",
  "response": {
    "status": "SUCCESS",
    "message": "Workflow triggered successfully"
  },
  "errors": []
}
```

## 6.2 Request/Response Schemas

<!-- Missing Content - Add it -->

## 6.3 Data Exchange Format

<!-- Missing Content - Add it -->

## 6.4 Field Mapping (CRVS ↔ MOSIP)

<!-- Missing Content - Add it -->

## 6.5 Mandatory vs Optional Fields

<!-- Missing Content - Add it -->

## 6.6 Data Validation Rules

<!-- Missing Content - Add it -->

---

# 7. Security & Authentication

## 7.1 OAuth2 Client Setup & Authentication

### Overview

As part of the integration approach, two specific APIs are exposed:

1. Create a packet API from the MOSIP packet manager module to create a packet
2. Trigger the API from the registration processor module to process the packet

allowing external systems (in this case, CRVS) to use these APIs to initiate requests.

To facilitate this, the external system must be assigned a specific new client ID and secret, ensuring secure and authenticated communication. Additionally, a new, specific role should be created for the external user, which will be associated with the API request in subsequent calls for packet creation and processing.

This role helps MOSIP validate and verify that the request is coming from an authorized and authentic source, ensuring secure and accurate handling of the registration process.

### Step 1: Create Client ID/Role for the CRVS

#### Create the Client

1. **Log in to Keycloak Admin Console**
   - Access the keycloak admin console.
   - Ensure you have the necessary administrative privileges to create clients.

2. **Select Your Realm**
   - If you are not already in the desired realm, switch to it from the top-left drop-down menu. The realm should be the one where you want to create the client.

3. **Create a New Client**
   - In the left-hand menu, go to **Clients** and click on **Create**.

4. **Enter the Client Details**
   - **Client ID**: Enter `mosip-crvs1-client` as the client ID (or a relevant name based on your deployment).
   - **Client Protocol**: Select `openid-connect`.
   - **Root URL**: Leave this field blank or enter the URL if required.

5. **Save the Client**
   - After entering the necessary details, click **Save** to create the client.

Once the client is created, please update the properties in the locations below:

1. `auth.server.admin.allowed.audience` In the [Packet manager default properties](https://github.com/mosip/mosip-config/blob/v1.2.4.0/packet-manager-default.properties#L24).
2. `auth.server.admin.allowed.audience` In the [Registration processor default properties](https://github.com/mosip/mosip-config/blob/v1.2.4.0/registration-processor-default.properties#L996).

> **Note:** The client name specified here is a placeholder and can be customised to suit the specific requirements of the System Integrator SI/CRVS.

#### Configuring the Client

1. **Access the Settings Tab**
   - After creating the client, navigate to the **Settings** tab.

2. **Configure Client Settings**
   - **Access Type**: Set this to **confidential** if you intend to use client credentials for authentication.
   - **Service Accounts Enabled**: Turn this option **ON** if you are using client credentials flow for secure communication.
   - **Valid Redirect URIs**: Enter `*` (or specify specific URLs if known and necessary).

3. **Save the Changes**
   - Once the configuration is complete, click **Save** to apply the changes.

#### Generate and Note the Secret Key

- Navigate to the **Credentials** tab.
- If you selected the **confidential** access type, keycloak will generate a **Secret Key**. Note this secret as it will be used for authentication in subsequent API calls.

#### Creating the Role

1. **Go to the Roles Section**
   - In the Keycloak Admin Console, under your realm, navigate to **Roles**.

2. **Create a New Role**
   - Click on **Add Role**.
   - Enter the following details:
     - **Role Name**: `ONLINE_REGISTRATION_CLIENT`
   - Click **Save** to create the role.

#### Assigning the Role to the Client

1. **Assign the Role to the Client**
   - Go back to the **Clients** section and select the client `mosip-crvs1-client` that you previously created.

2. **Navigate to Service Account Roles**
   - Under the **Service Account Roles** tab (this tab is visible only if **Service Accounts Enabled** is turned on), click on **Add Role**.

3. **Select the Role**
   - From the **Client Roles** dropdown, select either `realm-management` or your specific desired client role (if the role is specific to a client).
   - Add the `ONLINE_REGISTRATION_CLIENT` role to the selected client.

### Step 2: Fetch Access Token to Call the APIs

Once the role is created and mapped to the client ID. As a follow-up step, below keycloak API is to be called to authenticate the CRVS associated with the new role. In the response of the API, there is an access token returned in the response header. This is the access token that should be used when initiating any request using the packet manager API.

**Authenticate Endpoint:** `{domainname}/v1/authmanager/authenticate/clientidsecretkey`

**Method:** POST

**API Request Structure:**

```json
{
  "id": "string",
  "metadata": {},
  "request": {
    "appId": "{{appId}}",
    "clientId": "{{clientId}}",
    "secretKey": "{{secretKey}}"
  },
  "requesttime": "{{requestTime}}",
  "version": "string"
}
```

In the API above, the fields Client ID and Secret key are the values created in the previous steps, as mentioned above. Once the authentication is successful, in the response header, we will receive an access token, which is to be noted and used for the subsequent packet manager API request.

### eSignet Authentication Flow

#### Overview

eSignet is MOSIP's authentication service that enables secure identity verification. For CRVS integration, eSignet is used to authenticate informants/parents before submitting registration requests to MOSIP.

#### When is eSignet Used?

- During birth registration (to authenticate parent/informant)
- For demographic update requests
- When CRVS does not collect biometric data of the applicant

#### Authentication Flow

1. CRVS redirects user to eSignet for authentication
2. User completes authentication (e.g., OTP, biometric)
3. eSignet generates authentication token
4. CRVS receives token and includes it in MOSIP request
5. MOSIP validates token for audit and authorization

## 7.2 Partner Certificate Management

<!-- Missing Content - Add it -->

## 7.3 API Security (TLS, Encryption)

<!-- Missing Content - Add it -->

## 7.4 Access Control & Authorization

<!-- Missing Content - Add it -->

## 7.5 Audit & Compliance Requirements

<!-- Missing Content - Add it -->

---

# 8. Notifications & Event Handling

## 8.1 WebSub Overview

### What is WebSub?

WebSub is a publish-subscribe protocol used by MOSIP to send event notifications to integrated partners like CRVS systems. It allows real-time updates on credential generation and packet status changes.

### How Does It Work?

1. CRVS subscribes to specific WebSub topics during partner onboarding
2. MOSIP publishes events (e.g., UIN generated, packet processed) to these topics
3. WebSub hub delivers notifications to subscribed CRVS endpoints
4. CRVS processes the notification and takes appropriate action

## 8.2 Credential Issuance Notifications

### Credential Data Structure

When a UIN is generated, MOSIP publishes credential data via WebSub. The default recommendation is to share:

- PSUT Token (Print Secure UIN Token) - for printing eUIN cards
- Alternatively: UIN, VID, or PSUT token based on country policy

### Subscription Configuration

CRVS systems must:
1. Register as credential partners in MOSIP
2. Configure policy to receive credential notifications
3. Provide callback endpoint for WebSub delivery
4. Implement retry logic for failed deliveries

## 8.3 Packet Status Updates

### Status Types

MOSIP publishes packet status updates for:
- Packet received
- Packet under processing
- Packet processed successfully
- Packet rejected (with reason)

### Filtering

CRVS can filter status updates based on:
- Packet ID
- Status type
- Source CRVS system

## 8.4 Error Notifications

<!-- Missing Content - Add it --> (Detail error notification structure and handling)

---

# 9. Error Handling & Reconciliation

## 9.1 Error Handling

Error Scenarios and Handling:

1. **Invalid Input Data Formats** – MOSIP rejects the packet if mandatory information is missing.
2. **Duplicate Registration Requests** – Currently not rejected since CRVS is the source of truth. MOSIP generates multiple UINs for duplicate requests.
3. **Missing Mandatory Fields** – Packet validation fails and is rejected.
4. **eSignet Authentication Failures** – Authentication token not generated; request cannot proceed.
5. **Network/Connectivity Issues** – Retry mechanisms should be implemented by CRVS.

## 9.2 Retry Mechanisms

<!-- Missing Content - Add it -->

## 9.3 Reconciliation Strategies

<!-- Missing Content - Add it -->

## 9.4 Failed Packet Management

<!-- Missing Content - Add it -->

## 9.5 Logging & Monitoring

<!-- Missing Content - Add it -->

---

# 10. Policy Configuration & Customization

## 10.1 Partner Policy Configuration

<!-- Missing Content - Add it -->

## 10.2 Credential Sharing Policy

### Policy Options

Countries can customize credential sharing based on their requirements:

1. **Default (Recommended)**: Share PSUT token
  - Protects UIN privacy
  - Allows credential printing
  - Minimizes PII exposure

2. **Option 2**: Share UIN directly
  - Simplest approach
  - Higher privacy risk
  - Use only if CRVS has strong data protection

3. **Option 3**: Share VID
  - Temporary identifier
  - Better privacy than UIN
  - Requires VID management

4. **Option 4**: Share PSUT + VID
  - Balanced approach
  - Flexibility for different use cases

## 10.3 WebSub Topic Configuration

<!-- Missing Content - Add it -->

## 10.4 Country-Specific Customizations

<!-- Missing Content - Add it -->

---

# 11. Operational Considerations

## 11.1 Prerequisites & System Setup

This section outlines the technical requirements for integrating a Civil Registration and Vital Statistics (CRVS) system with MOSIP, applicable to any country implementation. It covers the necessary prerequisites and configurations to facilitate a seamless and secure connection between the two systems. Depending on the country's implementation model and agreements, these steps may be carried out by the System Integrator (SI) or the CRVS team.

### Pre-integration Checklist

Before beginning integration, ensure the following are in place:

1. **MOSIP Platform Readiness**
   - MOSIP deployed and operational
   - Partner management module configured
   - eSignet service running
   - WebSub hub configured
   - Keycloak admin access available

2. **CRVS System Readiness**
   - CRVS system can generate API requests
   - CRVS has webhook endpoint for receiving notifications
   - CRVS can process WebSub notifications
   - CRVS implements eSignet authentication flow

3. **Network and Security**
   - Secure network connectivity between CRVS and MOSIP
   - TLS certificates configured
   - Firewall rules configured

4. **Data Readiness**
   - Field mapping agreed between CRVS and MOSIP
   - Mandatory attributes defined
   - Data validation rules documented

### Configuration Steps

#### Step 1: ID Schema Configuration

To initiate any registration request, the country must define an ID schema based on the specific requirements for CRVS integration. The sample ID schema can be referred to [here](../../../../_files/id-schema/id-schema-sample.json) and should be customized to include all required fields for packet generation per the country's requirements. This schema governs the structure of the data submitted to MOSIP for processing and storage in the Identity Repository.

> **Note:** MOSIP advises adopting and customizing the latest released ID schema version to meet country-specific needs.

For comprehensive guidance on defining and customizing the ID schema in MOSIP, please refer to the [documentation here](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-management/id-schema).

#### Step 2: Create Default Officer

To process the request coming from CRVS, a default officer is to be registered and assigned to the CRVS. The officer is to be added from keycloak by the admin.

1. **Log in to Keycloak**
   - Open the Keycloak admin portal.
   - Log in using your admin credentials.

2. **Navigate to the Users Section**
   - In the left-hand menu, click on Users.

3. **Add a New User**
   - Click on the Add user button to create a new user.
   - Fill in the required fields under the details tab:
     - **Username**: Enter a unique username for the officer (this will become the Officer ID).
     - **First Name**: Enter the officer's first name.
     - **Last Name**: Enter the officer's last name.

4. **Set Temporary Password**
   - Under the **Credentials** tab, set the password for the officer.
   - Toggle **Temporary** to **Off** to disable the "temporary password" feature.
   - Click **Set Password**.

5. **Enable the User**
   - Under the **User Enabled** section, ensure that the user is **enabled**.
   - Click **Save** to create the user.

6. **Assign Roles to the User**
   - Navigate to the **Role Mappings** tab.
   - Under **Available Roles**, select the role **Registration_officer**.
   - Click the **Add selected** button to assign the role.

7. **Finalize the Officer Creation**
   - After saving, the officer's **username** from step 3 will serve as the **Officer ID**.
   - This **Officer ID** will be used in the subsequent create packet API request. Ensure you pass this ID correctly in the request.

#### Step 3: Create Centre

A unique default centre will be assigned to the CRVS to process requests. This centre can be created through the Admin Portal. For detailed instructions on how to create a centre, refer to the [**Admin Portal Center Creation Guide**](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/administration/test/admin-portal-user-guide#create-center).

**Fetching the Centre ID**

As of now, there is no direct support for fetching the specific centre ID in MOSIP. To retrieve the Centre ID, use the API below to get a list of all centres in the system. From this list, manually search for the Centre ID associated with the newly created centre for CRVS. The "id" attribute in the response will be the centre ID.

**Get List of All Centres Endpoint:** `{domain}/v1/admin/masterdata/registrationcenters`

**Method:** GET

Once the **Centre ID** is identified, ensure it is saved securely for future reference. This **Centre ID** will be required in subsequent interactions with the CRVS system for processing requests.

#### Step 4: Create Machine

A unique default machine will be assigned to the CRVS to process requests. This machine can be created through the Admin Portal.

Before creating a new machine, it is required that a public key is fetched using the below API. The public key received in the response of this API is to be used as the public key & signing public key, while adding the details in the admin portal, to create and onboard the machine.

**URL**: `{domain}/v1/keymanager/tpmsigning/publickey`

**Method**: POST

**API Request Structure:**

```json
{
  "request": {
    "serverProfile": "Prod"
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
  "response": {
    "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzWOM81oggDiy26SPzASVCLpEf0sJC-81j7GCpDHVcHCAfQVIxaBP9K6u2R19mgiakVY92Nlb5y4PUKV1EpLbZKQxaK14gU4ks7hroCM_gbEssaon7lCFCu8uobKriXlC9RI1ZY9HF9QFfilCuGC9q58gZ_YC-VMGZOB9YtN_5QRbXvI9XQr-d1eODtOVuVCpsPz6FkEHOSWdj0HPLeGTLZO7Ac7dfMyksNJzfmad6PQ7i2GHQ1ZqK6aVTZOt37_kuGkUz7CzVhBNhsdYVeK-qG331dI66XMxSVYNK5O9poDzH1mAAIG_2MdxAEWHcDstZl6YvmWn5-JEhS6QdB6mFwIDAQAB"
  },
  "errors": null
}
```

For detailed instructions on how to create a machine, refer to the [**Admin Portal Machine Creation Guide**](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/administration/test/admin-portal-user-guide#machines)

> **Note: "/home/mosip/.mosipkeys"** location must be volume mounted and kept secure as per the best security practices to prevent unauthorised access.

**Fetching the Machine ID**

As of now, there is no direct support for fetching the specific machine ID in MOSIP. To retrieve the machine ID, use the API below to get a list of all machines in the system. From this list, manually search for the machine ID associated with the newly created machine for CRVS. The "id" attribute in the response will be the machine ID.

**Get a List of All Machine Endpoints:** `{domain}/v1/masterdata/machines`

**Method:** GET

Once the **Machine ID** is identified, ensure it is saved securely for future reference. This **Machine ID** will be required in subsequent interactions with the CRVS system for processing requests.

#### Step 5: Map Officer to Centre and Machine

Once the officer, centre, and machine are created for CRVS, the next step is to map the user to the centre. This ensures the user is properly associated with the correct zone and centre for their operations.

1. **Log in to Admin UI**
   - Log in to the Admin UI using the **admin** user created in Keycloak if it is a fresh environment.

2. **Map Zone to User**
   - Navigate to **Resources** → **User Zone Mapping** → **Map Zone**.
   - Select the **username** for which the zone is to be mapped.
   - Choose the appropriate **zone** from the dropdown.
   - Click **Save**.

3. **Activate the User**
   - After mapping the zone, activate the user.

4. **Map Centre to User**
   - Navigate to **Resources** → **User Centre Mapping**.
   - Select the **username** for which the centre needs to be mapped.
   - Choose the appropriate **centre** from the dropdown.
   - Click **Save**.
   - Activate the user, as outlined in Step 3.

## 11.2 Partner Onboarding Process

<!-- Missing Content - Add it -->

## 11.3 Performance & Scalability

<!-- Missing Content - Add it -->

## 11.4 Monitoring & Alerting

<!-- Missing Content - Add it -->

## 11.5 Maintenance & Support

<!-- Missing Content - Add it -->

---

# 12. Testing & Validation

## 12.1 Test Environment Setup

<!-- Missing Content - Add it -->

## 12.2 Test Scenarios & Test Cases

### Birth Registration Test Cases

1. **Positive Test Cases**
  - Valid birth registration with all mandatory fields
  - Birth registration with optional fields
  - Birth registration with different authentication methods

2. **Negative Test Cases**
  - Birth registration with missing mandatory fields
  - Birth registration with invalid data formats
  - Duplicate birth registration requests

### Death Registration Test Cases

1. **Positive Test Cases**
  - Valid death registration with UIN
  - Valid death registration with VID

2. **Negative Test Cases**
  - Death registration with invalid UIN
  - Death registration without identifier
  - Duplicate death registration

### Demographic Update Test Cases

1. **Positive Test Cases**
  - Infant demographic update (no biometrics)
  - Address update for infant

2. **Negative Test Cases**
  - Demographic update for biometric-linked adult
  - Update with invalid data

## 12.3 Integration Testing Guide

<!-- Missing Content - Add it -->

## 12.4 UAT Checklist

<!-- Missing Content - Add it -->

## 12.5 Performance Testing

<!-- Missing Content - Add it -->

## 12.6 Security Testing

<!-- Missing Content - Add it -->

---

# 13. Code Examples & Reference Implementations

## 13.1 Sample Birth Registration Request

<!-- Missing Content - Add it --> (Provide actual JSON/XML examples)

## 13.2 Sample Death Registration Request

<!-- Missing Content - Add it -->

## 13.3 Sample WebSub Subscription

<!-- Missing Content - Add it -->

## 13.4 Sample Credential Notification Handling

<!-- Missing Content - Add it -->

## 13.5 Sample Error Handling Code

<!-- Missing Content - Add it -->

---

# 14. Troubleshooting Guide

## 14.1 Common Integration Issues

### Issue 1: Packet Rejected - Missing Mandatory Fields

**Symptoms**: MOSIP rejects packet with validation error

**Cause**: Request missing required fields

**Solution**:
1. Check API request against schema
2. Verify all mandatory fields are included
3. Check field data types match schema

### Issue 2: eSignet Authentication Fails

**Symptoms**: User cannot complete authentication

**Cause**: Invalid configuration or network issues

**Solution**:
1. Verify eSignet service is running
2. Check network connectivity
3. Validate authentication configuration

### Issue 3: WebSub Notifications Not Received

**Symptoms**: CRVS not receiving credential notifications

**Cause**: Subscription not configured or webhook endpoint unreachable

**Solution**:
1. Verify WebSub subscription is active
2. Check webhook endpoint is accessible
3. Review WebSub hub logs

## 14.2 Error Code Reference

<!-- Missing Content - Add it --> (Comprehensive error code table)

## 14.3 Log Analysis Guide

<!-- Missing Content - Add it -->

## 14.4 Performance Issues

<!-- Missing Content - Add it -->

## 14.5 Support Escalation Process

<!-- Missing Content - Add it -->

---

# 15. Appendices & References

## 15.1 API Documentation Links

<!-- Missing Content - Add it -->

## 15.2 Related MOSIP Documentation

- [MOSIP ID Lifecycle Management](https://docs.mosip.io/1.2.0/id-lifecycle-management)
- [Partner Management](https://docs.mosip.io/1.2.0/modules/partner-management-services)
- [eSignet Documentation](https://docs.esignet.io/)
- [WebSub Specification](https://www.w3.org/TR/websub/)

## 15.3 Standards & Compliance

<!-- Missing Content - Add it -->

## 15.4 Change Log

| **Version** | **Date** | **Changes** | **Author** |
|------------|----------|-------------|-----------|
| 1.0 | 2026-01-22 | Initial reorganized version per IA v2.0 | Documentation Team |

## 15.5 FAQ

<!-- Missing Content - Add it -->

## 15.6 Glossary

| **Term** | **Definition** |
|---------|---------------|
| CRVS | Civil Registration and Vital Statistics |
| UIN | Unique Identification Number |
| VID | Virtual ID |
| PSUT | Print Secure UIN Token |
| eSignet | MOSIP's authentication service |
| WebSub | Publish-subscribe protocol for event notifications |
| CRA | Civil Registration Authority |

---

**End of MOSIP-CRVS Integration Guide**

**Note to Technical Writers**: This document has been reorganized according to Information Architecture v2.0. Sections marked with "<!-- Missing Content - Add it -->" require additional content to be authored. Existing content from the original documentation has been mapped to the appropriate sections. Please review and complete the missing sections based on SME inputs and technical specifications.




# Attempt 2
The mosip-crvs-integration-guide.md has been significantly enhanced from 731 lines to 1,294 lines (77% increase) with all missing content from both source files.

📋 What Was Added:
From _ks_mosip-crvs-2.md:
✅ Correct diagram references:

Infant_birth_registration.png (instead of 01_birth-registration.png)
death_registration.png (instead of 02_death-registration.png)
✅ Detailed step-by-step workflows:

Birth Registration: 4 detailed steps (Operator Login & Authentication, Packet Creation, Workflow Trigger, Identity Credential Sharing)
Death Registration: 5 detailed steps (Registration Initiation, Packet Creation, Workflow Trigger, Validation, Notifications)
Data requirements for each step clearly specified
✅ Age demarcation note: Infants defined as 0-5 years (configurable)

✅ Duplicate/Repeated request handling:

Detailed scenarios for same RID vs different RID
Technical vs Validation failure handling
✅ Death registration schema fields:

declaredAsDeceased, deceasedDeclarationDate, typeOfDeath, deceasedInformer
From technical-details.md:
✅ Section 6.1 - Complete API Documentation:

AID Structure with detailed breakdown and example
Create Packet API - Full request/response with field descriptions
Trigger API - Complete documentation with examples
✅ Section 7.1 - OAuth2/Keycloak Setup (8 detailed steps):

Create Client in Keycloak (with configuration properties)
Configure Client (Access Type, Service Accounts)
Generate Secret Key
Create Role (ONLINE_REGISTRATION_CLIENT)
Assign Role to Client
Fetch Access Token API with request structure
eSignet Authentication Flow integrated
✅ Section 11.1 - Prerequisites & System Setup:

ID Schema Configuration with documentation links
Create Default Officer (7 sub-steps in Keycloak)
Create Centre with API to fetch Centre ID
Create Machine with public key API and response example
Map Officer to Centre and Machine (4 sub-steps in Admin UI)
📊 Content Distribution:
Section 4 (Workflows): Enhanced with 100+ lines of detailed steps
Section 6 (API Reference): Added 150+ lines of complete API documentation
Section 7 (Security): Added 100+ lines of OAuth/Keycloak setup
Section 11 (Prerequisites): Added 200+ lines of system setup procedures
🎯 All "Missing Content" markers remain for sections that still need:
Architecture diagrams (Section 5)
Additional API schemas (Section 6.2-6.6)
Partner certificate management (Section 7.2)
Code examples (Section 13)
Detailed troubleshooting (Section 14)