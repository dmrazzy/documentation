# CRVS ↔ MOSIP Integration Guide — Information Architecture

This file defines a clear structure to author CRVS ↔ Foundational ID (MOSIP) integration documentation with minimal ambiguity.

## Recommended Sections
- Purpose & Audience
- Scope & Out-of-Scope
- System Roles & Boundaries

- Core Principles & Assumptions
- Policy & Configuration

- End-to-End Flows (Birth, Death, Demographic Updates, Other flows - Confluence)
  - Data Model & Mappings
  - APIs & Packet Construction
  - Security & Trust
  - Notifications & Consumption
  - Error Handling & Reconciliation

- Operational Considerations (Monitoring, Idempotency, Versioning)

- Testing & Sandbox
- References to External Frameworks
- Terminology & Glossary





## Section Content Guidance
- **Purpose & Audience:** Who this is for (CRVS implementers, MOSIP integrators) and the decisions it enables.
- **Scope & Out-of-Scope:** Supported flows; explicitly list what is not default (e.g., back-to-CRVS notifications).
- **Core Principles & Assumptions:** CRVS as source of truth; no de-duplication in MOSIP for CRVS-origin requests; UIN stays active for deceased (flag only).
- **System Roles & Boundaries:** CRVS as external source (parallel to registration client) invoking MOSIP Create Packet; MOSIP services involved (Packet Manager/Workflow, Identity, Resident Services, Kernel Master Data).
- **Terminology & Glossary:** UIN, eSignet, provenance fields (`source`, `process`), WebSub (optional notifications).
- **End-to-End Flows:** Stepwise flows for Birth, Death, Updates with `source=CRVS` and `process=CRVS_BIRTH|CRVS_DEATH|CRVS_UPDATE`.
- **Data Model & Mappings:** Required identifiers, event timestamps, flags (e.g., `deceased=true`), and provenance.
- **APIs & Packet Construction:** Create Packet schema, required headers, workflow trigger, and expected stages.
- **Security & Trust:** OAuth2 for service-to-service; eSignet for parent/actor auth; auditing and traceability.
- **Notifications & Consumption:** Default resident notification; optional WebSub to CRVS is opt-in.
- **Error Handling & Reconciliation:** Deterministic errors, retry with idempotent IDs (`externalId`, `packetId`), reconciliation via logs/events.
- **Operational Considerations:** Monitoring workflow health, idempotency guarantees, schema versioning/backward compatibility.
- **Policy & Configuration:** Country-configurable biometric age threshold; introducer/informant rules; permitted update fields; retention.
- **Testing & Sandbox:** Test data, staging flows, validation of packet creation and workflow trigger.
- **References:** ID4D, OpenCRVS interoperability, AU Interoperability Framework, UNICEF legal identity guidance.

## Draft Skeleton (Starter Template)

```markdown
# CRVS ↔ MOSIP Integration Guide

## 1. Purpose & Audience
- For CRVS implementers and MOSIP integrators; enables birth, death, and update flows with clear responsibilities.

## 2. Scope & Out-of-Scope
- In scope: Birth → UIN issuance; Death → `deceased=true`; CRVS-driven demographic updates.
- Out of scope: UIN deactivation by default; MOSIP-side de-duplication on CRVS-origin requests.

## 3. Core Principles & Assumptions
- CRVS is authoritative; MOSIP trusts CRVS inputs with minimal syntactic validation.
- UIN remains active for deceased; relying parties consume `deceased=true`.
- Country policy defines biometric thresholds and introducer/informant usage.

## 4. System Roles & Boundaries
- CRVS acts as external source, calling MOSIP Create Packet API.
- MOSIP components: Packet Manager/Workflow, Identity Service, Resident Services, Kernel Master Data.

## 5. Terminology & Glossary
- UIN, eSignet, `source`, `process`, WebSub.

## 6. End-to-End Flows
### Birth
1. Parent identity/auth via eSignet.
2. Collect child details in CRVS.
3. Create packet: `source=CRVS`, `process=CRVS_BIRTH`, idempotent `externalId/packetId`.
4. Trigger MOSIP workflow → UIN issued → resident notification.

### Death
1. Submit existing UIN and death event data.
2. Create packet: `process=CRVS_DEATH`.
3. Workflow sets `deceased=true`; UIN remains active.
4. Relying parties read flag via userinfo.

### Demographic Update
1. Authenticated CRVS request, permitted fields per policy.
2. Create packet: `process=CRVS_UPDATE`.
3. Audit trail and provenance retained.

## 7. Data Model & Mappings
- Birth: child demographics, birth registration no., parent IDs, timestamps.
- Death: UIN, certificate identifiers, timestamps.
- Provenance: `source`, `process`, `externalId`, `packetId`.

## 8. APIs & Packet Construction
- Create Packet API usage and schema.
- Workflow trigger endpoint; expected stages; dedup bypass per policy.

## 9. Security & Trust
- OAuth2 for CRVS→MOSIP; eSignet for parent/actor.
- Full audit and traceability.

## 10. Notifications & Consumption
- Default: resident notification.
- Optional: WebSub topic back to CRVS (opt-in).

## 11. Error Handling & Reconciliation
- Deterministic errors; retries with idempotent IDs.
- Reconciliation via logs/events.

## 12. Operational Considerations
- Monitoring, idempotency, schema versioning.

## 13. Policy & Configuration
- Biometric age threshold; introducer/informant; permitted update fields; retention.

## 14. Testing & Sandbox
- Test data, staging flows, validation steps.

## 15. References
- ID4D, OpenCRVS, AU Interoperability, UNICEF.
```

Use this file to drive consistent, unambiguous authoring across MOSIP-CRVS integration content.