# Fraudulent Birth Registrations - National ID Deactivation Request from CRVS

#### When Does It Happen?

This rare scenario occurs when a civil registration authority (CRVS) determines that a previously recorded birth registration was fraudulent, incorrect, duplicated, or otherwise invalid. CRVS detects this issue post-registration and needs to notify MOSIP to flag the corresponding National ID (UIN) for review and potential deactivation.

**Triggers**:

* Discovery of fraudulent birth certificate issuance
* Detection of duplicate birth registrations for the same infant
* Identification of data integrity issues in initial registration
* Legal determination of registration invalidity

**Why This Scenario is Sensitive**:

* Infants do not yet have biometric authentication in MOSIP
* Fraudulent birth certificates are a known global identity fraud risk
* Direct ID deactivation without human review can cause wrongful service denial
* Legal and ethical implications require careful handling

> **For detailed context on real-world consequences and design principles, see Section 1.5.4 (Real-World Consequences Framework)**.

#### What Does MOSIP Do?

MOSIP receives the fraud notification from CRVS, validates the request against policy rules (time window, duplicate checks), and routes the case to a manual verification queue. MOSIP does **NOT** automatically deactivate the National ID. Instead, it:

1. **Validates the request** against configured time windows and duplicate prevention rules
2. **Sets a fraud flag** (`Fraud_Birth = True`) on the identity record
3. **Routes the packet** to manual verification using a dedicated Camel workflow
4. **Stores metadata** (process, source, reason) for audit and traceability
5. **Waits for manual decision** from authorized country authorities
6. **Executes deactivation** only after manual approval

#### What Does CRVS Receive?

Under the current implementation:

* **No automatic notification** is sent back to CRVS regarding packet status
* CRVS must subscribe to WebSub packet status updates if tracking is required
* Final deactivation status may be communicated through offline channels depending on country policy

> **Note**: Notification strategy for fraud scenarios is under consideration and may be enhanced in future implementations.

#### What Is the Workflow?

**Step 1: CRVS Submits a Fraudulent Birth Request**

CRVS submits a "fraudulent birth" request to MOSIP, including:

**Required Fields:**

* **National ID (UIN)** or **packet reference**
* **fraud\_birth / deactivate\_id** = `True` (property name to be finalized based on country requirements)
* **process** = `CRVS_fraud_birth` / `CRVS_deactivate_ID`
* **source** = `CRVS1`
* **fraud\_birth\_reason / Deactivation\_reason** (string describing the reason for deactivation)
* **date\_of\_initial\_registration**
* **Optional**: Supporting metadata/document references

> **Note**: These fields must be added to the ID schema to support this workflow.

**Step 2: MOSIP Validates the Request Window**

1. **Compare the submitted date of initial registration to the current date.**
2. **If within a configurable window** (e.g., 30–60 days as defined in country policy):
   * MOSIP accepts the request and initiates packet processing.
   * A new tag is introduced to categorize such requests: `CRVS_deactivate_ID`
   * This tag helps in anonymous profiling and tracking.
3. **If outside the time window**:
   * The request is automatically rejected.
   * Citizens must use offline grievance or legal channels.

> **Note**: MOSIP-side validation for garbage values in the `fraud_birth_reason` / `Deactivation_reason` field is **not required**, as notifications to CRVS are not sent in this workflow.

**Step 3: Route the Packet to Manual Verification**

1. **Basic de-duplication** is performed for any incoming packet in MOSIP.
2. Based on the **process** and **source** values, a specific **Camel route** is triggered to route the packet directly to the **manual verification stage**.
3. The following information is provided to the manual reviewer:
   * `fraud_birth_reason` / `Deactivation_reason`
   * Any supporting documents (if evidence collection is supported)
   * Details of the original packet
4. **Set attribute**: `fraud_Birth = True` against the National ID.
   * **No automatic deactivation** occurs at this stage.
5. **Packet details storage**:
   * Stored in **Packet Manager** and **Transaction Table** for reference.

**Step 4: Enforce One-Time Request Policy**

1. If a `Fraud_Birth = True` flag already exists, any subsequent request on the same UIN is **automatically rejected** until verification completes.
2. This prevents duplicate or repeated requests from being processed.

> **Note**: If someone authenticates using the National ID during the manual verification flow, no special flag is added to the KYC info shared with the Relying Party (RP).

**Step 5: Manual Verification by Country Authorities**

1. Responsible admins/legal authorities review:
   * Supporting documentation
   * Original packet details
   * `fraud_birth_reason` / `Deactivation_reason`
2. Authorities decide whether to:
   * **Approve deactivation**, or
   * **Reject the fraud claim**

**Step 6: Final Action**

**If Deactivation is Approved:**

1. The National ID is deactivated **offline** by authorities.
2. MOSIP must add logic to **deactivate the UIN** once the manual verifier approves.
3. **Manual Steps**: Countries can use the existing **Deactivate UIN (MOSIP ID) API** (not recommended for routine use).

**If Rejected:**

1. The packet is rejected, and the flow is completed.
2. No changes are made to the record in the ID system.

#### Use Case-Based Notifications

> **Note**: This section is under consideration for MOSIP-side changes. Final notification strategy is to be determined based on country requirements.

***

#### Pros & Cons of MOSIP Options

| **Option**                                          | **Advantages**                                                                                  | **Disadvantages / Risks**                                                                                                                      |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| **Automatic Online Deactivation**                   | Fast, immediate response to CRVS fraud detection                                                | High risk of misuse or wrongful deactivation; no biometric authentication for infants; downstream system disruption; legal + ethical liability |
| **Flag + Manual Verification (Recommended)**        | Ensures traceability; leverages human/legal judgement; safe for infants; audit trail maintained | Requires manual workload; may delay final resolution; depends on country administrative capacity                                               |
| **Reject All Online Fraud Requests (Offline Only)** | Simplest to implement; full control remains with authorities                                    | Loses benefit of integration; CRVS cannot leverage MOSIP automation; increased overhead and delays; reduced transparency                       |

#### Recommended MOSIP Policy

1. **Accept CRVS fraud-birth requests only if submitted within 30–60 days of initial registration.**
2. On valid request, set **`Fraud_Birth = True`** in schema with metadata:
   * **Process** = `CRVS_Fraud_Birth`
   * **Source** = `CRVS1`
3. **Route flagged requests to manual verification queue** using a dedicated Camel route.
4. **Enforce one-time request per UIN** until verification completes; block duplicate requests.
5. **No online deactivation**; final deactivation decision rests with national/regional authorities after their offline review.
6. **Requests outside the time window are automatically rejected**; citizens must use offline grievance or legal channels.

#### Additional Considerations & Notes

**Infants / No Biometric Enrollment**

* Without biometric verification, automated deactivation carries high risk.
* The **flag + manual review** approach maintains safety and due process.

**Audit & Traceability**

* Maintaining metadata (**Process**, **Source**) ensures every request is logged and traceable.
* This is critical if disputes or legal challenges arise later.

**Alignment with Global Best Practices**

* The recommended workflow mirrors practices observed in other foundational ID systems combining civil registration with identity issuance.
* Sensitive lifecycle events (fraud, death, deactivation) are reserved for manual review.

**Policy & Country-Specific Customization**

* The **30–60 day window** and verification procedures should be configurable per country's legal and governance environment.

**Infant Death Cases**

* Cases of infant death will be handled through a **death registration request** submitted by CRVS.

***

## Learn More

* [Birth Registration & UIN Issuance](../integration-flows/birth-registration-and-uin-issuance.md) - Understand the standard birth registration workflow in CRVS-MOSIP integration. This provides essential context for how National IDs are initially issued to infants, which is the baseline process before fraud detection triggers deactivation requests.
* [Manual Adjudication and Verification](../../../../../manual-adjudication-and-verification.md) - Learn about MOSIP's manual verification system that reviews fraudulent birth deactivation requests. This document details how packets are routed to manual verification queues, the decision-making workflow, and integration with external verification systems.
* [Reactivation of Deactivated National ID](reactivation-of-deactivated-national-id.md) - Explore the reverse scenario where a deactivated National ID needs to be reactivated (e.g., after wrongful deactivation). This complements the deactivation workflow and shows the complete lifecycle management of rare fraud cases.
* [Registration Processor Configurations Details](../../configurations-and-operations/configurations-details.md) - Discover the Camel route configurations (`CRVS_fraud_birth`) and packet processing workflows that enable fraud detection handling. This technical reference explains how deactivation requests are routed and processed within MOSIP's registration processor.
