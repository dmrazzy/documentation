# Rare Scenarios

### What does 'Rare-Scenarios' include and mean

Rare Scenarios describe how a CRVS and MOSIP systems drives following flows:

* **Fraudulent Birth Registration - Deactivation**
* **Reactivation of National ID**
* **Reversal of Deceased Flag (Fraud Death or Mistaken Death Entry)**



### Handling Rare Scenarios in MOSIP–CRVS Integration

#### Background

MOSIP already supports standard integration workflows with Civil Registration and Vital Statistics (CRVS) systems for birth registration and death registration. These are well-established, high-volume processes that operate smoothly in production environments.

During implementation, CRVS has requested MOSIP to extend the integration to a set of rare scenarios. These scenarios are infrequent, but they have significant legal, operational, or fraud-related implications. They impact the National ID lifecycle, require special handling, and cannot be treated as routine CRVS events.

> **Important**: The manual verification approach for rare scenarios reflects the risk-based integration design principles outlined in Section 1.5. These scenarios carry high potential for fraud, legal disputes, and service denial if processed automatically.

To address these cases safely and consistently, MOSIP must ensure:

* **Traceability**: Every incoming request is logged with clear metadata such as process, source, timestamps, and reason codes.
* **Controlled Processing**: No direct automatic changes to National ID status; sensitive requests must be routed for internal or country-level manual verification.
* **Policy Alignment**: MOSIP acts only as the foundational ID platform; final decisions rest with authorized country authorities.
* **Auditability**: All requests should leave a reliable audit trail for legal, compliance, and investigative needs.

This document outlines the three rare scenarios that the MOSIP-CRVS integration must support and provides the detailed workflow for each in the following sections.

#### Scenario Overview

**1. Fraudulent Birth Registration - Deactivation**

CRVS may later detect that a submitted birth registration was fraudulent, incorrect, duplicated, or otherwise invalid. In such cases, CRVS needs a mechanism to notify MOSIP so that the corresponding National ID (UIN) can be flagged and routed for manual verification.

This scenario is particularly sensitive because:

* Infants do not yet have biometrics in MOSIP.
* Fraudulent birth certificates are a known global risk for identity systems.
* Direct ID deactivation without human review can cause wrongful service denial and legal disputes.

**2. Reactivation of National ID**

CRVS may request MOSIP to deactivate a National ID—for example, because the identity was found invalid, duplicated, or incorrectly issued after verification. In rare cases, CRVS may later request reactivation if it determines that the earlier deactivation was incorrect.

This scenario is sensitive because:

* National ID activation status impacts all downstream services.
* Repeated activation/deactivation can destabilize integrations with banking, benefits, and authentication services.
* Errors in these actions carry legal and operational consequences.

**3. Reversal of Deceased Flag (Fraud Death or Mistaken Death Entry)**

CRVS may detect that a person previously reported as deceased is actually alive. This may occur due to reporting errors, communication delays, mistaken identity, or rare cases in conflict or disaster contexts.

This scenario is highly sensitive because:

* Deceased flags directly affect pensions, insurance, property transfers, and legal identity status.
* Incorrect deceased tagging can lead to complete service denial.
* Reversing this flag must be handled with caution and full traceability.



***

### Learn More

* [Core Integration Principles](../../integration-overview-and-context/core-integration-principles.md)
* [Integration Boundaries, Limitations and Implications](../../integration-overview-and-context/integration-principles-boundaries-and-real-world-implications.md)





***
