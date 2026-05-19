# Onboard Manual Adjudication Partner

## Overview

This guide shows how Partner Admins can **Onboard a Manual Adjudication Partner (MA Partner) in PMS**. Onboarding includes **Creating a Manual Adjudication Partner**, **Managing Manual Adjudication Partner's Partner Certificates**, **Linking a Data Share Policy** using **PMS portal**.

## What is a Manual Adjudication Partner

A Manual Adjudication Partner is an authorized entity - either a government department or a specialized service provider - that employs biometric experts. They act as the final decision-makers whenever [ABIS Partner](abis-partner-onboarding-by-partner-admin.md) identifies a possible duplicate identity that requires human judgment to resolve.

## Key Responsibilities of Manual Adjudication Partner

* Receive and manage cases flagged as potential identity duplicates.
* Perform side-by-side human expert comparisons of fingerprints, iris scans, and facial photos.
* Distinguish between true duplicates (fraud) and "false positives" (two different people who look similar to a machine).
* Issue a final verdict of **Match** to reject a duplicate or **No Match** to approve a unique applicant.
* Flag cases where biometric quality is too low for a human to make a reliable decision.

## Who can 'Onboard a Manual Adjudication Partner' and What you need to know?

Partner Admins with the appropriate credentials can onboard Manual Adjudication Partners in PMS. Before starting, ensure CA certificates are uploaded and required policy groups are created.

* You should be logged in with **Partner Admin** credentials (Role: Partner Admin).
* Root and Intermediate CA certificates should have already been uploaded to the PMS **Certificate Trust Store** (these are required before uploading partner CA Signed Certificates).
* Policy Manager should already have created the required **data share** **policy** (such that they can be selected later). See the Policies creation docs- [_Policy Manager_](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/functional-overview/policy-manager#policies-has-following-tabs)

## Interface Overview

You (As a Partner Admin) can log into the PMS portal using your credentials. After login, you see the dashboard or left navigation panel, where you click the Partners card to access the 'List of Partners' tabular view.

1. Log into the PMS portal with your Partner Admin account.

![](../../../../.gitbook/assets/pms-b5-map-image1.png)

2. Click on **Partners** card (from the left navigation panel or dashboard itself). You will be redirected to 'List of Partners' page (tabular view).

![](../../../../.gitbook/assets/pms-b5-map-image2.png)

## Onboarding Manual Adjudication Partners

Onboarding includes following:

* **Creating a Manual Adjudication Partner**
* **Manage Manual Adjudication Partner's Partner Certificate**
* **Manual Adjudication Partner Policy Linking**

### Create a Manual Adjudication Partner

1. Go to Dashboard > Partner, A 'List View' appears which shows all the partners.
2. Click **Create Partner** button placed on top-right (if partner records already exist) or positioned at the centre of the screen (if no records exist).

![](../../../../.gitbook/assets/pms-b5-map-image3.png)

3. Enter details in the **Create Partner** form:
   1. Select Partner Type as '**Manual Adjudication Partner'** from the dropdown.
   2. Enter Address, Organization Name, Email Address, and other mandatory fields and select a policy group.

{% hint style="info" %}
**Note**: Ensure that the Organization name matches the one in the 'CA Signed Certificate' that will be uploaded later.
{% endhint %}

![](../../../../.gitbook/assets/pms-b5-map-image4.png)

4. Click **Save/Submit**. A confirmation message appears on successful creation.

![](../../../../.gitbook/assets/pms-b5-map-image5.png)



{% hint style="info" %}
**Note**: On the 'Success/Confirmation' screen itself, you are provided with an option to upload CA-Signed partner certificate or return to 'Home page'.
{% endhint %}



5. Click **Upload Certificate** to proceed with uploading the 'CA Signed Partner Certificate'.

![](../../../../.gitbook/assets/pms-b5-map-image6.png)

### Upload'CA Signed Partner Certificate' (First time upload)

Either you can upload the partner certificate right after Manual Adjudication partner creation as explained in Create a Manual Adjudication Partner(Link to Create Manual Adjudication Partner) or you can do it later from the **'List of Partners'** page.

1. Go to Dashboard > Manual Adjudication Partner, A 'List View' appears which shows all the partners.
2. Locate the newly created Manual Adjudication Partner(inactive status) and choose **Upload Certificate** from the action menu, The **Upload Partner Certificate** popup opens.

![](../../../../.gitbook/assets/pms-b5-map-image7.png)

3. Click the upload area and select the 'CA Signed Certificate' file from local folder in _.cer_ or _.pem_ format.

![](../../../../.gitbook/assets/pms-b5-map-image8.png)

4. Verify the certificate details by clicking **Submit**.

![](../../../../.gitbook/assets/pms-b5-map-image9.png)

5. On success, you (admin) receives a confirmation and the partner row should show the certificate upload date/status along with status as 'ACTIVE'.

![](../../../../.gitbook/assets/pms-b5-map-image10.png)



{% hint style="info" %}
**Note:** If Root/Intermediate CA is missing, the system will reject the upload. Therefore ensure that the CA certificates are uploaded first in Certificate Trust Store.
{% endhint %}

### Re-Upload Partner Certificate (Replacing an existing certificate)

You can replace an existing partner certificate when it is expiring or has been re-issued. This ensures the partner remains compliant and can continue to use PMS services without interruption.

1. Go to Dashboard > Partners, A 'List View' appears which shows all the partners.
2. From Partners table action menu, select **Re-Upload Certificate**.
3. Follow the same upload flow as in [_Upload 'CA Signed Partner Certificate' (First time upload)_](https://docs.mosip.io/1.2.0/id-lifecycle-management/support-systems/partner-management-services/functional-overview/misp-partner-onboarding#upload-ca-signed-partner-certificate-first-time-upload) and click **Submit**.
4. The new certificate details and updated certificate status is displayed in the Partners list.

### Partner Policy Linking (requesting and approving Manual Adjudication policies)

1. Go to **Partner Policy Linking** (dashboard card or from left side menu).

![](../../../../.gitbook/assets/pms-b5-map-image11.png)

2. Click '**Request Policy'** to submit a policy request for the selected **Manual Adjudication Partner ID**. Once the Partner ID is selected, the **Policy Group** associated during partner creation is **auto-populated**, after which an applicable policy can be selected and submitted.

{% hint style="info" %}
**Note:** For **Manual Adjudication Partners**, only **Data Share Policies** must be selected.The available policies are automatically filtered based on the Policy Group linked to the selected Partner ID.
{% endhint %}

![](../../../../.gitbook/assets/pms-b5-map-image12.png)

![](../../../../.gitbook/assets/pms-b5-map-image13.png)

3. On clicking **Submit**, the policy request is submitted successfully with an option to **'Approve'** the policy right Away.

![](../../../../.gitbook/assets/pms-b5-map-image14.png)

4. If user choses not to **'Approve'** the policy right after submitting the request the request policy is added to the partner policy linking list page with status **'Pending for Approval'**

![](../../../../.gitbook/assets/pms-b5-map-image15.png)

5. Approve the requests after requesting policy, by navigating to the list of all policy requests and selecting 'Approve/Reject' from the action menu against each request. (View details to inspect the mapping and comments before acting).

![](../../../../.gitbook/assets/pms-b5-map-image16.png)

![](../../../../.gitbook/assets/pms-b5-map-image17.png)

6. Upon successful approval the policy request is approved and policy request status is changed to **'Approved'**

![](../../../../.gitbook/assets/pms-b5-map-image18.png)

**Success check:** Approved partner-policy links will appear in the partner's policy list.

### Deactivate Partner

You can deactivate the entire Manual Adjudication Partner (prevents future requests & license generation).

1. Go to **Partners**. From Partners list, open the action menu → **Deactivate Partner**.

![](../../../../.gitbook/assets/pms-b5-map-image19.png)

2. Upon confirmation to deactivate the status is changed to **'Deactivated'** and note the consequences (deactivated Manual Adjudication partner cannot request policies )

![](../../../../.gitbook/assets/pms-b5-map-image20.png)

3. The partner is successfully deactivated.

![](../../../../.gitbook/assets/pms-b5-map-image21.png)

&#x20;
