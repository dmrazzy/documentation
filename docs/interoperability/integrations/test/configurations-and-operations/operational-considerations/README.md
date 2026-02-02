# Operational Considerations

This section covers following requirements needed to run the MOSIP–CRVS integration.

Use it to create the operational entities that CRVS-initiated workflows depend on (centres, machines, officers, and an application identity).



* [Create Centre](create-centre.md): Create a centre to represent the physical/organizational location where CRVS operations are anchored.
* [Create the Default Officer](create-the-default-officer.md): Configure a default officer for cases where the system needs an operational “owner” for a transaction.
* [Create Machine](create-machine.md): Register a machine/device that will be associated with a centre for operational tracking and access controls.
* [Map Officer to Centre and Machine](map-officer-to-centre-and-machine.md): Assign an officer/operator to the right centre and machine so transactions are authorized and auditable.
* [Create the Application ID (AID)](create-the-application-id-aid.md): Create an application identity used by CRVS-initiated flows and integrations where an AID is required.



***

### Learn more

* [Security & Authentication](../prerequisites/security-and-authentication.md) - OAuth client setup, access tokens, and eSignet flow.
* [Policy Configuration & Customization](../configuration-and-customization.md) - Credential sharing choices (PSUT/UIN/VID) and partner policy setup.
* [Notifications & Event Handling](../../notifications-and-event-handling.md) - WebSub topics for credentials and packet status updates.
* [API Reference & Data Models](../../api-reference-and-data-models.md) - Packet APIs, payloads, and data structures used by CRVS.
* [Core Integration Principles](../../integration-overview-and-context/core-integration-principles.md) - Trust boundary, verification expectations, and defaults.
* [MOSIP Configuration Changes for CRVS Birth and Death Requests](../prerequisites/configurations-details.md) - ID schema fields, auth-policy updates, and routing configs.
* [Error Handling & Reconciliation](../../error-handling-and-reconciliation.md) - Failures, retries, and reconciliation patterns.
