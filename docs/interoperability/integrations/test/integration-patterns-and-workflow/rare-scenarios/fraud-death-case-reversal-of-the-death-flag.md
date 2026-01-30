# Fraud Death Case - Reversal of the Death Flag

#### When Does It Happen?

This rare but critical scenario occurs when CRVS discovers that a person previously registered as deceased is actually alive. These situations are infrequent but carry significant legal, operational, and identity-related implications.

**Triggers**:

* **Administrative errors**: Data-entry mistakes or communication delays in death reporting
* **Conflict or displacement**: Individuals reported missing or presumed dead later reappear
* **Disaster scenarios**: Mistaken identity during emergency response operations
* **Fraud cases**: Intentional misreporting of death for financial or legal gain
* **Delayed communication**: Individual resurfaces after being out of contact

**Why This Scenario is Sensitive**:

* Deceased flags directly affect pensions, insurance, property transfers, and legal identity status
* Incorrect deceased tagging can lead to complete service denial
* Reversing the flag requires extreme caution and full traceability
* May require longer time windows than birth fraud cases (e.g., post-conflict scenarios)
* Only one reversal per individual is permitted online; re-reversals require offline intervention

> **For detailed context on real-world consequences and design principles, see Section 1.5.4 (Real-World Consequences Framework)**.

**Standard Death Registration Context**:

For reference, when CRVS submits a standard death registration (Section 4.3), MOSIP updates:

* `Declared_as_Deceased = Y`
* `Deceased_Declaration_Date` = Date of death declaration
* National ID remains technically active; only the deceased flag is set
* Downstream services may restrict access based on this flag

#### What Does MOSIP Do?

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

#### What Does CRVS Receive?

Under the current implementation:

* **No automatic notification** is sent back to CRVS regarding reversal status
* CRVS may subscribe to WebSub for packet status updates if required
* Final reversal outcome may be communicated through offline channels per country policy

> **Note**: Acknowledgement to CRVS in case of rejection is under consideration for future enhancement.

#### What Is the Workflow?

**Step 1: CRVS Submits Death Reversal Request**

When CRVS determines that an individual marked as deceased is alive, they submit a reversal request with:

**Required Fields:**

* **UIN/VID**: National ID of the individual
* **Declared\_as\_Deceased**: `N` (indicating reversal)
* **Process**: `CRVS_Fraud_Death`
* **Source**: `CRVS1`
* **Deceased\_Declaration\_Date**: Original date of death declaration
* **Reversal\_Reason**: Description of why reversal is requested
* **Supporting Evidence**: Optional documentation references

> **Note**: The process name `CRVS_Fraud_Death` distinguishes reversal requests from standard death registrations.

**Step 2: MOSIP Validates the Request**

MOSIP performs the following validations before accepting the reversal request:

**1. Current Status Verification**

* **Check**: Ensure the individual is currently marked as deceased (`Declared_as_Deceased = Y`)
* **Action**: If not currently deceased, reject the request

**2. Time Window Validation**

Unlike fraud-birth scenarios, death reversals may require a **longer validity window** due to:

* Extended periods before reappearance (e.g., war, displacement, disaster)
* Lower risk since MOSIP only flips the flag without full ID reactivation

**Recommendation:**

* Calculate time window from `Deceased_Declaration_Date`
* **Recommended window**: 1-2 years (configurable by country policy)
* Countries can adjust based on local laws and contextual factors

**3. Duplicate Request Check**

* **Check**: Verify if a death reversal request for the same UIN is already in progress
* **Action**: If duplicate found, reject the new request

**4. Reversal History Check**

* **Check**: Verify if a previous death reversal has already been processed for this UIN
* **Action**: Accept only **one death reversal request per person** through CRVS
* **If second reversal attempted**: Reject and instruct the requestor to contact the National ID department for manual offline resolution

**5. Add Relevant Tags**

* Tag the packet with `CRVS_Fraud_Death` for tracking and anonymous profiling
* Store metadata for audit and traceability

> **Note**: Should MOSIP send acknowledgement to CRVS in case of rejection? This is under consideration.

**Step 3: Route to Manual Verification**

1. Route the packet to the **manual review queue** via the Camel route tied to `CRVS_Fraud_Death`
2. Provide the following information to the manual reviewer:
   * `Reversal_Reason`
   * **Complete history** of all updates to the packet for this individual
   * Supporting documentation (if evidence collection is supported)
   * Original death declaration details
3. Country authorities verify evidence before allowing reversal

**Step 4: Manual Verification Decision**

Authorized country administrators/legal authorities review:

* Supporting documentation
* Complete packet history
* Reversal reason and evidence
* Context of original death declaration

**Decision Options:**

**Option 1: Approve the Reversal**

1. Set `Declared_as_Deceased = N`
2. Remove the deceased status/flag in MOSIP ID repository (backend update by NID)
3. The National ID continues to be active (since it was never deactivated, only flagged)
4. Audit logs capture the complete chain of actions
5. No notification to CRVS (current policy)

**Option 2: Reject the Reversal**

1. `Declared_as_Deceased` remains `Y`
2. No further online reversal requests are accepted for this UIN
3. Future appeals must go to the National ID authority through **offline processes**
4. Packet status updated with rejection reason

**Step 5: Logging and Audit**

MOSIP maintains comprehensive audit trails including:

* **Original death declaration**: Date, source, reason
* **Reversal request**: Submission date, reason, supporting evidence
* **Validation outcomes**: All checks performed and results
* **Verification decision**: Approve/reject with justification
* **Complete action chain**: Full traceability for legal and administrative audits

The request metadata (**Process**, **Source**, and deceased fields) help distinguish:

* Original death registrations (`CRVS_Death`)
* Fraud/death reversals (`CRVS_Fraud_Death`)

***

#### MOSIP Options and Pros/Cons

| **Option**                                             | **Advantages**                                                                                         | **Disadvantages / Risks**                                                                                                                                                |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Automatic Online Reversal**                          | Fast; immediate response                                                                               | Very risky: could unmark deceased individuals without legal confirmation; prone to misuse; impacts downstream services without verification; legal and ethical liability |
| **Accept Request + Manual Verification (Recommended)** | Legally safe; aligns with civil registration practices; maintains audit trail; ensures human judgement | Requires administrative involvement; may delay final resolution                                                                                                          |
| **No Online Reversal Allowed (Offline Only)**          | Maximum safety; full control remains with authorities                                                  | Reduces transparency; delays correction for genuine cases; loses benefit of integration                                                                                  |

***

#### Recommended MOSIP Policy

1. **Use dedicated process value**: `CRVS_Fraud_Death` must be used for all reversal requests
2. **Source remains consistent**: `CRVS1` (or appropriate CRVS system identifier)
3. **Current status validation**: Only allow reversal if person is currently flagged as deceased (`Declared_as_Deceased = Y`)
4. **One-time reversal policy**: Accept only **one reversal request per person** through online CRVS integration
5. **Reject re-reversals**: Any subsequent reversal attempts must be handled offline by National ID authorities
6. **Longer time window**: Maintain a configurable time window (1-2 years recommended) from `Deceased_Declaration_Date` due to unique real-world scenarios (war, disaster, displacement)
7. **Mandatory manual verification**: All reversal requests must enter the manual verification queue; no automatic changes
8. **Full auditability**: Record all events, decisions, and supporting evidence

***

#### Additional Considerations

**National ID Status**

* **National ID is never deactivated** upon death declaration; only the deceased flag is set
* This makes reversal **lower-risk** compared to reactivation scenarios
* Downstream services rely on the flag for access control decisions

**Time Window Flexibility**

* Longer windows (1-2 years) accommodate real-world contexts:
  * War or conflict zones
  * Natural disasters
  * Displacement scenarios
  * Administrative backlogs
* Countries can configure based on local laws and operational needs

**Clear Rejection Rules**

* **One reversal per person** prevents repeated requests from becoming an endless loop
* Forces escalation to offline channels for complex or disputed cases
* Maintains system integrity and prevents misuse

**Traceability**

* Using consistent field structure (**Informant info + Deceased info**) enables:
  * Clear logging across all death-related events
  * Easier investigations and audits
  * Pattern detection for fraud prevention

**Downstream System Impacts**

* Financial services (insurance, pensions)
* Property transfers
* Legal identity status
* Access to government services

All these systems rely on the deceased flag, making reversal a sensitive operation requiring careful handling.

***

## Learn More

* [Death Registration & Identity Status Update](../integration-flows/death-registration-and-identity-status-update.md) - Understand the standard death registration workflow in CRVS-MOSIP integration, which is the baseline process from which fraud death case reversals deviate. This provides context on how deceased flags are originally set.
* [Manual Adjudication and Verification](../../../../../manual-adjudication-and-verification.md) - Learn about MOSIP's manual verification system used for reviewing fraud death reversals. This document details the queue-based architecture, decision workflows, and integration points that enable country authorities to approve or reject reversal requests.
* [Registration Processor Configurations Details](../../configurations-and-operations/configurations-details.md) - Explore the ID schema fields (`declaredAsDeceased`, `deceasedDeclarationDate`, `typeOfDeath`) and Camel route configurations (`CRVS_Fraud_Death`) that enable death-related packet processing and reversal workflows.
* [Reactivation of Deactivated National ID](reactivation-of-deactivated-national-id.md) - Compare fraud death reversal with another rare scenario involving National ID reactivation. Both follow similar manual verification patterns but apply to different identity lifecycle events (death vs. birth fraud).
