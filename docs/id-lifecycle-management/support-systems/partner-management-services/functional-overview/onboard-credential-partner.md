# Onboard Credential Partner

# New - Credential Partner Onboarding (End User Guide)

New Content

## **1. Overview of Guide**

This section serves as a manual for users who need to register and
configure a Credential\
Partner account. It covers the sequential actions required within the
PMS portal---from initial\
registration to the final submission of a policy request---to enable the
secure issuance of identity\
credentials.

## **2. What is a Credential Partner?**

A Credential Partner is an entity authorized to generate and issue
physical or digital identity\
documents (like ID cards) to citizens. In the MOSIP ecosystem, once an
identity is verified, the\
Credential Partner receives the authorized data packets to produce the
final credential.

## **3. Key Responsibilities of Credential Partner**

- Securely receive and retrieve citizen data packets from the MOSIP
  system immediately following successful UIN generation.
- Map authorized demographic details and biometrics (such as the facial
  photo) onto official physical or digital templates.
- Execute the technical printing of physical ID cards or the generation
  of digital credentials, such as secure PDFs or QR codes.
- Utilize received credentials to synchronize and link the National ID
  (UIN) with external government systems and authorized repositories,
  ensuring compliance with local laws and data policies.

## **4. Who can \'Onboard a Credential Partner\' and What you need to know?**

Onboarding is performed by the Partner and Partner Admin using the PMS
portal.\
Before you begin, ensure you have your organization\'s digital
certificates and have confirmed\
that the Partner Admini has uploaded the necessary Trust Chains.

### **Onboarding Steps - Partner**

4.  **Self-Registration by Partner:** Navigate to the PMS
    portal,complete the registration form for your organization, and log
    in to your account.
5.  **Policy Group Selection by Partner:** Select the appropriate
    \"Credential Partner\" policy group to unlock your specific
    dashboard view.
6.  **CA Certificate Upload by Partner:** Upload your organization\'s CA
    certificate.

**Note**: This requires that the Partner Admin has already uploaded the
Trust Chain (Root and Intermediate certificates).

7.  **Policy Request by Partner:** Initiate a policy request which
    involves three sub-steps:

    3.  Select the required Policy.
    4.  Map the Biometric Extractor Provider Configuration.
    5.  Select the desired Credential Type.

8.  **Policy request Submitted:** Your request is sent to the Partner
    Admin for review and approval.

9.  **Partner Admin Approval:** Once partner admin approves the policy
    request the partner onboarding is complete.

### **Onboarding Steps - Partner Admin**:

10. **Upload Certificate Trust Chain:** Partner admin uploads the
    certificate trust chain (Root, Intermediate and leaf certificates).
11. **Approve Policy Request:** Partner admin will approve or reject the
    policy request Submitted by Credential Partner.
12. **Create Biometric Extractor Provider Configuration:** Partner admin
    should create the required biometric extractor provider
    configuration before the partner create a policy request.

## **Interface Overview**

### Credential Partner - User Flow

**Step 1. Self-Register on PMS as Credential Partner**

13. The Credential Partner can register themselves on MOSIP PMS portal
    by clicking **Register** on the Login Page, a form comes up.

14. Enter the Authentication Partner details:

    11. Partner type (Authentication Partner)
    12. First and Last name
    13. Organization Name
    14. Address, Phone number
    15. Email, Username and password

image1.png


15. **Click** on **Register**, a popup comes up which asks you to
    \'**Choose a Policy Group**\' and seeks you to \'**Agree to Terms
    and Conditions**\' before you can be considered as \'Credential
    Partner.
16. Select the relevant/applicable **Policy Group** on **Select Policy
    Group** popup using **Policy Group** dropdown by reading through
    policy group description in dropdown.

image2.png
    

17. On Submit it will ask you to read through '**Terms and Condition**'
    and having carefully read through it you can agree and accept it.

**Validations**:

- User can select only one **Policy Group** per **Partner Type**.
- Policy selected once cannot be edited later.
- **Terms & Conditions:** Partner consent refers to voluntary and
  informed agreement provided by a partner user on behalf of the Partner
  Organisation, to a specific action or process where the users have a
  clear understanding of what they are consenting to. User consent is
  important to ensure data privacy, where it is compliant to obtain
  explicit consent from partners before collecting, processing, or
  sharing their personal/ organisation level data.
- A detailed description explaining which of their personal and
  organisation data is used and for what purposes it will be used in PMS
  will be informed while seeking user consent.

**Step 2: CA Signed Partner Certificate Upload**

Once registered, user is required to perform upload CA signed Partner
Certificate on behalf of their organisation which would be used to build
a trust store in MOSIP to cryptographically validate that they are from
a trusted organisation to perform authentication of citizens. Also this
certificate is used to encrypt the response shared in e-KYC.

**Note**: Later when required a Partner can also 'Download Certificate'
and 'Re-Upload Certificate' (As the need may be).

**Important**: Before a Partner can upload a 'CA Signed Certificate' it
is prerequisite that the 'Partner Admin' should have already had
uploaded the **Root CA** and **Sub CA** certificates.

22. Credential partner log in to the PMS portal and lands on the
    Dashboard.

image3
 
23. Click on **Partner Certificate** option, Click on the **Upload**
    button to upload the partner certificate signed by CA.

image4

24. Certificate is successfully fetched from local system.

image5

25. Click on **Submit**, Partner Certificate is uploaded successfully.

image6

26. On closing the popup, The user can view the uploaded certificate
    details in the form of a list view.

image7

**Download Certificate**

There is also an option to download initially uploaded CA signed
certificate and also the MOSIP Signed Certificate.

**Re-Upload Certificate**

Re-uploading certificate is required in cases when MOSIP Signed
Certificate gets expired after one year.

**Note:** \'MOSIP Signed Certificate has a validity of 1 year from the
time of Partner Certificate Upload. You must ensure that you re-upload
the partner certificate again so that new MOSIP signed certificate can
be generated and other functionalities such as Request Policy,
Authentication Policies can function.

**Step 3: Partner requests Policy**

27. Credential Partner clicks on the **"Policies"** card from the
    dashboard.

image8

28. Credential partner clicks the "Request Policy" button on the centre
    of page.

image9

**Note:** If partner has submitted policy requests previously, they will
appear in a summary list on this page. To create a new submission, click
the **Request Policy** button will be located in the top-right corner.

29. Requesting a policy for credential partner involves 3 steps:

    17. Add the policy Details: Credential partner selects a required
        policy from the available list as per policy group linked to the
        partner and clicks "Save and Proceed" to continue to next step.

image10

> b\. Map Biometric Extractor Provider Configuration: As second step
> credential partner will map a biometric extractor provider
> configuration to the policy selected in first step. During mapping
> biometric extractor configuration, partner will choose modality and
> will select a configuration from the list and clicks "Save and
> Proceed" to continue to last step.

image11

**Pre-requisite:** Ensure that Partner Admin has created the Biometric
extraction provider configurations. This ensures the correct options are
available in your dropdown menu during the policy request step.
image12
image13

> c\. Map Credential type: As last step credential partner will map a
> credential type to the policy selected in first step. During mapping
> credential type, partner will select a credential type from the
> dropdown list. Partner clicks on the **"Submit"** button to complete
> the policy request.

image14
image15
image16

30. Once the policy request is submitted, it will be added to the queue
    to be approved by Partner Admin.

image17

### Partner Admin - User Flow

**Step 1: Upload Root Certificate**

Partner admin has to upload the root certificate as the primary steps
for any partner onboarding. Please refer the guide here ( [*Keshav
Singh*](https://mosip.atlassian.net/wiki/people/712020:db7f76c3-b494-4ddf-93dc-7036472db6d0?ref=confluence)
Link to the Root Certificate upload guide under Partner admin end user
guide) to know more about the steps.

**Step 2: Create Biometric Extractor Provider Configuration**

Partner admin should create the biometric extraction provider
configuration so that the configurations are available for mapping
during policy request creation by credential partner.

31. Partner admin clicks on the "**Biometric Extractor Provider
    Configurations**" card from the dashboard.

image18

32. Partner admin lands on the "**Biometric Extractor Provider
    Configurations**" list page and clicks on the "Create
    Configurations" button on the top right.

image19

33. Partner admin fills the details required in the from and selects a
    biometric modality from the dropdown.

image20

34. After entering the required details partner admin clicks submit to
    create new biometric extractor provider information and is landed on
    the acknowledgement page.

image21

35. After successful creation of configuration partner admin clicks on
    \'Go Back\' and is landed on the configuration list page with the
    new configuration created listed on the top in the table.

image22

36. Now this is available for mapping when credential partner is
    requesting a new policy.

**Step 3: Approve Policy request Submitted by Credential Partner**

37. Once the policy request is submitted by the credential partner,
    partner admin can view the list of pending request by clicking on
    "Partner-Policy Linking" card.

image23

38. Partner admin can see the list of the request submitted with status
    as "Pending for approval".

image24


39. Partner admin can approve or reject the policy by clicking the
    "Approve/Reject" option from the actions menu.

image25

40. The Partner Admin can review the full details of each policy
    request, including the selected Biometric Extractor configuration
    and the specific Credential Type mappings.

image26

**Note:** Partner admin cannot approve nay policy request that does not
have credential type and biometric extractor provider configurations
added.

41. Clicking on the "Approve" button completes the process and the
    status of the policy is changed to "Active".

image27

The credential partner onboarding is completed.
