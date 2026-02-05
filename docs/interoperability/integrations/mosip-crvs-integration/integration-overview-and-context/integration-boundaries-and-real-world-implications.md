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

MOSIP requires accountability for every request that affects the identity lifecycle. Online authentication using [eSignet](https://docs.esignet.io/#overview) ensures that the identity of the person reporting an event - or the legal authority approving it - is known, verifiable, and auditable. The eSignet userinfo token is shared with MOSIP to establish this accountability.

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

{% hint style="info" %}
Note: Manual verification stage is controlled review step in the MOSIP, where sensitive requests are examined by authorized country officials before any change is applied to a National ID. Pleas [refer here](../../../../id-lifecycle-management/identity-issuance/registration-processor/test/manual-adjudication-and-verification.md) for more details.
{% endhint %}

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

#### 8. No Automatic Failure Acknowledgement to CRVS by Default

The default MOSIP–CRVS integration does not provide automatic failure notifications to CRVS when a request fails during MOSIP processing. In such cases, failure notifications are sent only to the registered phone number or email address of the resident or informant, who must then revisit CRVS to re-initiate the request.

**Rationale**\
When a request fails, corrective action can only be taken by the resident or informant by providing updated or corrected information through CRVS. Notifying CRVS alone does not result in any actionable outcome unless the resident re-engages with the process. To keep the integration focused and operationally effective, failure handling is designed around informing the individual who can act on it. Support for failure notifications to CRVS can be added if required, but it is not part of the default integration.

#### Final Note on Technical Capability vs. Policy Choice

All the above limitations are **policy-driven defaults**, not technical constraints. From a purely technical standpoint, MOSIP can be configured to support these actions automatically. However, countries must fully understand the **legal, operational, social, and downstream consequences** before enabling such capabilities.

These boundaries exist to protect individuals, preserve system integrity, and ensure that foundational identity remains stable, trustworthy, and governed by accountable human oversight where risks are high.

***
