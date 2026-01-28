# Operational Considerations

### Prerequisites & System Setup

This section outlines the technical requirements for integrating a Civil Registration and Vital Statistics (CRVS) system with MOSIP, applicable to any country implementation. It covers the necessary prerequisites and configurations to facilitate a seamless and secure connection between the two systems. Depending on the country's implementation model and agreements, these steps may be carried out by the System Integrator (SI) or the CRVS team.

#### Pre-integration Checklist

Before beginning integration, ensure the following are in place:

1. **MOSIP Platform Readiness**
   * MOSIP deployed and operational
   * Partner management module configured
   * eSignet service running
   * WebSub hub configured
   * Keycloak admin access available
2. **CRVS System Readiness**
   * CRVS system can generate API requests
   * CRVS has webhook endpoint for receiving notifications
   * CRVS can process WebSub notifications
   * CRVS implements eSignet authentication flow
3. **Network and Security**
   * Secure network connectivity between CRVS and MOSIP
   * TLS certificates configured
   * Firewall rules configured
4. **Data Readiness**
   * Field mapping agreed between CRVS and MOSIP
   * Mandatory attributes defined
   * Data validation rules documented

#### Configuration Steps

**Step 1: ID Schema Configuration**

To initiate any registration request, the country must define an ID schema based on the specific requirements for CRVS integration. The sample ID schema can be referred to here and should be customized to include all required fields for packet generation per the country's requirements. This schema governs the structure of the data submitted to MOSIP for processing and storage in the Identity Repository.

> **Note:** MOSIP advises adopting and customizing the latest released ID schema version to meet country-specific needs.

For comprehensive guidance on defining and customizing the ID schema in MOSIP, please refer to the [documentation here](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-management/id-schema).

**Step 2: Create Default Officer**

To process the request coming from CRVS, a default officer is to be registered and assigned to the CRVS. The officer is to be added from keycloak by the admin.

1. **Log in to Keycloak**
   * Open the Keycloak admin portal.
   * Log in using your admin credentials.
2. **Navigate to the Users Section**
   * In the left-hand menu, click on Users.
3. **Add a New User**
   * Click on the Add user button to create a new user.
   * Fill in the required fields under the details tab:
     * **Username**: Enter a unique username for the officer (this will become the Officer ID).
     * **First Name**: Enter the officer's first name.
     * **Last Name**: Enter the officer's last name.
4. **Set Temporary Password**
   * Under the **Credentials** tab, set the password for the officer.
   * Toggle **Temporary** to **Off** to disable the "temporary password" feature.
   * Click **Set Password**.
5. **Enable the User**
   * Under the **User Enabled** section, ensure that the user is **enabled**.
   * Click **Save** to create the user.
6. **Assign Roles to the User**
   * Navigate to the **Role Mappings** tab.
   * Under **Available Roles**, select the role **Registration\_officer**.
   * Click the **Add selected** button to assign the role.
7. **Finalize the Officer Creation**
   * After saving, the officer's **username** from step 3 will serve as the **Officer ID**.
   * This **Officer ID** will be used in the subsequent create packet API request. Ensure you pass this ID correctly in the request.

**Step 3: Create Centre**

A unique default centre will be assigned to the CRVS to process requests. This centre can be created through the Admin Portal. For detailed instructions on how to create a centre, refer to the [**Admin Portal Center Creation Guide**](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/administration/test/admin-portal-user-guide#create-center).

**Fetching the Centre ID**

As of now, there is no direct support for fetching the specific centre ID in MOSIP. To retrieve the Centre ID, use the API below to get a list of all centres in the system. From this list, manually search for the Centre ID associated with the newly created centre for CRVS. The "id" attribute in the response will be the centre ID.

**Get List of All Centres Endpoint:** `{domain}/v1/admin/masterdata/registrationcenters`

**Method:** GET

Once the **Centre ID** is identified, ensure it is saved securely for future reference. This **Centre ID** will be required in subsequent interactions with the CRVS system for processing requests.

**Step 4: Create Machine**

A unique default machine will be assigned to the CRVS to process requests. This machine can be created through the Admin Portal.

Before creating a new machine, it is required that a public key is fetched using the below API. The public key received in the response of this API is to be used as the public key & signing public key, while adding the details in the admin portal, to create and onboard the machine.

**URL**: `{domain}/v1/keymanager/tpmsigning/publickey`

**Method**: POST

**API Request Structure:**

```json
{
  "request": {
    "serverProfile": "Prod"
  }
}
```

**Sample Response:**

```json
{
  "id": null,
  "version": null,
  "responsetime": "2025-04-15T14:04:31.683Z",
  "metadata": null,
  "response": {
    "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzWOM81oggDiy26SPzASVCLpEf0sJC-81j7GCpDHVcHCAfQVIxaBP9K6u2R19mgiakVY92Nlb5y4PUKV1EpLbZKQxaK14gU4ks7hroCM_gbEssaon7lCFCu8uobKriXlC9RI1ZY9HF9QFfilCuGC9q58gZ_YC-VMGZOB9YtN_5QRbXvI9XQr-d1eODtOVuVCpsPz6FkEHOSWdj0HPLeGTLZO7Ac7dfMyksNJzfmad6PQ7i2GHQ1ZqK6aVTZOt37_kuGkUz7CzVhBNhsdYVeK-qG331dI66XMxSVYNK5O9poDzH1mAAIG_2MdxAEWHcDstZl6YvmWn5-JEhS6QdB6mFwIDAQAB"
  },
  "errors": null
}
```

For detailed instructions on how to create a machine, refer to the [**Admin Portal Machine Creation Guide**](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/administration/test/admin-portal-user-guide#machines)

> **Note: "/home/mosip/.mosipkeys"** location must be volume mounted and kept secure as per the best security practices to prevent unauthorised access.

**Fetching the Machine ID**

As of now, there is no direct support for fetching the specific machine ID in MOSIP. To retrieve the machine ID, use the API below to get a list of all machines in the system. From this list, manually search for the machine ID associated with the newly created machine for CRVS. The "id" attribute in the response will be the machine ID.

**Get a List of All Machine Endpoints:** `{domain}/v1/masterdata/machines`

**Method:** GET

Once the **Machine ID** is identified, ensure it is saved securely for future reference. This **Machine ID** will be required in subsequent interactions with the CRVS system for processing requests.

**Step 5: Map Officer to Centre and Machine**

Once the officer, centre, and machine are created for CRVS, the next step is to map the user to the centre. This ensures the user is properly associated with the correct zone and centre for their operations.

1. **Log in to Admin UI**
   * Log in to the Admin UI using the **admin** user created in Keycloak if it is a fresh environment.
2. **Map Zone to User**
   * Navigate to **Resources** → **User Zone Mapping** → **Map Zone**.
   * Select the **username** for which the zone is to be mapped.
   * Choose the appropriate **zone** from the dropdown.
   * Click **Save**.
3. **Activate the User**
   * After mapping the zone, activate the user.
4. **Map Centre to User**
   * Navigate to **Resources** → **User Centre Mapping**.
   * Select the **username** for which the centre needs to be mapped.
   * Choose the appropriate **centre** from the dropdown.
   * Click **Save**.
   * Activate the user, as outlined in Step 3.

### 11.2 Partner Onboarding Process

### 11.3 Performance & Scalability

### 11.4 Monitoring & Alerting

### 11.5 Maintenance & Support

***
