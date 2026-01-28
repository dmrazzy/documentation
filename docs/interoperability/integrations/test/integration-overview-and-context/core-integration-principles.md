# Core Integration Principles

### CRVS as Source of Truth

**CRVS is considered the source of truth**. MOSIP relies on CRVS to perform deduplication and treats CRVS as the authoritative source of vital event data. MOSIP does not perform de-duplication or data validation on requests from CRVS systems because of this trust relationship.

**Assumption:** This approach assumes that the CRVS system is recognized as the authoritative source of vital event data and is legally authorized to issue official birth and death certificates in accordance with the laws of the country.

> **See Section 1.5.1** for a detailed explanation of why Foundational ID and CRVS systems maintain distinct responsibilities and authorities.

### Trust & Data Verification Model

#### Data Verification and Trust

1. The initiation of the request with the data will be from CRVS to MOSIP
2. Data transmitted from CRVS to MOSIP will undergo thorough data/document verification and deduplication before any request is initiated for MOSIP to process.
3. MOSIP will trust all information provided by CRVS and honor the received requests. MOSIP will ensure that the technical and logical validations are all in place for packet processing.
4. The integration should define the mandatory attributes to be exchanged between the two systems. It is crucial to agree upon these attributes to ensure consistent and accurate data exchange.
5. **Exception for Rare Scenarios**: For sensitive cases such as fraud detection (Section 4.6.1), identity reactivation (Section 4.6.2), and death flag reversals (Section 4.6.3), MOSIP does not automatically process requests. Instead, these are routed to manual verification queues where authorized country authorities review evidence and make final decisions. This ensures legal compliance and prevents misuse while maintaining the trust relationship with CRVS.

### Identity Lifecycle Management

### De-duplication Strategy

> **Note**: MOSIP relies on CRVS to perform deduplication and treats CRVS as the source of truth. While MOSIP has its internal deduplication mechanism to detect and reject duplicate packets, duplicate and repeated request scenarios are not currently handled for rejection by MOSIP.

**Exception for Rare Scenarios**: For fraud detection, reactivation, and death reversal requests (Section 4.6), MOSIP enforces a **one-time request policy** per National ID. If a fraud flag or reversal request is already in progress or has been previously processed, subsequent duplicate requests are automatically rejected. This prevents repeated requests from creating endless loops and maintains system integrity.

### Authentication & Authorization Model

#### Proof of Authentication

1. For any request submitted to MOSIP, it is essential to include the biometric data of the informant or applicant, whenever feasible, to ensure the accurate verification of the individual initiating the request and confirm their legitimacy.
2. For cases where biometric data is not collected by CRVS, it is recommended for CRVS to use eSignet for the applicant or introducer authentication. Post-authentication user info token should be shared with MOSIP for validation and auditing purposes.

### Notification & Event Architecture

#### Identity Credential Sharing & Packet status

1. The identity credential sharing and packet status updates are two separate MOSIP integration points for any CRVS.
2. MOSIP will generate and share identity credentials for new registration cases via WebSub. Any partner configured according to the policy will receive these notifications automatically.
3. By default, MOSIP recommends sharing the PSUT token as part of the identity credential returned to CRVS. However, the system also supports sharing UIN, VID, or the PSUT token, based on the country's specific requirements.
4. MOSIP will publish packet status updates to a separate WebSub. Any credential partner subscribed to this WebSub topic can filter and track specific packets based on their status.

#### Multiple CRVS Systems

1. If a country deploys multiple CRVS systems, each system will be treated as a separate partner for MOSIP. This allows for independent management and integration with each CRVS system.
2. Credentials generated/update messages from MOSIP will be sent to all the configured credential partners subscribed to the websub as per policy. It is the responsibility of the country and CRVS systems to manage the sharing of information across multiple CRVS systems.
