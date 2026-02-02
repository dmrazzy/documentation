# Create the Application ID (AID)

### Overview <a href="#id-7.-create-the-rid" id="id-7.-create-the-rid"></a>

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
