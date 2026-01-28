# Integration Overview and Context

### What is covered in sub-sections

This section establishes the foundation for understanding MOSIP-CRVS integration. It covers:

* **Core Integration Principles**: The fundamental architectural and operational principles that guide how MOSIP and CRVS systems interact, including system roles, data flow patterns, and coordination mechanisms.
* **Integration Boundaries & Real-World Implications**: The rationale behind integration scope decisions, explaining why certain limitations exist and how they protect individual rights, maintain system integrity, and respect the distinct mandates of Foundational ID and Civil Registration systems.











***



### What's NOT in Scope (Out-of-Scope)

#### Integration Limitations

The limitations listed below are not arbitrary technical constraints. Each reflects careful consideration of real-world implications documented in Section 1.5 (Integration Boundaries: Principles & Real-World Implications). These boundaries protect individuals, maintain system integrity, and align with the distinct operational models of Foundational ID and Civil Registration systems.

Although the integration scope includes scenarios for birth, death, and updates, there are still some cases where limitations exist.

1. **No Support for New Adult Birth Registrations**: MOSIP does not support new adult birth registrations when the request comes from CRVS.
2. **No Support for Demographic Updates When Biometrics Are Already Linked to a National ID**: MOSIP will not support demographic updates from CRVS for individuals whose biometrics are already linked to a National ID. For such updates, individuals are expected to visit the National ID registration centre (MOSIP).
3. **Integration for Birth and Death Registrations**: The integration works seamlessly for birth and death registrations. However, updates to demographic data are still a work in progress.
4. **Duplicate Request Rejection**: Since the CRVS system is considered the source of truth, MOSIP currently does not reject duplicate birth/death registration requests received from the CRVS system. This can result in multiple UINs for the same infant or an update of the death flag for the same deceased. Deduplication is expected to be handled by CRVS. **Exception**: For rare scenarios (fraud detection, identity reversals) covered in Section 4.6, MOSIP enforces one-time request policies and routes cases to manual verification rather than automatic processing.
5. **No Support for Rejected Packets, Status Updates, and Reason**: If a request fails due to validation issues in MOSIP, there is currently no mechanism to send detailed rejection reasons back to CRVS.
6. **No Automatic Deactivation or Reactivation**: While CRVS can submit fraud reports or reactivation requests (Section 4.6), MOSIP does not automatically deactivate or reactivate National IDs. All such requests are routed to manual verification queues where authorized country authorities make final decisions.
7. **No Automatic Deactivation or Reactivation**: While CRVS can submit fraud reports or reactivation requests (Section 4.6), MOSIP does not automatically deactivate or reactivate National IDs. All such requests are routed to manual verification queues where authorized country authorities make final decisions.
8. **No support for Offline Integration**: This integration works only when online connectivity is available as eSignet authentication is a necessary step before a request is submitted to CRVS.
9. **Use of VID/UIN for Death Registration and Demo Data Updates**: Only VID or UIN can be used to register a death or submit requests for the demo data updates. Currently, MOSIP does not support death updates with any other identifier.
10. **Time-Bound Rare Scenario Requests**: Fraud detection and reversal requests (Section 4.6) are only accepted within configurable time windows from the initial registration/declaration date. Requests outside these windows are automatically rejected and must follow offline grievance channels.

### System Roles & Responsibilities
