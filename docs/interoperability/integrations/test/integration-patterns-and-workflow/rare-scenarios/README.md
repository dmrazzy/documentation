# Exceptional Scenarios

## What does '**Exceptional Scenarios**' mean

In addition to the standard CRVS–MOSIP integration workflows—such as birth registration for infants, death registration to mark a MOSIP ID as deceased, and demographic correction requests—which are well-established, high-volume processes operating smoothly in production environments, there may be exceptional situations that arise outside normal day-to-day operations.

These scenarios typically occur due to circumstances such as:

* Incorrect or incomplete verification during initial registration
* Submission of incorrect or fraudulent documents
* Intentional misuse or misreporting by individuals
* Discovery of new facts during re-verification by the Civil Registration Authority (CRVS)

Such situations may impact existing birth or death registration outcomes. For example, a birth registration may later be identified by CRVS as having been registered incorrectly, or an individual marked as deceased may subsequently be found to be alive.

As the legally authorized authority for civil registration, **CRVS communicates such findings to MOSIP**. These cases cannot be handled through the standard integration workflows and therefore require **special handling with additional safeguards**. As part of the default CRVS–MOSIP integration, MOSIP processes these scenarios with caution, and the **recommended handling approach is defined in the following sections**.

### Types of Exceptional Scenarios

The following exceptional scenarios are supported through recommended workflows:

1. **Incorrect Birth Registration**\
   A request from CRVS to deactivate a MOSIP ID when an infant’s birth registration is found to be incorrect or invalid.
2. **Reversal of Death Declaration**\
   A request from CRVS to reverse the deceased status when an individual previously marked as deceased is later confirmed to be alive.
3. **Reactivation of a Deactivated MOSIP ID**\
   A request from CRVS to reactivate a previously deactivated MOSIP ID for an infant, based on verified corrective information.

{% hint style="info" %}
**Note:** By default, the CRVS–MOSIP integration supports **birth registration only for infants**. Accordingly, the deactivation and reactivation workflows described in this section are **applicable only to infant MOSIP IDs**. Please [refer here](../../integration-overview-and-context/integration-principles-boundaries-and-real-world-implications.md#id-1.-adult-registration-via-crvs-is-not-supported-by-default) for mode details.

For death registration, since the process involves updating a status flag on an existing MOSIP ID, exceptional handling is limited to **reversal of the deceased flag** in cases where CRVS confirms that the individual is alive.
{% endhint %}

## Scenario Overview

#### **1. Incorrect Birth Registration**

CRVS may subsequently identify that a birth registration submitted earlier was incorrect, fraudulent, duplicated, or otherwise invalid. In such cases, CRVS must notify MOSIP so that the corresponding MOSIP ID (UIN) can be flagged and routed for manual verification.

This scenario requires special handling because:

* Infants do not yet have biometrics linked to their MOSIP ID.
* Fraudulent or erroneous birth registrations are a well-recognized risk in foundational identity systems.
* Automated or immediate deactivation without human review can lead to wrongful denial of services and potential legal or administrative disputes.

#### **2. Reversal of Death Declaration**

CRVS may later determine that an individual previously reported as deceased is, in fact, alive. This can occur due to reporting or data entry errors, communication delays, mistaken identity, or exceptional circumstances such as conflict, displacement, or disaster situations.

This scenario is highly sensitive because:

* A deceased status directly impacts pensions, insurance claims, property transfers, and an individual’s legal identity.
* An incorrect deceased flag can result in complete denial of essential services.
* Reversal of a death declaration must be performed with extreme caution, supported by manual verification and full audit traceability to prevent misuse and ensure legal accountability.

#### **3. Reactivation of a Deactivated MOSIP ID**

CRVS may request MOSIP to deactivate a MOSIP ID when an identity is later found to be invalid, duplicated, or incorrectly issued based on post-registration verification. In rare cases, CRVS may subsequently determine that the deactivation itself was incorrect and request the reactivation of the MOSIP ID.

This scenario requires careful handling because:

* The activation status of a MOSIP ID directly impacts access to all downstream services.
* Repeated activation and deactivation can destabilize integrations with banking, benefits, healthcare, and authentication systems.
* Errors in these actions can result in significant legal, operational, and reputational consequences.

This document outlines three rare scenarios that the MOSIP — CRVS integration can support and provides detailed workflows for each in the following sections.

### Learn More

* [Core Integration Principles](../../integration-overview-and-context/core-integration-principles.md)
* [Integration Boundaries, Limitations and Implications](../../integration-overview-and-context/integration-principles-boundaries-and-real-world-implications.md)





***
