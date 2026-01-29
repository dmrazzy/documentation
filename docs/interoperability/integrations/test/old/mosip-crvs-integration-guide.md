# MOSIP-CRVS Integration Guide

**Document Version**: 1.0  
**Last Updated**: 27 January 2026  
**Status**: Reorganized per Information Architecture v2.0

---

## Table of Contents

1. [Introduction & Context](#1-introduction--context)
   - 1.5 [Integration Boundaries: Principles & Real-World Implications](#15-integration-boundaries-principles--real-world-implications)
2. [Integration Overview](#2-integration-overview)
3. [Core Integration Principles](#3-core-integration-principles)
4. [Integration Patterns & Workflows](#4-integration-patterns--workflows)
   - 4.1 [High-Level Integration Flow](#41-high-level-integration-flow)
   - 4.2 [Birth Registration & UIN Issuance](#42-birth-registration--uin-issuance)
   - 4.3 [Death Registration & Identity Status Update](#43-death-registration--identity-status-update)
   - 4.4 [Demographic Data Updates](#44-demographic-data-updates)
   - 4.5 [Sequence Diagrams & Decision Trees](#45-sequence-diagrams--decision-trees)
   - 4.6 [Handling Rare Scenarios](#46-handling-rare-scenarios-in-mosipcrvs-integration)
     - 4.6.1 [Fraudulent Birth Registrations - National ID Deactivation](#461-fraudulent-birth-registrations---national-id-deactivation-request-from-crvs)
     - 4.6.2 [Reactivation of Deactivated National ID](#462-reactivation-of-deactivated-national-id)
     - 4.6.3 [Fraud Death Case - Reversal of the Death Flag](#463-fraud-death-case---reversal-of-the-death-flag)
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

<!-- Missing Content - Add it -->


## 1.2 Integration Value Proposition

MOSIP can integrate with CRVS (Civil Registration and Vital Statistics) systems to create a unified digital identity ecosystem that spans an individual's entire life cycle.

**Civil Registration and Vital Statistics (CRVS)** systems are essential for documenting key life events such as births, deaths, marriages, and divorces. These systems provide individuals with legal recognition and play a vital role in ensuring access to rights and services. Additionally, CRVS systems generate important demographic data that supports effective policy-making, governance, and resource planning.

**MOSIP** is an open-source, modular identity platform designed for scalability, interoperability, and compliance with global standards. It allows countries to customize identity systems tailored to their governance needs. To learn more about MOSIP, click [here](https://docs.mosip.io/1.2.0/id-lifecycle-management).

This synergy allows for the seamless management of an individual's identity across life events, starting from birth registration, through education and employment, to end-of-life verification.

> Note: When these systems operate in isolation, it leads to administrative inefficiencies, fragmented data, and challenges in service access.

## 1.3 Document Navigation Guide

<!-- Missing Content - Add it -->

## 1.4 Terminology & Glossary

<!-- Missing Content - Add it -->

## 1.5 Integration Boundaries: Principles & Real-World Implications

This section explains the philosophical foundation behind the technical boundaries, limitations, and design decisions documented throughout this guide. While MOSIP's technical capabilities could support various integration patterns, not all technically feasible approaches are recommended or appropriate.

### 1.5.1 Foundational ID vs. Civil Registration: Two Different Systems

MOSIP (Foundational ID) and CRVS (Civil Registration and Vital Statistics) are complementary but fundamentally distinct systems with different mandates, authorities, and operational models.

**CRVS Systems**:
- **Primary Mandate**: Legal recognition and registration of vital events (births, deaths, marriages, divorces)
- **Authority**: Empowered by civil registration laws to issue legally binding certificates
- **Data Ownership**: Authoritative source for vital event data
- **Operational Model**: Decentralized or centralized registration offices, often aligned with administrative boundaries
- **Focus**: Documenting life events as they occur; generating demographic statistics

**MOSIP (Foundational ID)**:
- **Primary Mandate**: Establish and verify unique digital identity for individuals
- **Authority**: Identity verification, biometric deduplication, credential management
- **Data Ownership**: Authoritative source for identity credentials and biometric data
- **Operational Model**: Centralized identity platform with distributed enrollment
- **Focus**: Preventing duplicate identities; enabling authentication for services

**Why This Distinction Matters**:

Integration does not mean merger. Each system maintains its core responsibilities:
- CRVS verifies vital events happened and issues certificates
- MOSIP verifies individuals are unique and issues identity credentials
- CRVS performs document verification and deduplication at the event level
- MOSIP performs biometric deduplication at the identity level

**Integration Bridges, Not Merges**: When a birth is registered in CRVS, integration enables automatic identity enrollment in MOSIP. But CRVS remains responsible for verifying the birth occurred, while MOSIP ensures the infant receives a unique identity credential.

### 1.5.2 Technical Possibility ≠ Recommended Practice

MOSIP's API architecture could technically support automatic processing of nearly any request from CRVS, including immediate identity deactivation, unlimited reversals, or unrestricted demographic updates. However, **technical capability must be tempered with operational wisdom**.

**The Engineering Trap**:

Just because a feature can be built doesn't mean it should be deployed. Consider:

1. **Automatic Deactivation Example**: 
   - **Technically possible**: CRVS sends fraud report → MOSIP instantly deactivates National ID
   - **Why we don't**: Infants lack biometric verification; wrongful deactivation causes immediate service denial; errors are irreversible without manual intervention
   - **What we do instead**: Flag for manual verification by country authorities (Section 4.6.1)

2. **Unlimited Reversal Example**:
   - **Technically possible**: Allow CRVS to toggle deceased flags repeatedly
   - **Why we don't**: Creates endless loops; destabilizes downstream services (banking, pensions); enables system gaming
   - **What we do instead**: One reversal per person with mandatory manual verification (Section 4.6.3)

**Risk-Based Integration Design**:

Every integration point is evaluated against:
- **Fraud potential**: Can bad actors abuse this?
- **Error impact**: What happens if data is wrong?
- **Reversibility**: Can mistakes be corrected without harm?
- **Legal implications**: Who is liable if something goes wrong?
- **Downstream effects**: How does this affect banking, benefits, healthcare?

This guide's boundaries reflect these evaluations. Where risks are high (fraud detection, identity reversals, death flag changes), we route to manual verification rather than automatic processing.

### 1.5.3 Country & Cultural Variations in CRVS Implementation

CRVS systems are not uniform globally. Implementation varies significantly based on legal frameworks, governance models, administrative capacity, and cultural contexts.

**Governance Model Variations**:

1. **Centralized CRVS**: Single national system with uniform processes
   - Easier integration with consistent data formats
   - Centralized authority for policy decisions
   - Examples: Small island nations, highly digitized countries

2. **Decentralized CRVS**: Regional/provincial systems with varying practices
   - Multiple CRVS partners requiring separate integration
   - Inconsistent data quality and timeliness
   - Requires flexible partner management (Section 3.6)

3. **Hybrid Models**: National framework with local implementation
   - Balance between standardization and local autonomy
   - Most common model globally

**Operational Context Variations**:

1. **Urban vs. Rural Registration**:
   - Urban: Timely registration, digital infrastructure, trained staff
   - Rural: Delayed registration, paper-based, limited connectivity
   - Integration must accommodate delayed birth registrations while managing time windows for fraud claims

2. **Post-Conflict & Fragile State Contexts**:
   - Missing records, displaced populations, mistaken death reports
   - Longer time windows needed for death reversals (1-2 years vs. 30-60 days for birth fraud)
   - Higher potential for administrative errors requiring correction mechanisms

3. **Administrative Capacity**:
   - High capacity: Manual verification queues processed efficiently
   - Low capacity: Manual reviews become bottlenecks; offline escalation becomes essential
   - Integration must provide configurable time windows and escalation paths

**Cultural Sensitivities**:

1. **Death Reporting Practices**:
   - Some cultures have extended mourning periods before formal registration
   - Informant roles vary (next of kin vs. community elders vs. religious authorities)
   - Integration timing must respect cultural practices while maintaining data integrity

2. **Family Structure & Informant Concepts**:
   - Nuclear vs. extended family structures affect "parent" definition
   - Guardian/introducer roles vary culturally
   - eSignet authentication requirements must be configurable for local context

3. **Legal Traditions**:
   - Common law: Precedent-based, flexible interpretation
   - Civil law: Code-based, prescriptive rules
   - Affects manual verification processes, evidence requirements, and dispute resolution

**Why Configurability Matters**:

This guide provides **principled boundaries with country-specific configurability**:
- Time windows (30-60 days, 1-2 years) are configurable based on country context
- Manual verification procedures adapt to administrative capacity
- Field requirements align with local legal frameworks
- But core principles (manual verification for high-risk actions, one-time policies, audit trails) remain consistent

### 1.5.4 Real-World Consequences Framework

Behind every technical boundary in this guide is protection against real-world harm. Understanding these consequences helps implementers make informed customization decisions and advocacy within their organizations.

#### Identity Fraud & Systemic Abuse

**Fraudulent Birth Certificates**:
- **Scenario**: Bad actor obtains fraudulent birth certificate from CRVS, uses it to enroll infant identity in MOSIP
- **Consequence**: Ghost identity used for benefit fraud, document fraud, or child trafficking
- **Protection**: Time-bounded fraud claims (Section 4.6.1) allow correction while preventing endless challenges

**Pension & Benefit Fraud**:
- **Scenario**: Individual reported deceased continues collecting pension via National ID authentication
- **Consequence**: Financial losses, system credibility erosion, genuine beneficiary denial
- **Protection**: Death flag integration (Section 4.3) enables service providers to restrict access

**Multiple Identity Gaming**:
- **Scenario**: Individual obtains multiple birth registrations with different RIDs to receive multiple UINs
- **Consequence**: Duplicate identities for fraud, terrorism, organized crime
- **Protection**: CRVS performs deduplication before MOSIP submission; MOSIP's internal deduplication provides backstop (Section 3.4)

#### Service Denial & Human Rights Impacts

**Wrongful Deceased Flags**:
- **Scenario**: Administrative error or fraud death report marks living person as deceased
- **Consequence**: 
  - Banking accounts frozen
  - Healthcare benefits suspended
  - Pension payments stopped
  - Authentication failures for essential services
  - Legal limbo requiring court intervention
- **Protection**: Manual verification for death reversals (Section 4.6.3); one reversal allowed to correct genuine errors

**Incorrect Identity Deactivation**:
- **Scenario**: Fraudulent fraud claim leads to National ID deactivation
- **Consequence**:
  - Complete loss of access to digital services
  - Employment termination requiring National ID
  - Education enrollment blocked
  - Legal disputes requiring extensive documentation
- **Protection**: Manual verification prevents automatic deactivation (Section 4.6.1); one-time reactivation for genuine errors (Section 4.6.2)

**Demographic Update Failures**:
- **Scenario**: Adult's legitimate name change rejected because biometrics already linked
- **Consequence**: Legal name vs. identity mismatch causing service access issues
- **Protection**: Clear limitation documented (Section 2.4) requiring in-person MOSIP enrollment; protects against CRVS-based demographic fraud for adults

#### Downstream System Disruption

**Banking & Financial Services**:
- Deceased flags trigger account freezes, loan payment blocks, beneficiary transfers
- Incorrect flags cause immediate financial disruption
- Repeated activation/deactivation cycles destabilize integration

**Government Benefits & Pensions**:
- Identity status directly gates benefit eligibility
- False positives deny rightful benefits
- False negatives enable fraud

**Healthcare & Insurance**:
- Identity verification gates emergency care access
- Deceased flags trigger insurance payouts
- Errors have life-or-death implications

**Authentication Services**:
- National ID used for authentication across government and private sector
- Status changes propagate through all integrated services
- System instability cascades

**Protection Through Design**:
- One-time request policies prevent repeated toggling (Sections 4.6.1, 4.6.2, 4.6.3)
- Manual verification provides stability
- Audit trails enable forensic investigation if issues occur

#### Legal & Compliance Liabilities

**Government Liability**:
- Wrongful deactivation: Compensation claims, legal challenges
- Fraud enablement: Liability for fraudulent identities issued
- Data protection: GDPR/privacy law violations if PII mishandled

**Operational Liability**:
- Implementing agencies responsible for integration failures
- System integrators liable for defects causing harm
- Audit requirements for public fund expenditure

**Individual Rights**:
- Right to identity (legal recognition)
- Right to services (healthcare, education)
- Right to appeal and correction
- Due process in identity lifecycle changes

**Protection Through Design**:
- Manual verification preserves due process
- Audit trails enable compliance verification
- Time windows balance correction with stability
- Offline escalation preserves judicial review rights

### 1.5.5 Guiding Principles for Integration Boundaries

These principles inform every design decision in this guide:

#### Principle 1: Manual Verification for High-Stakes Lifecycle Events

**Rationale**: Identity deactivation, reactivation, and death flag reversals are irreversible actions with severe consequences. Human judgment and legal oversight are essential.

**Application**:
- Fraud detection → Manual verification (Section 4.6.1)
- Reactivation requests → Manual verification (Section 4.6.2)
- Death reversals → Manual verification (Section 4.6.3)

**Why Not Automate?**:
- Infants lack biometric verification
- Fraud claims may be malicious
- Administrative errors happen
- Legal disputes require evidence review
- Downstream impacts are severe

**Country Variation**: Countries configure review procedures, approval authorities, and evidence requirements based on legal frameworks and administrative capacity.

#### Principle 2: Time-Bounded Requests

**Rationale**: Unlimited time windows enable endless challenges, destabilize systems, and prevent closure. But overly restrictive windows deny legitimate corrections.

**Application**:
- Birth fraud claims: 30-60 days from initial registration (Section 4.6.1)
- Death reversals: 1-2 years from death declaration (Section 4.6.3)
- Outside windows: Offline grievance/judicial channels

**Why Time Bounds?**:
- Balance correction opportunity with system stability
- Reflect typical error detection timelines
- Force escalation for complex/disputed cases
- Prevent weaponization of integration

**Country Variation**: Windows are configurable based on:
- CRVS registration timelines (urban vs. rural)
- Administrative capacity for timely error detection
- Post-conflict contexts requiring longer windows
- Legal frameworks for dispute resolution

#### Principle 3: Clear Source of Truth Responsibilities

**Rationale**: When two systems integrate, data ownership and decision authority must be crystal clear to prevent conflicts and maintain accountability.

**Application**:
- **CRVS owns**: Vital event data, document verification, event-level deduplication
- **MOSIP owns**: Biometric data, identity-level deduplication, credential management
- **Shared**: Integration metadata, audit trails

**Why This Matters**:
- MOSIP trusts CRVS-verified vital events (Section 3.1)
- CRVS receives MOSIP-issued credentials (Sections 4.2, 4.3)
- Disputes about data accuracy escalate to owning system
- Legal liability aligns with data ownership

**Country Variation**: Multi-CRVS environments require clear partner designation (Section 3.6).

#### Principle 4: One-Time Request Policies

**Rationale**: Unlimited repeated requests enable gaming, create operational burden, and destabilize downstream systems.

**Application**:
- One fraud flag per UIN until verification completes (Section 4.6.1)
- One reactivation per deactivated UIN (Section 4.6.2)
- One death reversal per person; re-reversals offline only (Section 4.6.3)

**Why One-Time?**:
- Prevents endless activation/deactivation loops
- Forces escalation to higher authority for complex cases
- Maintains downstream service stability
- Signals serious processing (not routine)

**What Happens After?**:
- Further requests rejected
- Citizen/CRVS directed to National ID authority
- Offline processes (courts, ombudsman) handle disputes

**Country Variation**: Offline escalation channels vary by legal system and governance model.

#### Principle 5: Offline Escalation Channels

**Rationale**: Not every scenario belongs in automated integration. Some require judicial review, legal interpretation, or high-level policy decisions.

**Application**:
- Requests outside time windows → Offline (Sections 4.6.1, 4.6.3)
- Repeated reversals → Offline (Section 4.6.3)
- Disputed identity status → Offline
- Complex fraud investigations → Offline

**Why Preserve Offline?**:
- Some decisions require judicial authority
- Edge cases need human judgment
- Legal precedent established through courts
- Automation increases risk in rare scenarios

**Country Variation**: Offline channels include courts, ombudsman offices, National ID appeals boards, ministerial review—varies by country.

### 1.5.6 How to Read This Document

Different readers have different needs. This guide is structured to support multiple pathways:

**For Solution Architects**:
- Start here (Section 1.5) for philosophical foundation
- Focus on system boundaries and trust models (Sections 2, 3)
- Understand technical architecture (Section 5)
- Review policy configuration options (Section 10)

**For Developers**:
- Understand "why" (this section) before implementing "how"
- Follow integration workflows (Section 4)
- Implement API specifications (Section 6)
- Reference code examples (Section 13)

**For Policy Makers & Government Officials**:
- Read integration value proposition (Section 1.2)
- Understand principles and real-world consequences (this section)
- Focus on rare scenarios requiring manual verification (Section 4.6)
- Review operational considerations (Section 11)

**For System Integrators**:
- Understand full context (Sections 1, 2, 3)
- Implement standard workflows (Sections 4.2, 4.3, 4.4)
- Configure for country context (Sections 10, 11)
- Test thoroughly (Section 12)

**For CRVS Implementers**:
- Understand MOSIP's role vs. CRVS's role (Sections 1.5.1, 3)
- Review supported use cases (Section 2.3)
- Understand limitations (Section 2.4)
- Plan for edge cases (Section 4.6)

**Key Principle**: The boundaries documented in this guide are not arbitrary technical constraints. They reflect:
- Real-world consequences for individuals
- Legal and ethical considerations
- Operational wisdom from foundational ID implementations globally
- Country-specific variation while maintaining core safety principles

When you encounter a limitation or boundary in subsequent sections, refer back to this section to understand its protective intent.

---

# 2. Integration Overview

## 2.1 CRVS and MOSIP Ecosystem

<div align="right"><figure><img src="../../../.gitbook/assets/CRVS (1).png" alt=""><figcaption><p>Birth and Death Registration</p></figcaption></figure></div>

<!-- Missing Content - Add it --> (Needs detailed ecosystem explanation)

## 2.2 Integration Scope & Boundaries

<!-- Missing Content - Add it --> (Needs clear scope boundaries statement)

## 2.3 Supported Use Cases

Integration can be done for following scenarios:

- Birth Registration and UIN Issuance
- Death Registration and Identity Update
- Demographic Updates (e.g., Name Change, Address Update)
- Rare Scenarios (Fraud Detection, Identity Reversals)

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
4. [Rare scenarios requiring manual verification](#46-handling-rare-scenarios-in-mosipcrvs-integration)
  1. [Fraudulent birth registration - National ID deactivation](#461-fraudulent-birth-registrations---national-id-deactivation-request-from-crvs)
  2. [Reactivation of deactivated National ID](#462-reactivation-of-deactivated-national-id)
  3. [Death flag reversal (fraud death case)](#463-fraud-death-case---reversal-of-the-death-flag)

> **Note**: These are the currently supported scenarios. The rare scenarios (4.6.x) involve manual verification processes and are not fully automated. Additional use cases will be introduced as the integration evolves and expands based on country-specific requirements and feedback.

## 2.4 What's NOT in Scope (Out-of-Scope)

### Integration Limitations

The limitations listed below are not arbitrary technical constraints. Each reflects careful consideration of real-world implications documented in [Section 1.5 (Integration Boundaries: Principles & Real-World Implications)](#15-integration-boundaries-principles--real-world-implications). These boundaries protect individuals, maintain system integrity, and align with the distinct operational models of Foundational ID and Civil Registration systems.

Although the integration scope includes scenarios for birth, death, and updates, there are still some cases where limitations exist.

1. **No Support for New Adult Birth Registrations**: MOSIP does not support new adult birth registrations when the request comes from CRVS.

2. **No Support for Demographic Updates When Biometrics Are Already Linked to a National ID**: MOSIP will not support demographic updates from CRVS for individuals whose biometrics are already linked to a National ID. For such updates, individuals are expected to visit the National ID registration centre (MOSIP).

3. **Integration for Birth and Death Registrations**: The integration works seamlessly for birth and death registrations. However, updates to demographic data are still a work in progress.

4. **Duplicate Request Rejection**: Since the CRVS system is considered the source of truth, MOSIP currently does not reject duplicate birth/death registration requests received from the CRVS system. This can result in multiple UINs for the same infant or an update of the death flag for the same deceased. Deduplication is expected to be handled by CRVS. **Exception**: For rare scenarios (fraud detection, identity reversals) covered in Section 4.6, MOSIP enforces one-time request policies and routes cases to manual verification rather than automatic processing.

5. **No Support for Rejected Packets, Status Updates, and Reason**: If a request fails due to validation issues in MOSIP, there is currently no mechanism to send detailed rejection reasons back to CRVS.

6. **No Automatic Deactivation or Reactivation**: While CRVS can submit fraud reports or reactivation requests (Section 4.6), MOSIP does not automatically deactivate or reactivate National IDs. All such requests are routed to manual verification queues where authorized country authorities make final decisions.

6. **No Automatic Deactivation or Reactivation**: While CRVS can submit fraud reports or reactivation requests (Section 4.6), MOSIP does not automatically deactivate or reactivate National IDs. All such requests are routed to manual verification queues where authorized country authorities make final decisions.

7. **No support for Offline Integration**: This integration works only when online connectivity is available as eSignet authentication is a necessary step before a request is submitted to CRVS.

8. **Use of VID/UIN for Death Registration and Demo Data Updates**: Only VID or UIN can be used to register a death or submit requests for the demo data updates. Currently, MOSIP does not support death updates with any other identifier.

9. **Time-Bound Rare Scenario Requests**: Fraud detection and reversal requests (Section 4.6) are only accepted within configurable time windows from the initial registration/declaration date. Requests outside these windows are automatically rejected and must follow offline grievance channels.

## 2.5 System Roles & Responsibilities

<!-- Missing Content - Add it -->


---

# 3. Core Integration Principles

## 3.1 CRVS as Source of Truth

**CRVS is considered the source of truth**. MOSIP relies on CRVS to perform deduplication and treats CRVS as the authoritative source of vital event data. MOSIP does not perform de-duplication or data validation on requests from CRVS systems because of this trust relationship.

**Assumption:** This approach assumes that the CRVS system is recognized as the authoritative source of vital event data and is legally authorized to issue official birth and death certificates in accordance with the laws of the country.

> **See [foundational-id-vs-civil-registration-two-different-systems](#foundational-id-vs-civil-registration-two-different-systems)** for a detailed explanation of why Foundational ID and CRVS systems maintain distinct responsibilities and authorities.

## 3.2 Trust & Data Verification Model

### Data Verification and Trust

1. The initiation of the request with the data will be from CRVS to MOSIP
2. Data transmitted from CRVS to MOSIP will undergo thorough data/document verification and deduplication before any request is initiated for MOSIP to process.
3. MOSIP will trust all information provided by CRVS and honor the received requests. MOSIP will ensure that the technical and logical validations are all in place for packet processing.
4. The integration should define the mandatory attributes to be exchanged between the two systems. It is crucial to agree upon these attributes to ensure consistent and accurate data exchange.
5. **Exception for Rare Scenarios**: For sensitive cases such as fraud detection (Section 4.6.1), identity reactivation (Section 4.6.2), and death flag reversals (Section 4.6.3), MOSIP does not automatically process requests. Instead, these are routed to manual verification queues where authorized country authorities review evidence and make final decisions. This ensures legal compliance and prevents misuse while maintaining the trust relationship with CRVS.

## 3.3 Identity Lifecycle Management

<!-- Missing Content - Add it -->

## 3.4 De-duplication Strategy

> **Note**: MOSIP relies on CRVS to perform deduplication and treats CRVS as the source of truth. While MOSIP has its internal deduplication mechanism to detect and reject duplicate packets, duplicate and repeated request scenarios are not currently handled for rejection by MOSIP.

**Exception for Rare Scenarios**: For fraud detection, reactivation, and death reversal requests (Section 4.6), MOSIP enforces a **one-time request policy** per National ID. If a fraud flag or reversal request is already in progress or has been previously processed, subsequent duplicate requests are automatically rejected. This prevents repeated requests from creating endless loops and maintains system integrity.

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

<!-- Missing Content - Add it -->

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


## 4.6 Handling Rare Scenarios in MOSIP–CRVS Integration


### Background

MOSIP already supports standard integration workflows with Civil Registration and Vital Statistics (CRVS) systems for birth registration and death registration. These are well-established, high-volume processes that operate smoothly in production environments.

During implementation, CRVS has requested MOSIP to extend the integration to a set of rare scenarios. These scenarios are infrequent, but they have significant legal, operational, or fraud-related implications. They impact the National ID lifecycle, require special handling, and cannot be treated as routine CRVS events.

> **Important**: The manual verification approach for rare scenarios reflects the risk-based integration design principles outlined in [Section 1.5](#15-integration-boundaries-principles--real-world-implications). These scenarios carry high potential for fraud, legal disputes, and service denial if processed automatically.

To address these cases safely and consistently, MOSIP must ensure:

- **Traceability**: Every incoming request is logged with clear metadata such as process, source, timestamps, and reason codes.
- **Controlled Processing**: No direct automatic changes to National ID status; sensitive requests must be routed for internal or country-level manual verification.
- **Policy Alignment**: MOSIP acts only as the foundational ID platform; final decisions rest with authorized country authorities.
- **Auditability**: All requests should leave a reliable audit trail for legal, compliance, and investigative needs.

This document outlines the three rare scenarios that the MOSIP-CRVS integration must support and provides the detailed workflow for each in the following sections.

### Scenario Overview

#### 1. Fraudulent Birth Registration - Deactivation

CRVS may later detect that a submitted birth registration was fraudulent, incorrect, duplicated, or otherwise invalid. In such cases, CRVS needs a mechanism to notify MOSIP so that the corresponding National ID (UIN) can be flagged and routed for manual verification.

This scenario is particularly sensitive because:

- Infants do not yet have biometrics in MOSIP.
- Fraudulent birth certificates are a known global risk for identity systems.
- Direct ID deactivation without human review can cause wrongful service denial and legal disputes.

#### 2. Reactivation of National ID

CRVS may request MOSIP to deactivate a National ID—for example, because the identity was found invalid, duplicated, or incorrectly issued after verification. In rare cases, CRVS may later request reactivation if it determines that the earlier deactivation was incorrect.

This scenario is sensitive because:

- National ID activation status impacts all downstream services.
- Repeated activation/deactivation can destabilize integrations with banking, benefits, and authentication services.
- Errors in these actions carry legal and operational consequences.

#### 3. Reversal of Deceased Flag (Fraud Death or Mistaken Death Entry)

CRVS may detect that a person previously reported as deceased is actually alive. This may occur due to reporting errors, communication delays, mistaken identity, or rare cases in conflict or disaster contexts.

This scenario is highly sensitive because:

- Deceased flags directly affect pensions, insurance, property transfers, and legal identity status.
- Incorrect deceased tagging can lead to complete service denial.
- Reversing this flag must be handled with caution and full traceability.



## 4.6.1 Fraudulent Birth Registrations - National ID Deactivation Request from CRVS

### When Does It Happen?

This rare scenario occurs when a civil registration authority (CRVS) determines that a previously recorded birth registration was fraudulent, incorrect, duplicated, or otherwise invalid. CRVS detects this issue post-registration and needs to notify MOSIP to flag the corresponding National ID (UIN) for review and potential deactivation.

**Triggers**:
- Discovery of fraudulent birth certificate issuance
- Detection of duplicate birth registrations for the same infant
- Identification of data integrity issues in initial registration
- Legal determination of registration invalidity

**Why This Scenario is Sensitive**:
- Infants do not yet have biometric authentication in MOSIP
- Fraudulent birth certificates are a known global identity fraud risk
- Direct ID deactivation without human review can cause wrongful service denial
- Legal and ethical implications require careful handling

> **For detailed context on real-world consequences and design principles, see [Section 1.5.4 (Real-World Consequences Framework)](#154-real-world-consequences-framework)**.

### What Does MOSIP Do?

MOSIP receives the fraud notification from CRVS, validates the request against policy rules (time window, duplicate checks), and routes the case to a manual verification queue. MOSIP does **NOT** automatically deactivate the National ID. Instead, it:

1. **Validates the request** against configured time windows and duplicate prevention rules
2. **Sets a fraud flag** (`Fraud_Birth = True`) on the identity record
3. **Routes the packet** to manual verification using a dedicated Camel workflow
4. **Stores metadata** (process, source, reason) for audit and traceability
5. **Waits for manual decision** from authorized country authorities
6. **Executes deactivation** only after manual approval

### What Does CRVS Receive?

Under the current implementation:
- **No automatic notification** is sent back to CRVS regarding packet status
- CRVS must subscribe to WebSub packet status updates if tracking is required
- Final deactivation status may be communicated through offline channels depending on country policy

> **Note**: Notification strategy for fraud scenarios is under consideration and may be enhanced in future implementations.

### What Is the Workflow?

#### Step 1: CRVS Submits a Fraudulent Birth Request

CRVS submits a "fraudulent birth" request to MOSIP, including:

**Required Fields:**

- **National ID (UIN)** or **packet reference**
- **fraud_birth / deactivate_id** = `True` (property name to be finalized based on country requirements)
- **process** = `CRVS_fraud_birth` / `CRVS_deactivate_ID`
- **source** = `CRVS1`
- **fraud_birth_reason / Deactivation_reason** (string describing the reason for deactivation)
- **date_of_initial_registration**
- **Optional**: Supporting metadata/document references

> **Note**: These fields must be added to the ID schema to support this workflow.

#### Step 2: MOSIP Validates the Request Window

1. **Compare the submitted date of initial registration to the current date.**
2. **If within a configurable window** (e.g., 30–60 days as defined in country policy):
   - MOSIP accepts the request and initiates packet processing.
   - A new tag is introduced to categorize such requests: `CRVS_deactivate_ID`
   - This tag helps in anonymous profiling and tracking.

3. **If outside the time window**:
   - The request is automatically rejected.
   - Citizens must use offline grievance or legal channels.

> **Note**: MOSIP-side validation for garbage values in the `fraud_birth_reason` / `Deactivation_reason` field is **not required**, as notifications to CRVS are not sent in this workflow.

#### Step 3: Route the Packet to Manual Verification

1. **Basic de-duplication** is performed for any incoming packet in MOSIP.
2. Based on the **process** and **source** values, a specific **Camel route** is triggered to route the packet directly to the **manual verification stage**.
3. The following information is provided to the manual reviewer:
   - `fraud_birth_reason` / `Deactivation_reason`
   - Any supporting documents (if evidence collection is supported)
   - Details of the original packet

4. **Set attribute**: `fraud_Birth = True` against the National ID.
   - **No automatic deactivation** occurs at this stage.

5. **Packet details storage**:
   - Stored in **Packet Manager** and **Transaction Table** for reference.

#### Step 4: Enforce One-Time Request Policy

1. If a `Fraud_Birth = True` flag already exists, any subsequent request on the same UIN is **automatically rejected** until verification completes.
2. This prevents duplicate or repeated requests from being processed.

> **Note**: If someone authenticates using the National ID during the manual verification flow, no special flag is added to the KYC info shared with the Relying Party (RP).

#### Step 5: Manual Verification by Country Authorities

1. Responsible admins/legal authorities review:
   - Supporting documentation
   - Original packet details
   - `fraud_birth_reason` / `Deactivation_reason`

2. Authorities decide whether to:
   - **Approve deactivation**, or
   - **Reject the fraud claim**

#### Step 6: Final Action

**If Deactivation is Approved:**

1. The National ID is deactivated **offline** by authorities.
2. MOSIP must add logic to **deactivate the UIN** once the manual verifier approves.
3. **Manual Steps**: Countries can use the existing **Deactivate UIN (MOSIP ID) API** (not recommended for routine use).

**If Rejected:**

1. The packet is rejected, and the flow is completed.
2. No changes are made to the record in the ID system.

### Use Case-Based Notifications

> **Note**: This section is under consideration for MOSIP-side changes. Final notification strategy is to be determined based on country requirements.

---

### Pros & Cons of MOSIP Options

| **Option** | **Advantages** | **Disadvantages / Risks** |
|-----------|---------------|--------------------------|
| **Automatic Online Deactivation** | Fast, immediate response to CRVS fraud detection | High risk of misuse or wrongful deactivation; no biometric authentication for infants; downstream system disruption; legal + ethical liability |
| **Flag + Manual Verification (Recommended)** | Ensures traceability; leverages human/legal judgement; safe for infants; audit trail maintained | Requires manual workload; may delay final resolution; depends on country administrative capacity |
| **Reject All Online Fraud Requests (Offline Only)** | Simplest to implement; full control remains with authorities | Loses benefit of integration; CRVS cannot leverage MOSIP automation; increased overhead and delays; reduced transparency |

---

### Recommended MOSIP Policy

1. **Accept CRVS fraud-birth requests only if submitted within 30–60 days of initial registration.**
2. On valid request, set **`Fraud_Birth = True`** in schema with metadata:
   - **Process** = `CRVS_Fraud_Birth`
   - **Source** = `CRVS1`
3. **Route flagged requests to manual verification queue** using a dedicated Camel route.
4. **Enforce one-time request per UIN** until verification completes; block duplicate requests.
5. **No online deactivation**; final deactivation decision rests with national/regional authorities after their offline review.
6. **Requests outside the time window are automatically rejected**; citizens must use offline grievance or legal channels.

---

### Additional Considerations & Notes

#### Infants / No Biometric Enrollment

- Without biometric verification, automated deactivation carries high risk.
- The **flag + manual review** approach maintains safety and due process.

#### Audit & Traceability

- Maintaining metadata (**Process**, **Source**) ensures every request is logged and traceable.
- This is critical if disputes or legal challenges arise later.

#### Alignment with Global Best Practices

- The recommended workflow mirrors practices observed in other foundational ID systems combining civil registration with identity issuance.
- Sensitive lifecycle events (fraud, death, deactivation) are reserved for manual review.

#### Policy & Country-Specific Customization

- The **30–60 day window** and verification procedures should be configurable per country's legal and governance environment.

#### Infant Death Cases

- Cases of infant death will be handled through a **death registration request** submitted by CRVS.

---


## 4.6.2 Reactivation of Deactivated National ID

### When Does It Happen?

This scenario occurs in rare cases when a National ID has been deactivated following a fraud-birth verification (see Section 4.6.1), but CRVS or country authorities later determine that the original deactivation decision was incorrect or unjustified.

**Triggers**:
- Discovery that the fraud claim was erroneous
- New evidence proving the birth registration was legitimate
- Administrative error correction after review
- Legal reversal of the deactivation decision

**Why This Scenario is Sensitive**:
- Original deactivation involved thorough verification by country authorities
- Automatic reactivation could bypass legal and administrative safeguards
- Multiple reactivation/deactivation cycles could compromise system integrity
- Downstream services (banking, benefits, authentication) may be affected
- Risk of operational confusion or misuse

> **For detailed context on real-world consequences and design principles, see [Section 1.5.4 (Real-World Consequences Framework)](#154-real-world-consequences-framework)**.

### What Does MOSIP Do?

MOSIP receives the reactivation request from CRVS, validates that the National ID was previously deactivated with a fraud flag, and routes the case to manual verification. MOSIP does **NOT** automatically reactivate the National ID. Instead, it:

1. **Validates the request** by confirming the UIN exists and was deactivated with `Fraud_Birth = True`
2. **Checks time windows** to ensure the request falls within policy-defined limits
3. **Prevents duplicate requests** by rejecting if a reactivation is already under review
4. **Routes to manual verification** using the same Camel workflow as deactivation
5. **Provides context** (original fraud reason, supporting documentation) to reviewers
6. **Waits for manual decision** from authorized country authorities
7. **Executes reactivation** only after manual approval

### What Does CRVS Receive?

Under the current implementation:
- **No automatic notification** is sent back to CRVS regarding reactivation status
- CRVS may subscribe to WebSub for packet status updates if needed
- Final reactivation status may be communicated through offline coordination

> **Note**: The notification mechanism for reactivation scenarios is under policy review and may be enhanced based on country requirements.

### What Is the Workflow?

#### Step 1: CRVS Request Submission

CRVS submits a reactivation request via the MOSIP packet API with the following parameters:

**Required Fields:**

- **Fraud_birth** = `False` (indicates reactivation request)
- **Process** = `CRVS_fraud_birth`
- **Source** = `CRVS1`
- **National_ID** (UIN of the individual)
- **Fraud_birth_reason** (reason for the original deactivation)
- **Optional**: Supporting metadata/document references

> **Note**: Should evidence documentation be required as reference for manual review?

#### Step 2: Validation by MOSIP

1. **Confirm UIN status**: Verify that the UIN exists and was deactivated with `Fraud_birth = True`.
2. **Reject duplicate requests**: If a previous reactivation request for the same UIN is already under review, reject the new request.
3. **Time window validation**: Requests are valid only within a defined period from the initial birth registration date. Requests outside this window are automatically rejected, requiring citizens to follow offline grievance/legal processes.
   
   > **Note**: Should the time window be calculated from the date of initial registration or from the date of deactivation?

4. **Optional validation**: Should MOSIP-side validation for garbage values in the `fraud_birth_reason`/`Deactivation_reason` field be added?

5. If all validations pass, initiate packet processing.

#### Step 3: Routing to Manual Verification

1. Route the packet to a dedicated manual verification queue using the Camel route associated with `CRVS_fraud_birth`.
2. Include the `Fraud_birth_reason` to provide context for the country authorities' review.

> **Note**: Where should the packet details be stored for reference during manual verification?

#### Step 4: Manual Verification by Authorities

Country administrators/legal authorities review the case, using the original deactivation reason and supporting documentation.

**Decision Options:**

- **Approve**: Reactivate the National ID offline.
- **Reject**: Keep the National ID deactivated. The fraud flag may be retained or cleared depending on policy.

#### Step 5: Logging and Audit

1. Store all request, validation, and verification details in MOSIP for compliance and traceability.
2. Ensure traceability of all actions for potential audits or legal review.

---

### MOSIP Options and Pros/Cons

| **Option** | **Advantages** | **Disadvantages / Risks** |
|-----------|---------------|--------------------------|
| **Automatic Online Reactivation** | Fast; reduces manual overhead | High risk of fraud; bypasses legal review; multiple toggling possible; downstream service disruption; audit challenges |
| **Receive Request + Route to Manual Verification (Recommended)** | Preserves legal and operational control; ensures traceability; allows genuine corrections | Requires administrative effort; slight delay in final resolution |
| **Reject All Online Reactivation Requests (Offline Only)** | Fully prevents misuse; simple to implement | Citizens and CRVS cannot leverage integration; delays in legitimate corrections; reduced transparency |

---

### Recommended MOSIP Policy

1. **Accept CRVS reactivation requests only if the UIN was previously deactivated** with `Fraud_birth = True`.
2. Use `Fraud_birth_reason` to reference the original deactivation during manual verification.
3. **Route flagged requests to the manual verification queue** using the dedicated Camel route (`CRVS_fraud_birth`).
4. **Enforce one active request per UIN**: No subsequent requests are accepted until the verification is complete.
5. **Validate time window**: Requests must fall within the defined time window from the initial birth registration date. Requests outside this window are rejected, and offline grievance/legal channels must be followed.
6. **No automatic reactivation is allowed**; final decisions rest with country authorities.

---

### Additional Considerations

#### Traceability

Maintaining metadata (**Process**, **Source**, and **Fraud_birth_reason**) ensures every reactivation request is auditable.

#### Operational Safety

Limiting reactivation to manual verification avoids misuse and prevents disruption to downstream authentication services.

#### Time Window Logic

The window for reactivation should be calculated from the initial birth registration date, consistent with the original deactivation policy.

#### Alignment with Global Best Practices

This workflow mirrors international practices, combining civil registration with identity management while reserving sensitive lifecycle events for human review.

---

## 4.6.3 Fraud Death Case - Reversal of the Death Flag

### When Does It Happen?

This rare but critical scenario occurs when CRVS discovers that a person previously registered as deceased is actually alive. These situations are infrequent but carry significant legal, operational, and identity-related implications.

**Triggers**:
- **Administrative errors**: Data-entry mistakes or communication delays in death reporting
- **Conflict or displacement**: Individuals reported missing or presumed dead later reappear
- **Disaster scenarios**: Mistaken identity during emergency response operations
- **Fraud cases**: Intentional misreporting of death for financial or legal gain
- **Delayed communication**: Individual resurfaces after being out of contact

**Why This Scenario is Sensitive**:
- Deceased flags directly affect pensions, insurance, property transfers, and legal identity status
- Incorrect deceased tagging can lead to complete service denial
- Reversing the flag requires extreme caution and full traceability
- May require longer time windows than birth fraud cases (e.g., post-conflict scenarios)
- Only one reversal per individual is permitted online; re-reversals require offline intervention

> **For detailed context on real-world consequences and design principles, see [Section 1.5.4 (Real-World Consequences Framework)](#154-real-world-consequences-framework)**.

**Standard Death Registration Context**:

For reference, when CRVS submits a standard death registration (Section 4.3), MOSIP updates:
- `Declared_as_Deceased = Y`
- `Deceased_Declaration_Date` = Date of death declaration
- National ID remains technically active; only the deceased flag is set
- Downstream services may restrict access based on this flag

### What Does MOSIP Do?

MOSIP receives the death reversal request from CRVS, validates the request against multiple criteria (current status, time windows, reversal history), and routes the case to manual verification. MOSIP does **NOT** automatically reverse the deceased flag. Instead, it:

1. **Validates current status** to ensure the individual is currently marked as deceased
2. **Checks time windows** (recommended 1-2 years from death declaration, configurable)
3. **Prevents duplicate requests** by rejecting if a reversal is already in progress
4. **Verifies reversal history** to enforce one-reversal-per-person policy
5. **Tags the packet** with `CRVS_Fraud_Death` for tracking and profiling
6. **Routes to manual verification** using dedicated Camel workflow
7. **Provides complete context** (reversal reason, death history, supporting evidence) to reviewers
8. **Waits for manual decision** from authorized country authorities
9. **Updates deceased flag** (`Declared_as_Deceased = N`) only after manual approval

### What Does CRVS Receive?

Under the current implementation:
- **No automatic notification** is sent back to CRVS regarding reversal status
- CRVS may subscribe to WebSub for packet status updates if required
- Final reversal outcome may be communicated through offline channels per country policy

> **Note**: Acknowledgement to CRVS in case of rejection is under consideration for future enhancement.

### What Is the Workflow?

#### Step 1: CRVS Submits Death Reversal Request

When CRVS determines that an individual marked as deceased is alive, they submit a reversal request with:

**Required Fields:**

- **UIN/VID**: National ID of the individual
- **Declared_as_Deceased**: `N` (indicating reversal)
- **Process**: `CRVS_Fraud_Death`
- **Source**: `CRVS1`
- **Deceased_Declaration_Date**: Original date of death declaration
- **Reversal_Reason**: Description of why reversal is requested
- **Supporting Evidence**: Optional documentation references

> **Note**: The process name `CRVS_Fraud_Death` distinguishes reversal requests from standard death registrations.

#### Step 2: MOSIP Validates the Request

MOSIP performs the following validations before accepting the reversal request:

##### 1. Current Status Verification

- **Check**: Ensure the individual is currently marked as deceased (`Declared_as_Deceased = Y`)
- **Action**: If not currently deceased, reject the request

##### 2. Time Window Validation

Unlike fraud-birth scenarios, death reversals may require a **longer validity window** due to:

- Extended periods before reappearance (e.g., war, displacement, disaster)
- Lower risk since MOSIP only flips the flag without full ID reactivation

**Recommendation:**

- Calculate time window from `Deceased_Declaration_Date`
- **Recommended window**: 1-2 years (configurable by country policy)
- Countries can adjust based on local laws and contextual factors

##### 3. Duplicate Request Check

- **Check**: Verify if a death reversal request for the same UIN is already in progress
- **Action**: If duplicate found, reject the new request

##### 4. Reversal History Check

- **Check**: Verify if a previous death reversal has already been processed for this UIN
- **Action**: Accept only **one death reversal request per person** through CRVS
- **If second reversal attempted**: Reject and instruct the requestor to contact the National ID department for manual offline resolution

##### 5. Add Relevant Tags

- Tag the packet with `CRVS_Fraud_Death` for tracking and anonymous profiling
- Store metadata for audit and traceability

> **Note**: Should MOSIP send acknowledgement to CRVS in case of rejection? This is under consideration.

#### Step 3: Route to Manual Verification

1. Route the packet to the **manual review queue** via the Camel route tied to `CRVS_Fraud_Death`
2. Provide the following information to the manual reviewer:
   - `Reversal_Reason`
   - **Complete history** of all updates to the packet for this individual
   - Supporting documentation (if evidence collection is supported)
   - Original death declaration details
3. Country authorities verify evidence before allowing reversal

#### Step 4: Manual Verification Decision

Authorized country administrators/legal authorities review:

- Supporting documentation
- Complete packet history
- Reversal reason and evidence
- Context of original death declaration

**Decision Options:**

##### Option 1: Approve the Reversal

1. Set `Declared_as_Deceased = N`
2. Remove the deceased status/flag in MOSIP ID repository (backend update by NID)
3. The National ID continues to be active (since it was never deactivated, only flagged)
4. Audit logs capture the complete chain of actions
5. No notification to CRVS (current policy)

##### Option 2: Reject the Reversal

1. `Declared_as_Deceased` remains `Y`
2. No further online reversal requests are accepted for this UIN
3. Future appeals must go to the National ID authority through **offline processes**
4. Packet status updated with rejection reason

#### Step 5: Logging and Audit

MOSIP maintains comprehensive audit trails including:

- **Original death declaration**: Date, source, reason
- **Reversal request**: Submission date, reason, supporting evidence
- **Validation outcomes**: All checks performed and results
- **Verification decision**: Approve/reject with justification
- **Complete action chain**: Full traceability for legal and administrative audits

The request metadata (**Process**, **Source**, and deceased fields) help distinguish:

- Original death registrations (`CRVS_Death`)
- Fraud/death reversals (`CRVS_Fraud_Death`)

---

### MOSIP Options and Pros/Cons

| **Option** | **Advantages** | **Disadvantages / Risks** |
|-----------|---------------|--------------------------|
| **Automatic Online Reversal** | Fast; immediate response | Very risky: could unmark deceased individuals without legal confirmation; prone to misuse; impacts downstream services without verification; legal and ethical liability |
| **Accept Request + Manual Verification (Recommended)** | Legally safe; aligns with civil registration practices; maintains audit trail; ensures human judgement | Requires administrative involvement; may delay final resolution |
| **No Online Reversal Allowed (Offline Only)** | Maximum safety; full control remains with authorities | Reduces transparency; delays correction for genuine cases; loses benefit of integration |

---

### Recommended MOSIP Policy

1. **Use dedicated process value**: `CRVS_Fraud_Death` must be used for all reversal requests
2. **Source remains consistent**: `CRVS1` (or appropriate CRVS system identifier)
3. **Current status validation**: Only allow reversal if person is currently flagged as deceased (`Declared_as_Deceased = Y`)
4. **One-time reversal policy**: Accept only **one reversal request per person** through online CRVS integration
5. **Reject re-reversals**: Any subsequent reversal attempts must be handled offline by National ID authorities
6. **Longer time window**: Maintain a configurable time window (1-2 years recommended) from `Deceased_Declaration_Date` due to unique real-world scenarios (war, disaster, displacement)
7. **Mandatory manual verification**: All reversal requests must enter the manual verification queue; no automatic changes
8. **Full auditability**: Record all events, decisions, and supporting evidence

---

### Additional Considerations

#### National ID Status

- **National ID is never deactivated** upon death declaration; only the deceased flag is set
- This makes reversal **lower-risk** compared to reactivation scenarios
- Downstream services rely on the flag for access control decisions

#### Time Window Flexibility

- Longer windows (1-2 years) accommodate real-world contexts:
  - War or conflict zones
  - Natural disasters
  - Displacement scenarios
  - Administrative backlogs
- Countries can configure based on local laws and operational needs

#### Clear Rejection Rules

- **One reversal per person** prevents repeated requests from becoming an endless loop
- Forces escalation to offline channels for complex or disputed cases
- Maintains system integrity and prevents misuse

#### Traceability

- Using consistent field structure (**Informant info + Deceased info**) enables:
  - Clear logging across all death-related events
  - Easier investigations and audits
  - Pattern detection for fraud prevention

#### Downstream System Impacts

- Financial services (insurance, pensions)
- Property transfers
- Legal identity status
- Access to government services

All these systems rely on the deceased flag, making reversal a sensitive operation requiring careful handling.






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
   - `CRVS_fraud_birth` or `CRVS_deactivate_ID` - When submitting a fraud detection/deactivation request (Section 4.6.1 and 4.6.2)
   - `CRVS_Fraud_Death` - When submitting a death flag reversal request (Section 4.6.3)
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

**Additional Fields for Rare Scenarios (Section 4.6):**

These fields must be added to the ID schema to support fraud detection, reactivation, and death reversal workflows:

**For Fraud Birth / Deactivation Requests (4.6.1 & 4.6.2):**
1. `fraud_birth` or `Fraud_Birth`: Boolean field (`True` for deactivation, `False` for reactivation)
2. `fraud_birth_reason` or `Deactivation_reason`: String describing the reason for deactivation/reactivation
3. `date_of_initial_registration`: Date of the original birth registration
4. `National_ID`: UIN of the individual

**For Death Reversal Requests (4.6.3):**
1. `Declared_as_Deceased`: String field (`Y` = deceased, `N` = reversal)
2. `Deceased_Declaration_Date`: Original date of death declaration
3. `Reversal_Reason`: Description of why reversal is requested
4. `UIN` or `VID`: National ID of the individual

> **Note**: Field names may vary based on country-specific ID schema design. Consult Section 4.6 for detailed workflow requirements.

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