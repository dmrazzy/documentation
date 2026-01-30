# Integration Boundaries, Limitation and Real-World Implications

## Overview

MOSIP’s API architecture is technically capable of automatically processing a wide range of requests from CRVS systems, including identity creation, demographic changes, status updates, deactivation, and reversals. However, technical capability does not imply recommended or default practice.

As a foundational identity system, MOSIP deliberately enforces integration boundaries to balance automation with legal accountability, data integrity, human rights protection, and downstream system stability. Certain CRVS-initiated actions are therefore **not supported as part of the default integration**, even though they are technically feasible. These boundaries are informed by real-world consequences observed across large-scale national ID implementations.

The following subsections describe the **default integration limitations**, the rationale behind each, and the implications of removing these safeguards.

#### 1. Adult Registration via CRVS Is Not Supported by Default

The default MOSIP–CRVS integration supports identity creation through CRVS only for **infant birth registrations**. Registration of adults through CRVS is not supported.

**Rationale**

For adults, biometric data represents the highest level of identity assurance available in MOSIP. Biometric capture and verification are essential to ensure uniqueness, prevent impersonation, and protect against identity fraud. As a result, adults are required to register through designated National ID enrollment centers, where biometric capture is mandatory and supervised.

MOSIP is configured by default to treat individuals above a configured age threshold as adults. Once this threshold is crossed, biometric presence is required for new identity creation, and CRVS-based registration is intentionally restricted.

**Consequences if Adult Registration via CRVS Were Allowed Without Biometrics**

Allowing adult registrations via CRVS without biometric capture would significantly weaken the assurance level of the national ID system. It would increase the risk of duplicate or fraudulent identities, enable impersonation, and undermine trust in downstream services such as banking, benefits, and authentication-based access controls. Over time, this could erode confidence in the foundational ID itself and expose the system to organized misuse.

#### 2. Demographic Updates for Identities with Linked Biometrics Are Not Supported by Default

The default integration does not support demographic update or correction requests from CRVS for identities where biometrics have already been linked. In practice, this primarily applies to adult citizens.

**Rationale**

MOSIP considers biometric verification to be the strongest mechanism for confirming that the individual requesting a demographic change is the rightful identity holder. For identities with linked biometrics, allowing demographic updates based solely on CRVS submissions—without biometric or in-person verification—would weaken this assurance model.

As a result, demographic updates via CRVS are limited to identities without linked biometrics, which are typically infants below a configured age threshold.

**Consequences if Adult Demographic Updates Were Allowed Automatically**

Allowing demographic updates for biometric-linked identities without direct verification could lead to unintended or malicious changes propagating across multiple dependent systems. Citizens may face service denials, authentication failures, or legal inconsistencies if their identity data changes unexpectedly. In many cases, individuals may not fully understand the downstream impact of such automatic updates, leading to disputes and loss of trust in public systems.

**Note**

By default, MOSIP considers individuals under the age of five as children. This age threshold is configurable based on country-specific policies and legal requirements.

#### 3. Offline Authentication Is Not Supported in the Default Integration

The default integration does not support offline authentication for CRVS-initiated requests. MOSIP recommends online authentication of the reporting individual or approving authority using the eSignet module for all CRVS events, including birth, death, and demographic updates.

**Rationale**

MOSIP requires accountability for every request that affects the identity lifecycle. Online authentication using eSignet ensures that the identity of the person reporting an event—or the legal authority approving it—is known, verifiable, and auditable. The eSignet userinfo token is shared with MOSIP to establish this accountability.

In cases where CRVS data is collected remotely or offline, the expectation is that once connectivity is restored, the responsible registrar or administrator authenticates using their National ID via eSignet before the request is submitted to MOSIP.

**Consequences of Allowing Requests Without eSignet Authentication**

Without authenticated accountability, it becomes difficult to trace who initiated or approved identity-affecting actions. This opens the system to misuse, including unauthorized submissions, fraudulent reporting, and manipulation of identity states for personal or financial gain. Lack of authentication also weakens auditability, complicates legal investigations, and undermines confidence in the integrity of the integration.

#### 4. Automatic Deactivation of National ID on Death Is Not Supported by Default

The default integration does not support automatic deactivation of a National ID upon receipt of a death event from CRVS. Instead, MOSIP marks the identity with a deceased flag.

**Rationale**

A National ID is often deeply integrated across multiple services, including banking, insurance, pensions, healthcare, and inheritance-related processes. Automatic deactivation would immediately disrupt these services, affecting not only the deceased individual but also their family members and dependents.

By using a deceased flag, MOSIP allows service providers to make informed decisions during authentication flows, based on their own legal and operational requirements.

**Consequences if Automatic Deactivation Were Enabled**

Automatic deactivation could freeze accounts, halt benefit disbursements, block insurance claims, and create legal ambiguity. It also increases the risk of misuse, where false or premature death reports could be weaponized to cause harm or financial disruption.

#### 5. Automatic Deactivation for Incorrect Birth Registrations Is Not Supported by Default

In cases where CRVS identifies an incorrect birth registration after an infant identity has already been created, the default integration does not support automatic deactivation. Such requests are routed to MOSIP’s manual verification queue.

**Rationale**

Even infant identities may already be linked to benefits, healthcare, or social services. Immediate deactivation without verification could disrupt essential services for the child and family. Manual verification ensures that legal authorities can assess evidence, intent, and impact before making a decision.

**Consequences of Automatic Deactivation**

Automatic deactivation in such scenarios could cause unnecessary hardship, deny critical services, and create administrative confusion. However, unlike adult cases, deactivation _may_ be performed after manual verification if the incorrect registration is confirmed and identified early.

#### 6. Automatic Reactivation of Deactivated Infant IDs Is Not Supported by Default

Once an infant identity is deactivated following manual verification of an incorrect birth, the default integration does not support automatic reactivation of the same identity.

**Rationale**

If a birth is later confirmed to be legitimate, the correct approach is to register the birth again and issue a new identity. Reactivating a previously deactivated ID undermines identity finality and weakens system control.

**Consequences if Reactivation Were Allowed**

Allowing automatic reactivation would enable repeated toggling of identity states, encouraging system gaming and external control of the national ID lifecycle. This would significantly weaken the authority of the National ID system and is therefore not recommended.

#### 7. Automatic Reversal of Deceased Flag Is Not Supported by Default

The default integration does not support automatic reversal of a deceased flag. If CRVS determines that a death report was incorrect, the reversal request is routed to MOSIP’s manual verification queue.

**Rationale**

Reversing a death status has far-reaching implications, including insurance settlements, pension payouts, inheritance claims, and service eligibility. Such reversals require careful verification from a National ID authority perspective, beyond CRVS records alone.

**Consequences of Automatic Reversal**

Automatic reversals could enable misuse, repeated toggling of death status, and systemic instability. This could result in financial fraud, legal disputes, and erosion of trust across all systems relying on the National ID.

#### Final Note on Technical Capability vs. Policy Choice

All the above limitations are **policy-driven defaults**, not technical constraints. From a purely technical standpoint, MOSIP can be configured to support these actions automatically. However, countries must fully understand the **legal, operational, social, and downstream consequences** before enabling such capabilities.

These boundaries exist to protect individuals, preserve system integrity, and ensure that foundational identity remains stable, trustworthy, and governed by accountable human oversight where risks are high.

***

<!--

### Overview

Integration does not mean merger, it's a bridge between the two systems and that's why each system maintains its core responsibilities. This section explains the philosophical foundation behind the technical boundaries, limitations, and design decisions documented throughout this guide. While MOSIP's technical capabilities could support various integration patterns, not all technically feasible approaches are recommended or appropriate.

### Foundational ID vs. Civil Registration: Two Different Systems

MOSIP (Foundational ID) and CRVS (Civil Registration and Vital Statistics) are complementary but fundamentally distinct systems with different mandates, authorities, and operational models.

**CRVS Systems**:

* **Primary Mandate**: Legal recognition and registration of vital events (births, deaths, marriages, divorces)
* **Authority**: Empowered by civil registration laws to issue legally binding certificates
* **Data Ownership**: Authoritative source for vital event data
* **Operational Model**: Decentralized or centralized registration offices, often aligned with administrative boundaries
* **Focus**: Documenting life events as they occur; generating demographic statistics

**MOSIP (Foundational ID)**:

* **Primary Mandate**: Establish and verify unique digital identity for individuals
* **Authority**: Identity verification, biometric deduplication, credential management
* **Data Ownership**: Authoritative source for identity credentials and biometric data
* **Operational Model**: Centralized identity platform with distributed enrollment
* **Focus**: Preventing duplicate identities; enabling authentication for services

**Why This Distinction Matters**:

Integration does not mean merger. Each system maintains its core responsibilities:

* CRVS verifies vital events happened and issues certificates
* MOSIP verifies individuals are unique and issues identity credentials
* CRVS performs document verification and deduplication at the event level
* MOSIP performs biometric deduplication at the identity level

**Integration Bridges, Not Merges**: When a birth is registered in CRVS, integration enables automatic identity enrollment in MOSIP. But CRVS remains responsible for verifying the birth occurred, while MOSIP ensures the infant receives a unique identity credential.

### Technical Possibility ≠ Recommended Practice

MOSIP's API architecture could technically support automatic processing of nearly any request from CRVS, including immediate identity deactivation, unlimited reversals, or unrestricted demographic updates. However, **technical capability must be tempered with operational wisdom**.

**The Engineering Trap**:

Just because a feature can be built doesn't mean it should be deployed. Consider:

1. **Automatic Deactivation Example**:
   * **Technically possible**: CRVS sends fraud report → MOSIP instantly deactivates National ID
   * **Why we don't**: Infants lack biometric verification; wrongful deactivation causes immediate service denial; errors are irreversible without manual intervention
   * **What we do instead**: Flag for manual verification by country authorities (Section 4.6.1)
2. **Unlimited Reversal Example**:
   * **Technically possible**: Allow CRVS to toggle deceased flags repeatedly
   * **Why we don't**: Creates endless loops; destabilizes downstream services (banking, pensions); enables system gaming
   * **What we do instead**: One reversal per person with mandatory manual verification (Section 4.6.3)

**Risk-Based Integration Design**:

Every integration point is evaluated against:

* **Fraud potential**: Can bad actors abuse this?
* **Error impact**: What happens if data is wrong?
* **Reversibility**: Can mistakes be corrected without harm?
* **Legal implications**: Who is liable if something goes wrong?
* **Downstream effects**: How does this affect banking, benefits, healthcare?

This guide's boundaries reflect these evaluations. Where risks are high (fraud detection, identity reversals, death flag changes), we route to manual verification rather than automatic processing.

### Country & Cultural Variations in CRVS Implementation

CRVS systems are not uniform globally. Implementation varies significantly based on legal frameworks, governance models, administrative capacity, and cultural contexts.

**Governance Model Variations**:

1. **Centralized CRVS**: Single national system with uniform processes
   * Easier integration with consistent data formats
   * Centralized authority for policy decisions
   * Examples: Small island nations, highly digitized countries
2. **Decentralized CRVS**: Regional/provincial systems with varying practices
   * Multiple CRVS partners requiring separate integration
   * Inconsistent data quality and timeliness
   * Requires flexible partner management (Section 3.6)
3. **Hybrid Models**: National framework with local implementation
   * Balance between standardization and local autonomy
   * Most common model globally

**Operational Context Variations**:

1. **Urban vs. Rural Registration**:
   * Urban: Timely registration, digital infrastructure, trained staff
   * Rural: Delayed registration, paper-based, limited connectivity
   * Integration must accommodate delayed birth registrations while managing time windows for fraud claims
2. **Post-Conflict & Fragile State Contexts**:
   * Missing records, displaced populations, mistaken death reports
   * Longer time windows needed for death reversals (1-2 years vs. 30-60 days for birth fraud)
   * Higher potential for administrative errors requiring correction mechanisms
3. **Administrative Capacity**:
   * High capacity: Manual verification queues processed efficiently
   * Low capacity: Manual reviews become bottlenecks; offline escalation becomes essential
   * Integration must provide configurable time windows and escalation paths

**Cultural Sensitivities**:

1. **Death Reporting Practices**:
   * Some cultures have extended mourning periods before formal registration
   * Informant roles vary (next of kin vs. community elders vs. religious authorities)
   * Integration timing must respect cultural practices while maintaining data integrity
2. **Family Structure & Informant Concepts**:
   * Nuclear vs. extended family structures affect "parent" definition
   * Guardian/introducer roles vary culturally
   * eSignet authentication requirements must be configurable for local context
3. **Legal Traditions**:
   * Common law: Precedent-based, flexible interpretation
   * Civil law: Code-based, prescriptive rules
   * Affects manual verification processes, evidence requirements, and dispute resolution

**Why Configurability Matters**:

This guide provides **principled boundaries with country-specific configurability**:

* Time windows (30-60 days, 1-2 years) are configurable based on country context
* Manual verification procedures adapt to administrative capacity
* Field requirements align with local legal frameworks
* But core principles (manual verification for high-risk actions, one-time policies, audit trails) remain consistent

### Real-World Consequences Framework

Behind every technical boundary in this guide is protection against real-world harm. Understanding these consequences helps implementers make informed customization decisions and advocacy within their organizations.

**Identity Fraud & Systemic Abuse**

**Fraudulent Birth Certificates**:

* **Scenario**: Bad actor obtains fraudulent birth certificate from CRVS, uses it to enroll infant identity in MOSIP
* **Consequence**: Ghost identity used for benefit fraud, document fraud, or child trafficking
* **Protection**: Time-bounded fraud claims (Section 4.6.1) allow correction while preventing endless challenges

**Pension & Benefit Fraud**:

* **Scenario**: Individual reported deceased continues collecting pension via National ID authentication
* **Consequence**: Financial losses, system credibility erosion, genuine beneficiary denial
* **Protection**: Death flag integration (Section 4.3) enables service providers to restrict access

**Multiple Identity Gaming**:

* **Scenario**: Individual obtains multiple birth registrations with different RIDs to receive multiple UINs
* **Consequence**: Duplicate identities for fraud, terrorism, organized crime
* **Protection**: CRVS performs deduplication before MOSIP submission; MOSIP's internal deduplication provides backstop (Section 3.4)

**Service Denial & Human Rights Impacts**

**Wrongful Deceased Flags**:

* **Scenario**: Administrative error or fraud death report marks living person as deceased
* **Consequence**:
  * Banking accounts frozen
  * Healthcare benefits suspended
  * Pension payments stopped
  * Authentication failures for essential services
  * Legal limbo requiring court intervention
* **Protection**: Manual verification for death reversals (Section 4.6.3); one reversal allowed to correct genuine errors

**Incorrect Identity Deactivation**:

* **Scenario**: Fraudulent fraud claim leads to National ID deactivation
* **Consequence**:
  * Complete loss of access to digital services
  * Employment termination requiring National ID
  * Education enrollment blocked
  * Legal disputes requiring extensive documentation
* **Protection**: Manual verification prevents automatic deactivation (Section 4.6.1); one-time reactivation for genuine errors (Section 4.6.2)

**Demographic Update Failures**:

* **Scenario**: Adult's legitimate name change rejected because biometrics already linked
* **Consequence**: Legal name vs. identity mismatch causing service access issues
* **Protection**: Clear limitation documented (Section 2.4) requiring in-person MOSIP enrollment; protects against CRVS-based demographic fraud for adults

**Downstream System Disruption**

**Banking & Financial Services**:

* Deceased flags trigger account freezes, loan payment blocks, beneficiary transfers
* Incorrect flags cause immediate financial disruption
* Repeated activation/deactivation cycles destabilize integration

**Government Benefits & Pensions**:

* Identity status directly gates benefit eligibility
* False positives deny rightful benefits
* False negatives enable fraud

**Healthcare & Insurance**:

* Identity verification gates emergency care access
* Deceased flags trigger insurance payouts
* Errors have life-or-death implications

**Authentication Services**:

* National ID used for authentication across government and private sector
* Status changes propagate through all integrated services
* System instability cascades

**Protection Through Design**:

* One-time request policies prevent repeated toggling (Sections 4.6.1, 4.6.2, 4.6.3)
* Manual verification provides stability
* Audit trails enable forensic investigation if issues occur

**Legal & Compliance Liabilities**

**Government Liability**:

* Wrongful deactivation: Compensation claims, legal challenges
* Fraud enablement: Liability for fraudulent identities issued
* Data protection: GDPR/privacy law violations if PII mishandled

**Operational Liability**:

* Implementing agencies responsible for integration failures
* System integrators liable for defects causing harm
* Audit requirements for public fund expenditure

**Individual Rights**:

* Right to identity (legal recognition)
* Right to services (healthcare, education)
* Right to appeal and correction
* Due process in identity lifecycle changes

**Protection Through Design**:

* Manual verification preserves due process
* Audit trails enable compliance verification
* Time windows balance correction with stability
* Offline escalation preserves judicial review rights

### Guiding Principles for Integration Boundaries

These principles inform every design decision in this guide:

**Principle 1: Manual Verification for High-Stakes Lifecycle Events**

**Rationale**: Identity deactivation, reactivation, and death flag reversals are irreversible actions with severe consequences. Human judgment and legal oversight are essential.

**Application**:

* Fraud detection → Manual verification (Section 4.6.1)
* Reactivation requests → Manual verification (Section 4.6.2)
* Death reversals → Manual verification (Section 4.6.3)

**Why Not Automate?**:

* Infants lack biometric verification
* Fraud claims may be malicious
* Administrative errors happen
* Legal disputes require evidence review
* Downstream impacts are severe

**Country Variation**: Countries configure review procedures, approval authorities, and evidence requirements based on legal frameworks and administrative capacity.

**Principle 2: Time-Bounded Requests**

**Rationale**: Unlimited time windows enable endless challenges, destabilize systems, and prevent closure. But overly restrictive windows deny legitimate corrections.

**Application**:

* Birth fraud claims: 30-60 days from initial registration (Section 4.6.1)
* Death reversals: 1-2 years from death declaration (Section 4.6.3)
* Outside windows: Offline grievance/judicial channels

**Why Time Bounds?**:

* Balance correction opportunity with system stability
* Reflect typical error detection timelines
* Force escalation for complex/disputed cases
* Prevent weaponization of integration

**Country Variation**: Windows are configurable based on:

* CRVS registration timelines (urban vs. rural)
* Administrative capacity for timely error detection
* Post-conflict contexts requiring longer windows
* Legal frameworks for dispute resolution

**Principle 3: Clear Source of Truth Responsibilities**

**Rationale**: When two systems integrate, data ownership and decision authority must be crystal clear to prevent conflicts and maintain accountability.

**Application**:

* **CRVS owns**: Vital event data, document verification, event-level deduplication
* **MOSIP owns**: Biometric data, identity-level deduplication, credential management
* **Shared**: Integration metadata, audit trails

**Why This Matters**:

* MOSIP trusts CRVS-verified vital events (Section 3.1)
* CRVS receives MOSIP-issued credentials (Sections 4.2, 4.3)
* Disputes about data accuracy escalate to owning system
* Legal liability aligns with data ownership

**Country Variation**: Multi-CRVS environments require clear partner designation (Section 3.6).

**Principle 4: One-Time Request Policies**

**Rationale**: Unlimited repeated requests enable gaming, create operational burden, and destabilize downstream systems.

**Application**:

* One fraud flag per UIN until verification completes (Section 4.6.1)
* One reactivation per deactivated UIN (Section 4.6.2)
* One death reversal per person; re-reversals offline only (Section 4.6.3)

**Why One-Time?**:

* Prevents endless activation/deactivation loops
* Forces escalation to higher authority for complex cases
* Maintains downstream service stability
* Signals serious processing (not routine)

**What Happens After?**:

* Further requests rejected
* Citizen/CRVS directed to National ID authority
* Offline processes (courts, ombudsman) handle disputes

**Country Variation**: Offline escalation channels vary by legal system and governance model.

**Principle 5: Offline Escalation Channels**

**Rationale**: Not every scenario belongs in automated integration. Some require judicial review, legal interpretation, or high-level policy decisions.

**Application**:

* Requests outside time windows → Offline (Sections 4.6.1, 4.6.3)
* Repeated reversals → Offline (Section 4.6.3)
* Disputed identity status → Offline
* Complex fraud investigations → Offline

**Why Preserve Offline?**:

* Some decisions require judicial authority
* Edge cases need human judgment
* Legal precedent established through courts
* Automation increases risk in rare scenarios

**Country Variation**: Offline channels include courts, ombudsman offices, National ID appeals boards, ministerial review—varies by country.

### How to Read This Document

Different readers have different needs. This guide is structured to support multiple pathways:

**For Solution Architects**:

* Start here (Section 1.5) for philosophical foundation
* Focus on system boundaries and trust models (Sections 2, 3)
* Understand technical architecture (Section 5)
* Review policy configuration options (Section 10)

**For Developers**:

* Understand "why" (this section) before implementing "how"
* Follow integration workflows (Section 4)
* Implement API specifications (Section 6)
* Reference code examples (Section 13)

**For Policy Makers & Government Officials**:

* Read integration value proposition (Section 1.2)
* Understand principles and real-world consequences (this section)
* Focus on rare scenarios requiring manual verification (Section 4.6)
* Review operational considerations (Section 11)

**For System Integrators**:

* Understand full context (Sections 1, 2, 3)
* Implement standard workflows (Sections 4.2, 4.3, 4.4)
* Configure for country context (Sections 10, 11)
* Test thoroughly (Section 12)

**For CRVS Implementers**:

* Understand MOSIP's role vs. CRVS's role (Sections 1.5.1, 3)
* Review supported use cases (Section 2.3)
* Understand limitations (Section 2.4)
* Plan for edge cases (Section 4.6)

**Key Principle**: The boundaries documented in this guide are not arbitrary technical constraints. They reflect:

* Real-world consequences for individuals
* Legal and ethical considerations
* Operational wisdom from foundational ID implementations globally
* Country-specific variation while maintaining core safety principles

When you encounter a limitation or boundary in subsequent sections, refer back to this section to understand its protective intent.

***

## System Roles & Responsibilities

| **Area**                  | **Responsibility**                                                  |
| ------------------------- | ------------------------------------------------------------------- |
| **Data Collection**       | Capture vital event data from informants/applicants                 |
| **Data Validation**       | Validate completeness and accuracy before sending to MOSIP          |
| **Deduplication**         | Prevent duplicate registrations for same vital event                |
| **Authentication**        | Integrate eSignet for informant/introducer authentication           |
| **API Integration**       | Call MOSIP APIs (Create Packet, Trigger) with proper authentication |
| **Notification Handling** | Subscribe to WebSub and process credential/status notifications     |
| **Certificate Issuance**  | Generate and issue birth/death certificates                         |
| **Error Management**      | Handle API errors and retry failed requests                         |
| **Audit Trail**           | Maintain logs of all MOSIP interactions                             |
| **Data Security**         | Secure transmission and storage of identity data                    |

## MOSIP Platform Responsibilities

| **Area**                    | **Responsibility**                                       |
| --------------------------- | -------------------------------------------------------- |
| **Packet Validation**       | Validate structure, schema, and mandatory fields         |
| **Identity Processing**     | Process registration packets through workflows           |
| **UIN Generation**          | Generate unique identifiers for new births               |
| **Status Management**       | Update deceased status for death registrations           |
| **Credential Generation**   | Create UIN/VID/PSUT tokens                               |
| **Notification Publishing** | Publish events to WebSub for credential/status updates   |
| **Authentication**          | Validate eSignet tokens for audit purposes               |
| **Technical Validation**    | Enforce schema compliance and data integrity             |
| **Error Reporting**         | Send validation errors via WebSub notifications          |
| **Partner Management**      | Manage CRVS as trusted partner with appropriate policies |

## Shared Responsibilities

| **Area**           | **CRVS Contribution**               | **MOSIP Contribution**           |
| ------------------ | ----------------------------------- | -------------------------------- |
| **Field Mapping**  | Define CRVS field names and formats | Define ID schema structure       |
| **Error Handling** | Implement retry logic               | Provide detailed error codes     |
| **Security**       | Secure OAuth2 client credentials    | Issue and validate access tokens |
| **Monitoring**     | Monitor API response times          | Monitor packet processing times  |
| **Reconciliation** | Track pending requests              | Publish status updates           |
| **Testing**        | Provide test data and scenarios     | Provide sandbox environment      |

## Role-Based Access Control

**CRVS Operator:**

* Access CRVS portal
* Capture vital event data
* Initiate authentication flows
* Cannot directly access MOSIP APIs

**CRVS System (OAuth2 Client):**

* Call Packet Manager API
* Call Trigger API
* Receive WebSub notifications
* Cannot access admin functions

**MOSIP Administrator:**

* Configure partner policies
* Create centres/machines/officers
* Monitor integration health
* Cannot access CRVS data directly

**System Integrator:**

* Configure OAuth2 clients
* Set up WebSub subscriptions
* Map fields between systems
* Test end-to-end flows

***

### Learn More

* [Core Integration Principles](core-integration-principles.md)
* [Integration Boundaries, Limitations and Implications](integration-principles-boundaries-and-real-world-implications.md)


-->