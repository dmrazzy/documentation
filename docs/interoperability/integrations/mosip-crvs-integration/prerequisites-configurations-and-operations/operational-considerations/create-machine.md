# Create Machine

#### Overview <a href="#id-5.-create-machine" id="id-5.-create-machine"></a>

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
