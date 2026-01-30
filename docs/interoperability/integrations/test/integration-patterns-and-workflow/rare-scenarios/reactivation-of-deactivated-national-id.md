# Reactivation of Deactivated National ID

#### When Does It Happen?

This scenario occurs in rare cases when a National ID has been deactivated following a fraud-birth verification (see Section 4.6.1), but CRVS or country authorities later determine that the original deactivation decision was incorrect or unjustified.

**Triggers**:

* Discovery that the fraud claim was erroneous
* New evidence proving the birth registration was legitimate
* Administrative error correction after review
* Legal reversal of the deactivation decision

**Why This Scenario is Sensitive**:

* Original deactivation involved thorough verification by country authorities
* Automatic reactivation could bypass legal and administrative safeguards
* Multiple reactivation/deactivation cycles could compromise system integrity
* Downstream services (banking, benefits, authentication) may be affected
* Risk of operational confusion or misuse

> **For detailed context on real-world consequences and design principles, see Section 1.5.4 (Real-World Consequences Framework)**.

#### What Does MOSIP Do?

MOSIP receives the reactivation request from CRVS, validates that the National ID was previously deactivated with a fraud flag, and routes the case to manual verification. MOSIP does **NOT** automatically reactivate the National ID. Instead, it:

1. **Validates the request** by confirming the UIN exists and was deactivated with `Fraud_Birth = True`
2. **Checks time windows** to ensure the request falls within policy-defined limits
3. **Prevents duplicate requests** by rejecting if a reactivation is already under review
4. **Routes to manual verification** using the same Camel workflow as deactivation
5. **Provides context** (original fraud reason, supporting documentation) to reviewers
6. **Waits for manual decision** from authorized country authorities
7. **Executes reactivation** only after manual approval

#### What Does CRVS Receive?

Under the current implementation:

* **No automatic notification** is sent back to CRVS regarding reactivation status
* CRVS may subscribe to WebSub for packet status updates if needed
* Final reactivation status may be communicated through offline coordination

> **Note**: The notification mechanism for reactivation scenarios is under policy review and may be enhanced based on country requirements.

#### What Is the Workflow?

**Step 1: CRVS Request Submission**

CRVS submits a reactivation request via the MOSIP packet API with the following parameters:

**Required Fields:**

* **Fraud\_birth** = `False` (indicates reactivation request)
* **Process** = `CRVS_fraud_birth`
* **Source** = `CRVS1`
* **National\_ID** (UIN of the individual)
* **Fraud\_birth\_reason** (reason for the original deactivation)
* **Optional**: Supporting metadata/document references

> **Note**: Should evidence documentation be required as reference for manual review?

**Step 2: Validation by MOSIP**

1. **Confirm UIN status**: Verify that the UIN exists and was deactivated with `Fraud_birth = True`.
2. **Reject duplicate requests**: If a previous reactivation request for the same UIN is already under review, reject the new request.
3.  **Time window validation**: Requests are valid only within a defined period from the initial birth registration date. Requests outside this window are automatically rejected, requiring citizens to follow offline grievance/legal processes.

    > **Note**: Should the time window be calculated from the date of initial registration or from the date of deactivation?
4. **Optional validation**: Should MOSIP-side validation for garbage values in the `fraud_birth_reason`/`Deactivation_reason` field be added?
5. If all validations pass, initiate packet processing.

**Step 3: Routing to Manual Verification**

1. Route the packet to a dedicated manual verification queue using the Camel route associated with `CRVS_fraud_birth`.
2. Include the `Fraud_birth_reason` to provide context for the country authorities' review.

> **Note**: Where should the packet details be stored for reference during manual verification?

**Step 4: Manual Verification by Authorities**

Country administrators/legal authorities review the case, using the original deactivation reason and supporting documentation.

**Decision Options:**

* **Approve**: Reactivate the National ID offline.
* **Reject**: Keep the National ID deactivated. The fraud flag may be retained or cleared depending on policy.

**Step 5: Logging and Audit**

1. Store all request, validation, and verification details in MOSIP for compliance and traceability.
2. Ensure traceability of all actions for potential audits or legal review.

***

#### MOSIP Options and Pros/Cons

| **Option**                                                       | **Advantages**                                                                            | **Disadvantages / Risks**                                                                                              |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Automatic Online Reactivation**                                | Fast; reduces manual overhead                                                             | High risk of fraud; bypasses legal review; multiple toggling possible; downstream service disruption; audit challenges |
| **Receive Request + Route to Manual Verification (Recommended)** | Preserves legal and operational control; ensures traceability; allows genuine corrections | Requires administrative effort; slight delay in final resolution                                                       |
| **Reject All Online Reactivation Requests (Offline Only)**       | Fully prevents misuse; simple to implement                                                | Citizens and CRVS cannot leverage integration; delays in legitimate corrections; reduced transparency                  |

***

#### Recommended MOSIP Policy

1. **Accept CRVS reactivation requests only if the UIN was previously deactivated** with `Fraud_birth = True`.
2. Use `Fraud_birth_reason` to reference the original deactivation during manual verification.
3. **Route flagged requests to the manual verification queue** using the dedicated Camel route (`CRVS_fraud_birth`).
4. **Enforce one active request per UIN**: No subsequent requests are accepted until the verification is complete.
5. **Validate time window**: Requests must fall within the defined time window from the initial birth registration date. Requests outside this window are rejected, and offline grievance/legal channels must be followed.
6. **No automatic reactivation is allowed**; final decisions rest with country authorities.

***

#### Additional Considerations

**Traceability**

Maintaining metadata (**Process**, **Source**, and **Fraud\_birth\_reason**) ensures every reactivation request is auditable.

**Operational Safety**

Limiting reactivation to manual verification avoids misuse and prevents disruption to downstream authentication services.

**Time Window Logic**

The window for reactivation should be calculated from the initial birth registration date, consistent with the original deactivation policy.

**Alignment with Global Best Practices**

This workflow mirrors international practices, combining civil registration with identity management while reserving sensitive lifecycle events for human review.

***

## Learn More

* [Fraudulent Birth Registrations - National ID Deactivation Request from CRVS](fraudulent-birth-registrations-national-id-deactivation-request-from-crvs.md) - Understand the deactivation workflow that precedes reactivation. This document explains how National IDs are originally deactivated for fraud cases, providing essential context for the reactivation process.
* [Manual Adjudication and Verification](../../../../../manual-adjudication-and-verification.md) - Learn about MOSIP's manual verification system used for reviewing reactivation requests. This details the queue-based architecture, decision workflows, and how country authorities approve or reject reactivation after reviewing evidence and fraud history.
* [Birth Registration & UIN Issuance](../integration-flows/birth-registration-and-uin-issuance.md) - Explore the standard birth registration workflow that establishes National IDs initially. Understanding this baseline process helps contextualize why reactivation after fraud-based deactivation requires such careful handling.
* [Registration Processor Configurations Details](../../configurations-and-operations/configurations-details.md) - Discover the technical configurations including Camel routes (`CRVS_fraud_birth`) and packet processing workflows that enable both deactivation and reactivation requests to be routed correctly for manual review.
