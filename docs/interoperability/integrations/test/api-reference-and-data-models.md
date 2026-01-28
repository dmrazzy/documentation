# API Reference & Data Models

### API Endpoints & Request Structure

#### Overview

Once all the prerequisites are in place, the next step is to initiate a request (birth/death/update) by calling the create packet API of MOSIP's packet manager module, followed by the trigger API to process the packet.

#### Application ID (AID) Structure

The Application ID (AID) refers to the unique identifier assigned to track the packet that is being processed for events such as birth or death registration. It can be used by MOSIP or CRVS to track the progress and status of the specific event.

**AID Structure (Recommended):**

1. **Centre ID** (First 5 digits): The first 5 digits of the AID represent the **Centre ID**.
2. **Machine ID** (Next 5 digits): The next 5 digits of the AID represent the **Machine ID**.
3. **Random Sequence** (next N digits): The next N digits can be a randomly generated sequence based on the length that the country wants to use for the AID.

**Example of AID:**

For the AID `10001100771006920220128223618`

The breakdown is as follows:

1. **Centre ID**: `10001` (First 5 digits)
2. **Machine ID**: `10077` (Next 5 digits)
3. **Random Sequence**: `1006920220128223618` (Remaining 16 digits)

The AID format mentioned above is the recommendation to be followed, but not mandatory. CRVS can generate the AID in any specified format as per their requirement and include it in the Create Packet API Request to ensure proper packet identification and mapping.

#### Create Packet API (Packet Manager Module)

**Create Packet Endpoint:** `{domain}/commons/v1/packetmanager/createPacket`

**Method:** PUT

**API Request Structure:**

```json
{
  "id": "string",
  "version": "string",
  "requesttime": "2025-02-25T11:14:17.667Z",
  "request": {
    "source": "CRVS",
    "process": "CRVS_NEW",
    "id": "10001100771006920220128223618",
    "ref_id": "10001_10077",
    "schema_version": "0.1",
    "fields": {
      "fullName": "John Doe",
      "dateOfBirth": "2025-01-01",
      "gender": "Male",
      "addressLine1": "123 Main Street",
      "addressLine2": "Apt 4B",
      "city": "Capital City",
      "state": "State Name",
      "postalCode": "12345",
      "email": "john.doe@example.com",
      "phone": "+1234567890",
      "zone": "Zone1",
      "region": "Region1",
      "province": "Province1"
    },
    "metaInfo": {
      "centerId": "10001",
      "machineId": "10077",
      "operationsData": {
        "officerId": "officer123",
        "officerPassword": "password",
        "supervisorId": "supervisor456"
      },
      "registration_type": "CRVS_NEW"
    },
    "audit": {
      "uuid": "unique-uuid-1234",
      "createdBy": "CRVS_OPERATOR",
      "createdDateTime": "2025-02-25T11:14:17.667Z",
      "updatedBy": "CRVS_OPERATOR",
      "updatedDateTime": "2025-02-25T11:14:17.667Z"
    },
    "schemaJson": "{\"$schema\":\"http://json-schema.org/draft-07/schema#\"}"
  }
}
```

> **Note**: The API request shared above is only a sample and is not to be used for any implementation. Customize based on country-specific ID schema and requirements.

**Field Descriptions**

**Request Object:**

1. `source`: Specifies the source of the registration request. This will be the same for any request that comes to MOSIP for birth or death.
2. `process`: Identifies the specific process for the registration.
   * `CRVS_NEW` - When initiating an infant birth request
   * `CRVS_DEATH` - When initiating a death registration request
   * `CRVS_UPDATE` - When initiating a demographic update request
   * `CRVS_fraud_birth` or `CRVS_deactivate_ID` - When submitting a fraud detection/deactivation request (Section 4.6.1 and 4.6.2)
   * `CRVS_Fraud_Death` - When submitting a death flag reversal request (Section 4.6.3)
3. `id`: The unique identifier for the registration request (AID).

> **Note**: As per the current implementation, if the same AID is used twice, the record will be updated with the latest request data.

4. `ref_id`: Combination of centre ID and machine ID.
   * Ex - "centerid\_machineid"
5. `schema_version`: The version of the ID schema that the country is using in production.

**Field Object:**

1. `fullName`: The full name of the individual.
2. `dateOfBirth`: The date of birth of the individual.
3. `gender`: Gender of the individual.
4. `addressLine1`: First line of the address.
5. `addressLine2`: Second line of the address.
6. `city`: City of residence.
7. `state`: State of residence.
8. `postalCode`: Postal code of the address.
9. `email`: Email address.
10. `phone`: Contact phone number.
11. `zone`: Geographic zone.
12. `region`: Region of the address.
13. `province`: Province of residence.
14. Additional fields can be added based on country requirements and ID schema.

**Additional Fields for** [**Rare Scenarios**](integration-patterns-and-workflow/rare-scenarios/)**:**

These fields must be added to the ID schema to support fraud detection, reactivation, and death reversal workflows:

**For** [**Fraud Birth**](integration-patterns-and-workflow/rare-scenarios/fraudulent-birth-registrations-national-id-deactivation-request-from-crvs.md) **/ Deactivation Requests:**

1. `fraud_birth` or `Fraud_Birth`: Boolean field (`True` for deactivation, `False` for reactivation)
2. `fraud_birth_reason` or `Deactivation_reason`: String describing the reason for deactivation/reactivation
3. `date_of_initial_registration`: Date of the original birth registration
4. `National_ID`: UIN of the individual

**For** [**Death Reversal Requests**](integration-patterns-and-workflow/rare-scenarios/fraud-death-case-reversal-of-the-death-flag.md) **:**

1. `Declared_as_Deceased`: String field (`Y` = deceased, `N` = reversal)
2. `Deceased_Declaration_Date`: Original date of death declaration
3. `Reversal_Reason`: Description of why reversal is requested
4. `UIN` or `VID`: National ID of the individual

> **Note**: Field names may vary based on country-specific ID schema design. Consult Section 4.6 for detailed workflow requirements.

**MetaInfo Object (Center and Operator Information):**

1. `centerId`: Unique identifier for the centre where the registration is processed.
2. `machineId`: Unique identifier for the machine used for registration.
3. `operationsData`: Contains fields such as officer ID, officer password, supervisor ID, etc.
4. `registration_type`: This is the value same as the process field for birth (`CRVS_NEW`) or death (`CRVS_DEATH`).

`centerId`, `machineId`, `officerId` must be provided along with any additional relevant operational information for the request to be processed.

**Audit Object:**

1. `uuid`: Unique uuid to be sent with any request coming from CRVS.
2. Please add the values for the other fields in the audit object as per the details of the machine and the person who is registering the request from the CRVS.

It is required that at least one attribute in the audit object is populated with valid data before making the request.

`schemaJson`: JSON schema (in stringified format) used by the country

**Sample Response:**

```json
{
  "id": "mosip.registration.packet.writer",
  "version": "1.0",
  "responsetime": "2025-02-25T11:14:17.667Z",
  "metadata": null,
  "response": {
    "status": "SUCCESS",
    "packetId": "10001100771006920220128223618",
    "message": "Packet created successfully"
  },
  "errors": []
}
```

#### Trigger API (Registration Processor Module)

In MOSIP, after a packet is created, it is processed for validations and verification of the information in the **Registration Processor**. Inside the Registration Processor, each packet follows a specific workflow defined by the **Camel route**.

For the integration with CRVS, the newly created packet is uploaded to the Object Store. To pick up this new packet and trigger the processing, we have developed a new API. This API ensures to trigger the appropriate workflow is triggered for further processing of the registration packet. For this integration, the camel route workflow to be executed is determined by the values provided for the **source** and **process**.

**API Documentation:** [Create workflow instance for packet processing | Registration processor](https://mosip.stoplight.io/docs/registration-processor/branches/main/d56c892cfa950-create-workflow-instance-for-packet-processing)

**Trigger Endpoint:** `{domain}/registrationprocessor/v1/registration-processor/workflow/trigger`

**Method:** POST

**API Request Structure:**

```json
{
  "id": "mosip.registration.processor.workflow.trigger",
  "version": "1.0",
  "requesttime": "2025-02-25T11:14:17.667Z",
  "request": {
    "registrationId": "10001100771006920220128223618"
  }
}
```

**Sample Response:**

```json
{
  "id": "mosip.registration.processor.workflow.trigger",
  "version": "1.0",
  "responsetime": "2025-02-25T11:14:17.667Z",
  "response": {
    "status": "SUCCESS",
    "message": "Workflow triggered successfully"
  },
  "errors": []
}
```

### 6.2 Request/Response Schemas

### 6.3 Data Exchange Format

### 6.4 Field Mapping (CRVS ↔ MOSIP)

### 6.5 Mandatory vs Optional Fields

### 6.6 Data Validation Rules



***

### Learn More

* [Schema Configuration](operational-considerations.md)

