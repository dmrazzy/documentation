# Demographic Data Updates

#### When Does It Happen?

Demographic updates can occur when there are changes to an individual's personal information (e.g., name change, address update). This is more common for infants (pre-biometric stage) but can also apply to adults in specific cases.

#### What Does MOSIP Do?

MOSIP receives the update request from CRVS, validates the request, and updates the demographic information in the identity repository. For infants (without biometrics), the update is straightforward. For adults (with biometric records), updates are more restricted.

#### What does CRVS Receive?

MOSIP sends a status update notification via WebSub confirming the successful demographic update.

#### What Is the Workflow?

1. Applicant submits demographic update request through CRVS
2. CRVS validates the information and submits update request to MOSIP
3. MOSIP validates and processes the update
4. MOSIP sends status update via WebSub to CRVS
5. CRVS confirms update to the applicant

#### Handling Failures in this Scenario

| **Scenario**                           | **Existing Handling / Mechanism**                                                                          | **Improvement Required?**                                        |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| **Update Rejected (Biometric Linked)** | MOSIP currently does not support demographic updates for individuals with biometric-linked UINs from CRVS. | Document this limitation clearly. Should there be exceptions?    |
| **Invalid Update Data**                | MOSIP validates update requests and rejects invalid data.                                                  | Enhancement: Provide detailed validation error messages to CRVS. |
