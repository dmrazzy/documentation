# Integration Patterns & Workflow

### High-Level Integration Flow







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
