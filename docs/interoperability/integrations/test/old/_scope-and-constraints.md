# MOSIP–CRVS: Scope and Constraints

## Prompt

### 1
I need more on 'What is not in scope', I mean the ideal practice that should be allowed as scope and what should not be allowed as scope when integrating a CRVS syatem to that with Foundational ID system
Such as Updating an adult's demographic data should not be allowed as practice by a Foundational Id System bacause it could be linked with so many other implementation such as Passport or PAN etc
In such a way can you drfat it in a lucid, clear and detailed way?

### 2
Now that you added our last interaction as scope and constrain file, I will take it further as below:
I need to draft the rationale behind the constraints and boundries and why MOSIP advocates it as practice to follow
For example behind all the constraints and boundries MOSIP reasons that while ven being technically feasible practices like updating an adults demographic data will have ripple effect which even the seeker is not aware of such as Passport, PAN etc, for example if some one wants the Name change or DOB change and if that is allowed it will conflict afterwards which the seeker will not want himself
Such are the rationale behind every constrain
Can you now revise this scope-and-constrain.md again and accomodate my request?

-------------------------------------------------------------------

# MOSIP–CRVS: Scope and Constraints


This document defines what is in scope and not in scope when integrating a Civil Registration and Vital Statistics (CRVS) system with the MOSIP Foundational ID system. It aims to remove ambiguity and establish clear, default behaviors, configuration points, and explicit exclusions.

## What This Covers
- Principles and defaults for birth, death, and limited demographic updates
- Authentication and schema requirements
- Country-configurable policy levers
- Explicit out-of-scope practices and why they’re disallowed
- Rare scenarios that require manual verification

## In Scope (Allowed by Default)
- Birth → UIN Issuance:
  - CRVS submits infant birth packets; MOSIP creates the UIN.
  - No infant biometrics are required; CRVS is treated as the source of truth.
- Death → Flagging Only:
  - MOSIP sets `deceased=true` and records `deceasedDeclarationDate`.
  - UIN remains active to support downstream services (e.g., insurance, banking).
- Child/No-Biometric Demographic Updates:
  - Limited updates for IDs without biometrics; country-configurable fields.
  - Suggested minimum: name, date of birth (DOB), gender. Countries may add address.
- Single Entry API:
  - All flows use the `create packet` API.
  - Workflow routing is determined by `process` + `source` (e.g., `CRVS_NEW`, `CRVS_DEATH`, `CRVS_UPDATE`).
- Authentication Requirement:
  - The informant/parent (or CRVS registrar in edge cases) must authenticate via eSignet.
  - Include the eSignet user info token in the packet for MOSIP validation.
- Schema Alignment:
  - CRVS must construct packets using MOSIP’s ID schema.
  - Death uses fields such as `declaredAsDeceased` (boolean) and `deceasedDeclarationDate`.
- Notifications (Default Behavior):
  - MOSIP notifies the resident (e.g., parent) on final status (approved/rejected).
  - CRVS does not receive status/failure notifications by default.
- Country Configuration:
  - Infant/minor cutoff is country-configurable (e.g., 0–5) and enforced by MOSIP’s child filter.
  - A fraud-detection window (e.g., 60–90 days) for child deactivation requests can be configured to guide manual verification (informational, not a hard block).

## Not In Scope (Disallowed by Default)
- Adult Demographic Updates via CRVS (for biometric-linked IDs):
  - Adults must update via MOSIP registration with biometric authentication.
  - Rationale: high-assurance changes can impact many relying systems (e.g., passport, PAN/tax, banking, insurance, land registry). CRVS-side updates without biometrics risk inconsistency and fraud.
- New Adult “Birth Registration” for UIN via CRVS:
  - Adults must enroll through MOSIP centers with biometric capture; CRVS cannot initiate UIN creation for adults.
- Automatic UIN Deactivation on Death:
  - MOSIP does not deactivate UINs when a death is registered; it flags `deceased=true`.
  - Rationale: downstream services and families still need the identity reference for legitimate processes.
- Default CRVS Status/Failure Notifications:
  - MOSIP does not send status/failure notifications to CRVS by default. Resident-only notifications are the default.
  - CRVS-facing notifications (e.g., WebSub) are opt-in and require explicit configuration and policy alignment.
- Offline Integration:
  - While data can be collected offline, packet submission and eSignet authentication require online connectivity.
- MOSIP-side De-duplication/Document Validation for CRVS Birth/Death Packets:
  - MOSIP does not perform de-duplication or document validation on CRVS submissions; CRVS is responsible for due diligence.
  - Consequence: duplicates may override data or result in multiple UINs when biometrics are absent.
- Biometric Capture/Transfer via CRVS:
  - CRVS is not a biometric enrollment channel for MOSIP. Any biometric updates must occur within MOSIP registration processes.
- Schema Deviations:
  - Packets must conform to MOSIP’s ID schema (including required eSignet token). Non-conformant packets are out of scope.

## Optional (Opt-in) Features
- CRVS Event Subscriptions:
  - Countries may enable CRVS subscriptions for ID creation/status events (e.g., WebSub). This is not default and must be explicitly configured.
- Country-Specific Policy Choices:
  - Clearly distinguish defaults from configurable options: age thresholds, allowed update fields, notification routing, deactivation window.

## Rare Scenarios (Manual Verification Required)
- Child ID Deactivation (Fraud/Misinformation):
  - CRVS may submit a deactivation request packet with a flag, reason, and optional evidence (e.g., court order, supporting documents).
  - A manual verifier approves/rejects the request. If approved, MOSIP deactivates the UIN.
- Child ID Reactivation:
  - Not supported. Guidance is to submit a new request to issue a new UIN rather than attempting reactivation.
- Adult Death Flag Reversal:
  - If a person was mistakenly marked deceased, the `deceased` flag can be reversed after manual verification.
  - There is no adult deactivation/reactivation concept beyond flag correction.

## Rationale (Expanded)
The constraints and boundaries are intentional. Even when a change looks technically feasible, MOSIP advocates cautious practices to protect assurance, interoperability, and citizens’ service continuity.

- Assurance-first updates:
  - Adult identity changes (e.g., name, DOB, gender, address) require biometric authentication within MOSIP to reach the highest assurance level and prevent impersonation or fraud.
  - CRVS is not a biometric channel; routing adult updates via MOSIP registration centers ensures strong identity proofing.

- Interoperability stability (ripple effects):
  - Foundational IDs are referenced across many relying systems (passport, PAN/tax, banking, insurance, land registry, education, social programs). An ungoverned update via CRVS can silently desynchronize records.
  - Examples:
    - Name change approved via CRVS without biometrics may diverge from passport issuance, creating travel and KYC issues.
    - DOB change affects eligibility thresholds (voting, benefits, pension), tax liabilities, and legal records.
    - Address change impacts service delivery, jurisdiction, and fraud controls in banking or land registry.

- Least disruption to citizens and services:
  - Changes to adult records can unintentionally break access to services if relying systems are not updated coherently. MOSIP’s biometric update flow coordinates the change with proper audit, notifications, and propagation.

- Role separation and governance:
  - CRVS is the authority for vital events (births, deaths). MOSIP governs identity lifecycle, high-assurance updates, and downstream stability.
  - Keeping roles distinct prevents overlapping mandates, duplicated checks, and contradictory outcomes.

- Auditability and accountability:
  - High-assurance updates via MOSIP maintain auditable trails, verifiable consent, and regulator-ready artifacts (who changed what, when, and how).

- Clarity and informed consent:
  - Citizens often underestimate downstream impacts. By routing adult updates via MOSIP, seekers receive clear guidance on consequences and coordinated updates across systems.

### Constraint-to-Rationale Map
- Adult demographic updates via CRVS (disallowed):
  - Rationale: Requires biometric assurance; avoids desynchronization across passport/PAN/banking/insurance/land; ensures auditability and citizen consent.
- New adult UIN via CRVS (disallowed):
  - Rationale: Adults must enroll with biometric capture in MOSIP to establish a trusted identity anchor.
- Automatic UIN deactivation on death (disallowed):
  - Rationale: Families and relying parties still need the identity reference for claims, transfers, and record settlement; `deceased=true` is sufficient.
- Default CRVS notifications (disallowed):
  - Rationale: Reduces noise and misaligned actions; resident is the party who can act. CRVS notifications are opt-in under country policy.
- Offline integration (disallowed):
  - Rationale: Submission and eSignet auth must be online to preserve security, assurance, and freshness of checks.
- MOSIP-side dedup/doc validation for CRVS birth/death (disallowed):
  - Rationale: CRVS is the source of truth for vital events; MOSIP avoids duplicating checks. Without biometrics, dedup is inconclusive and may mislead.
- Biometric capture via CRVS (disallowed):
  - Rationale: Avoids fragmented biometric pipelines; MOSIP keeps a single, governed biometric channel.
- Schema deviations (disallowed):
  - Rationale: Ensures predictable processing, validation, and downstream interoperability; missing eSignet tokens break assurance.

### Impact If Disallowed Practices Are Enabled
- Conflicting records across systems (e.g., different names in passport vs foundational ID), blocked KYC, rejected claims, or legal disputes.
- Increased fraud risk due to lower-assurance updates outside MOSIP’s biometric flows.
- Loss of auditability and unclear accountability for changes.
- Citizen frustration when “quick” changes later disrupt service access.

### Exceptional Approvals (Governance-only)
If a country chooses to allow a disallowed practice, MOSIP recommends a strict governance checklist:
- Policy decision and legal basis documented.
- Risk assessment: ripple effects across major relying systems (passport, tax, banking, land, insurance).
- Assurance plan: compensating controls (e.g., in-person verification, enhanced KYC), audit trails, and fraud monitoring.
- Interoperability plan: downstream propagation and confirmation from affected agencies.
- Citizen communication: clear consent, consequences explained, and remediation path.

## Configuration Checklist (for Countries)
- Age Threshold:
  - Define infant/minor cutoff and configure MOSIP’s child filter accordingly.
- Allowed Update Fields (Child/No-Biometric):
  - Enumerate permitted demographic attributes (e.g., name, DOB, gender; optionally address). Exclude adult/biometric-linked updates.
- Notification Routing:
  - Decide whether CRVS receives events. Default is resident-only notifications.
- Fraud Window (Child Deactivation):
  - Set an informational review window (e.g., 60–90 days) and define evidence requirements.
- Evidence Policy:
  - Specify acceptable documents for deactivation requests (e.g., court orders, investigation reports).
- Process + Source Mapping:
  - Publish the exact `process` values and their workflows for birth, death, and updates (`CRVS_NEW`, `CRVS_DEATH`, `CRVS_UPDATE`).

## Defaults Summary
- CRVS is the source of truth for birth/death submissions; MOSIP does not perform de-duplication or data validation on these packets.
- UIN remains active after death; MOSIP sets `deceased=true`.
- All flows use `create packet` with routing by `process` + `source`.
- eSignet user info token is mandatory for the informant/parent/registrar.
- Resident-only notifications by default; CRVS notifications are opt-in.
- No offline submission/authentication.
