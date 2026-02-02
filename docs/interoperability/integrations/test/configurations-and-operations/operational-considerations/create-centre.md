# Create Centre

### Overview <a href="#id-4.-create-centre" id="id-4.-create-centre"></a>

A unique default centre will be assigned to the CRVS to process requests. This centre can be created through the Admin Portal. For detailed instructions on how to create a centre, refer to the [**Admin Portal Center Creation Guide**.](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/administration/test/admin-portal-user-guide#create-center)

### **Fetching the Centre ID**

As of now, there is no direct support for fetching the specific centre ID in MOSIP. To retrieve the Centre ID, use the API below to get a list of all centres in the system. From this list, manually search for the Centre ID associated with the newly created centre for CRVS. The “id“ attribute in the response will be the centre ID.

**Get List of All Centres Endpoint:** `{domain}/v1/admin/masterdata/registrationcenters`

**Method:** GET\
\
Once the **Centre ID** is identified, ensure it is saved securely for future reference. This **Centre ID** will be required in subsequent interactions with the CRVS system for processing requests.
