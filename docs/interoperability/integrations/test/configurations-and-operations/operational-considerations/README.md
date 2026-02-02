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

#### Configuring Pre-requisite Steps

#### **1. ID schema Configuration:** <a href="#id-schema-configuration" id="id-schema-configuration"></a>

To initiate any registration request, the country must define an ID schema based on the specific requirements for CRVS integration. The sample ID schema can be referred to [here](../../../../../_files/id-schema/id-schema-sample.json) and should be customized to include all required fields for packet generation per the country’s requirements. This schema governs the structure of the data submitted to MOSIP for processing and storage in the Identity Repository.

{% hint style="info" %}
**Note:** MOSIP advises adopting and customizing the latest released ID schema version to meet country-specific needs.
{% endhint %}

For comprehensive guidance on defining and customizing the ID schema in MOSIP, please refer to the [documentation here](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-management/id-schema).

#### **2. Create Client ID/Role for the CRVS** <a href="#id-1.-create-client-id-role-for-the-crvs" id="id-1.-create-client-id-role-for-the-crvs"></a>

As part of the integration approach, two specific API’s are exposed:

1. Create a packet API from the MOSIP packet manager module to create a packet
2. Trigger the API from the registration processor module to process the packet.

allowing external systems (in this case, CRVS) to use these APIs to initiate requests.

To facilitate this, the external system must be assigned a specific new client ID and secret, ensuring secure and authenticated communication. Additionally, a new, specific role should be created for the external user, which will be associated with the API request in subsequent calls for packet creation and processing.

This role helps MOSIP validate and verify that the request is coming from an authorized and authentic source, ensuring secure and accurate handling of the registration process. By associating the role with the API request, MOSIP can properly authenticate the external system and manage permissions for the request flow.

Read on to learn more about the specific steps involved

**Create the Client**

* **Log in to Keycloak Admin Console**
  * Access the keycloak admin console.
  * Ensure you have the necessary administrative privileges to create clients.
* **Select Your Realm**
  * If you are not already in the desired realm, switch to it from the top-left drop-down menu. The realm should be the one where you want to create the client.
* **Create a New Client**
  * In the left-hand menu, go to **Clients** and click on **Create**.
* **Enter the Client Details**
  * **Client ID**: Enter `mosip-crvs1-client` as the client ID (or a relevant name based on your deployment).
  * **Client Protocol**: Select `openid-connect`.
  * **Root URL**: Leave this field blank or enter the URL if required.
* **Save the Client**
  * After entering the necessary details, click **Save** to create the client.

Once the client is created, please update the properties in the locations below:

1. `auth.server.admin.allowed.audience` In the [Packet manager default properties](https://github.com/mosip/mosip-config/blob/v1.2.4.0/packet-manager-default.properties#L24).
2. `auth.server.admin.allowed.audience` In the [Registration processor default properties](https://github.com/mosip/mosip-config/blob/v1.2.4.0/registration-processor-default.properties#L996).

{% hint style="info" %}
**Note:** The client name specified here is a placeholder and can be customised to suit the specific requirements of the System Integrator SI/CRVS.
{% endhint %}

**Configuring the Client**

* **Access the Settings Tab**
  * After creating the client, navigate to the **Settings** tab.
* **Configure Client Settings**
  * **Access Type**: Set this to **confidential** if you intend to use client credentials for authentication.
  * **Service Accounts Enabled**: Turn this option **ON** if you are using client credentials flow for secure communication.
  * **Valid Redirect URIs**: Enter `*` (or specify specific URLs if known and necessary).
* **Save the Changes**
  * Once the configuration is complete, click **Save** to apply the changes.

**Generate and note the Secret key**

* Navigate to the **Credentials** tab.
* If you selected the **confidential** access type, keycloak will generate a **Secret Key**. Note this secret as it will be used for authentication in subsequent API calls.

**Creating the Role**

* **Go to the Roles Section**
  * In the Keycloak Admin Console, under your realm, navigate to **Roles**.
* **Create a New Role**
  * Click on **Add Role**.
  * Enter the following details:
    * **Role Name**: `ONLINE_REGISTRATION_CLIENT`
  * Click **Save** to create the role.

#### **Assigning the Role to the Client**

* **Assign the Role to the Client**
  * Go back to the **Clients** section and select the client `mosip-crvs1-client` that you previously created.
* **Navigate to Service Account Roles**
  * Under the **Service Account Roles** tab (this tab is visible only if **Service Accounts Enabled** is turned on), click on **Add Role**.
* **Select the Role**
  * From the **Client Roles** dropdown, select either `realm-management` or your specific desired client role (if the role is specific to a client).
  * Add the `ONLINE_REGISTRATION_CLIENT` role to the selected client.

#### **3. Fetch Access Token to Call the APIs** <a href="#id-2.-fetch-access-token-to-call-the-apis" id="id-2.-fetch-access-token-to-call-the-apis"></a>

Once the role is created and mapped to the client ID. As a follow-up step, below keycloak API is to be called to authenticate the CRVS associated with the new role. In the response of the API, there is an access token returned in the response header. This is the access token that should be used when initiating any request using the packet manager API.\
\
**Authenticate Endpoint:** {domainname}/v1/authmanager/authenticate/clientidsecretkey

**Method:** POST

**API Request Structure:**

```json
{
  "id": "string",
  "metadata": {},
  "request": {
    "appId": {{appId}},
    "clientId": {{clientId}},
    "secretKey": {{secretKey}}
  },
  "requesttime": "{{requestTime}}",
  "version": "string"
```

In the API above, the fields Client ID and Secret key are the values created in the previous steps, as mentioned above. Once the authentication is successful, in the response header, we will receive an access token, which is to be noted and used for the subsequent packet manager API request.

#### 4. Create the Default Officer <a href="#id-3.-create-the-default-officer" id="id-3.-create-the-default-officer"></a>

To process the request coming from CRVS, a default officer is to be registered and assigned to the CRVS. The officer is to be added from keycloak by the admin.

* **Log in to Keycloak**
  * Open the Keycloak admin portal.
  * Log in using your admin credentials.
* **Navigate to the Users Section**
  * In the left-hand menu, click on Users.
* **Add a New User**
  * Click on the Add user button to create a new user.
  * Fill in the required fields under the details tab:
    * **Username**: Enter a unique username for the officer (this will become the Officer ID).
    * **First Name**: Enter the officer's first name.
    * **Last Name**: Enter the officer's last name.
* **Set Temporary Password**
  * Under the **Credentials** tab, set the password for the officer.
  * Toggle **Temporary** to **Off** to disable the "temporary password" feature.
  * Click **Set Password**.
* **Enable the User**
  * Under the **User Enabled** section, ensure that the user is **enabled**.
  * Click **Save** to create the user.
* **Assign Roles to the User**
  * Navigate to the **Role Mappings** tab.
  * Under **Available Roles**, select the role **Registration\_officer**.
  * Click the **Add selected** button to assign the role.
* **Finalize the Officer Creation**
  * After saving, the officer's **username** from step 3 will serve as the **Officer ID**.
  * This **Officer ID** will be used in the subsequent create packet API request. Ensure you pass this ID correctly in the request.

#### 5. Create Centre <a href="#id-4.-create-centre" id="id-4.-create-centre"></a>

A unique default centre will be assigned to the CRVS to process requests. This centre can be created through the Admin Portal. For detailed instructions on how to create a centre, refer to the [**Admin Portal Center Creation Guide**.](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/administration/test/admin-portal-user-guide#create-center)

**Fetching the Centre ID**\
As of now, there is no direct support for fetching the specific centre ID in MOSIP. To retrieve the Centre ID, use the API below to get a list of all centres in the system. From this list, manually search for the Centre ID associated with the newly created centre for CRVS. The “id“ attribute in the response will be the centre ID.

**Get List of All Centres Endpoint:** `{domain}/v1/admin/masterdata/registrationcenters`

**Method:** GET\
\
Once the **Centre ID** is identified, ensure it is saved securely for future reference. This **Centre ID** will be required in subsequent interactions with the CRVS system for processing requests.

#### 6. Create Machine <a href="#id-5.-create-machine" id="id-5.-create-machine"></a>

A unique default machine will be assigned to the CRVS to process requests. This machine can be created through the Admin Portal.

Before creating a new machine, it is required that a public key is fetched using the below API. The public key received in the response of this API is to be used as the public key & signing public key, while adding the details in the admin portal, to create and onboard the machine.

**URL**: `{domain}/v1/keymanager/tpmsigning/publickey`

**Method**: POST

**API Request Structure:**

```json
{     "request" : 
    {         
        "serverProfile" : "Prod"    
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
    "response": 
    {
        "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzWOM81oggDiy26SPzASVCLpEf0sJC-81j7GCpDHVcHCAfQVIxaBP9K6u2R19mgiakVY92Nlb5y4PUKV1EpLbZKQxaK14gU4ks7hroCM_gbEssaon7lCFCu8uobKriXlC9RI1ZY9HF9QFfilCuGC9q58gZ_YC-VMGZOB9YtN_5QRbXvI9XQr-d1eODtOVuVCpsPz6FkEHOSWdj0HPLeGTLZO7Ac7dfMyksNJzfmad6PQ7i2GHQ1ZqK6aVTZOt37_kuGkUz7CzVhBNhsdYVeK-qG331dI66XMxSVYNK5O9poDzH1mAAIG_2MdxAEWHcDstZl6YvmWn5-JEhS6QdB6mFwIDAQAB"
    },
    "errors": null
}
```

For detailed instructions on how to create a machine, refer to the [**Admin Portal Machine Creation Guide**](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/administration/test/admin-portal-user-guide#machines)

{% hint style="info" %}
**Note: “/home/mosip/.mosipkeys“** location must be volume mounted and kept secure as per the best security practices to prevent unauthorised access.
{% endhint %}

**Fetching the Machine ID**

As of now, there is no direct support for fetching the specific machine ID in MOSIP. To retrieve the machine ID, use the API below to get a list of all centres in the system. From this list, manually search for the machine ID associated with the newly created centre for CRVS. The “id“ attribute in the response will be the machine ID.

**Get a List of All Machine Endpoints:** `{domain}/v1/masterdata/machines`

**Method:** GET

Once the **Machine ID** is identified, ensure it is saved securely for future reference. This **Machine ID** will be required in subsequent interactions with the CRVS system for processing requests.

#### 7. Map Officer to Centre and Machine <a href="#id-6.-map-officer-to-centre-and-machine" id="id-6.-map-officer-to-centre-and-machine"></a>

Once the officer, centre, and machine are created for CRVS, the next step is to map the user to the centre. This ensures the user is properly associated with the correct zone and centre for their operations.

1. **Log in to Admin UI**
   1. Log in to the Admin UI using the **admin** user created in Keycloak if it is a fresh environment.
2. **Map Zone to User**
   1. Navigate to **Resources** → **User Zone Mapping** → **Map Zone**.
   2. Select the **username** for which the zone is to be mapped.
   3. Choose the appropriate **zone** from the dropdown.
   4. Click **Save**.
   5. Refer to the accompanying images for further guidance.
3. **Activate the User**
   1. After mapping the zone, activate the user.
4. **Map Centre to User**
   1. Navigate to **Resources** → **User Centre Mapping**.
   2. Select the **username** for which the centre needs to be mapped.
   3. Choose the appropriate **centre** from the dropdown.
   4. Click **Save**.
   5. Activate the user, as outlined in Step 3.

#### 8. Create the Application ID (AID) <a href="#id-7.-create-the-rid" id="id-7.-create-the-rid"></a>

The Application ID (AID) refers to the unique identifier assigned to track the packet that is being processed for events such as birth or death registration. It can be used by MOSIP or CRVS to track the progress and status of the specific event.

**AID Structure(Recommended):**

1. **Centre ID** (First 5 digits): The first 5 digits of the RID represent the **Centre ID**.
2. **Machine ID** (Next 5 digits): The next 5 digits of the RID represent the **Machine ID**.
3. **Random Sequence** (next N digits): The next N digits can be a randomly generated sequence based on the length that the country wants to use for the RID.

**Example of AID:**

For the AID `10001100771006920220128223618`The breakdown is as follows:

1. **Centre ID**: `10001` (First 5 digits)
2. **Machine ID**: `10077` (Next 5 digits)
3. **Random Sequence**: `1006920220128223618` (Remaining 16 digits)

The AID format mentioned above is the recommendation to be followed, but not mandatory. CRVS can generate the AID in any specified format as per their requirement and include it in the Create Packet API Request to ensure proper packet identification and mapping.

Once all the above pre-requisites are in place, the next step is to initiate a request(birth/death/update) by calling the create packet API of MOSIP’s packet manager module. API structure, required fields, and other details are mentioned further in this document.

***

### Learn more

* [Security & Authentication](../prerequisites/security-and-authentication.md) - OAuth client setup, access tokens, and eSignet flow.
* [Policy Configuration & Customization](../configuration-and-customization.md) - Credential sharing choices (PSUT/UIN/VID) and partner policy setup.
* [Notifications & Event Handling](../../notifications-and-event-handling.md) - WebSub topics for credentials and packet status updates.
* [API Reference & Data Models](../../api-reference-and-data-models.md) - Packet APIs, payloads, and data structures used by CRVS.
* [Core Integration Principles](../../integration-overview-and-context/core-integration-principles.md) - Trust boundary, verification expectations, and defaults.
* [MOSIP Configuration Changes for CRVS Birth and Death Requests](../prerequisites/configurations-details.md) - ID schema fields, auth-policy updates, and routing configs.
* [Error Handling & Reconciliation](../../error-handling-and-reconciliation.md) - Failures, retries, and reconciliation patterns.
