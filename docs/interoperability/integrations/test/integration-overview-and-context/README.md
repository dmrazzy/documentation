# Integration Overview and Context

### Purpose, Audience and Integration Value Proposition

MOSIP can integrate with CRVS (Civil Registration and Vital Statistics) systems to create a unified digital identity ecosystem that spans an individual's entire life cycle.

**Civil Registration and Vital Statistics (CRVS)** systems are essential for documenting key life events such as births, deaths, marriages, and divorces. These systems provide individuals with legal recognition and play a vital role in ensuring access to rights and services. Additionally, CRVS systems generate important demographic data that supports effective policy-making, governance, and resource planning.

**MOSIP** is an open-source, modular identity platform designed for scalability, interoperability, and compliance with global standards. It allows countries to customize identity systems tailored to their governance needs. To learn more about MOSIP, click [here](https://docs.mosip.io/1.2.0/id-lifecycle-management).

This synergy allows for the seamless management of an individual's identity across life events, starting from birth registration, through education and employment, to end-of-life verification.

> Note: When these systems operate in isolation, it leads to administrative inefficiencies, fragmented data, and challenges in service access.

### Supported Use Cases

**Must read**: Refer to 'What is in scope of integration (integration-depth) and what is kept from integration, and why!

#### Use Cases Supported Through CRVS-MOSIP Integration

This integration currently supports the following use cases:

1. Birth registration
   1. Birth registration is initiated by the CRVS system
   2. Duplicate and/or repeated infant birth registration requests
   3. Handling failures
2. Death registration
   1. New death registration initiated by CRVS
   2. Duplicate and/or repeated requests for death registration
   3. Handling failures
3. Demographic data update
   1. Infant demo data update request initiated by CRVS
   2. Duplicate and/or repeated infant demo data update requests
   3. Adult demo data update request initiated by CRVS
4. Rare scenarios requiring manual verification
   1. Fraudulent birth registration - National ID deactivation
   2. Reactivation of deactivated National ID
   3. Death flag reversal (fraud death case)

> **Note**: These are the currently supported scenarios. The rare scenarios (4.6.x) involve manual verification processes and are not fully automated. Additional use cases will be introduced as the integration evolves and expands based on country-specific requirements and feedback.

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
