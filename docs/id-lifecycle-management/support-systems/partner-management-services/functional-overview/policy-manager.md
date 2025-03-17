# Policy Manager

**Using Keycloak to allocate/get 'Partner Admin' and/or 'Policy Manager'**

After registration you need to go to keycloak to enable roles.

1. Go to keycloak and search your 'User-Name' in Users tab.

<figure><img src="../../../../.gitbook/assets/temp-pms-admin-image2.png" alt=""><figcaption></figcaption></figure>

2. Go to the **Role Mapping** tab.

<figure><img src="../../../../.gitbook/assets/temp-pms-admin-image3.png" alt=""><figcaption></figcaption></figure>

3. In the **Available Roles** section, select **Partner Admin** or **Policy Manager**, click **Add** to move the selected role to the **Assigned Roles** list.

<figure><img src="../../../../.gitbook/assets/temp-pms-admin-image4.png" alt=""><figcaption></figcaption></figure>

4. You can now log in to the **PMS** portal with the same user credentials and you will have access to the **Admin Dashboard**.

<figure><img src="../../../../.gitbook/assets/temp-pms-admin-image5.png" alt=""><figcaption></figcaption></figure>

**Note:** Add **Policy Manager** role when you want that the 'Policies-Card'/ 'Priviledge' should also get enabled for you and turn you into a 'Policy Manager' as well.

#### Registering as Policy Manager

By following the above steps (1-4) in keycloak, the admin can also configure **Policy Manager** role to enable and manage **Policies** card as shown in the dashboard below:

<figure><img src="../../../../.gitbook/assets/temp-pms-admin-image6.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Note**:

If only 'Policy Manager' role is configured in keycloak, then the user will still be able to access the portals as a normal partner. Hence both; 'Partner Admin' & 'Policy Manager' roles are necessary to access all the cards/privileges above.
{% endhint %}

{% hint style="success" %}
**Important**:&#x20;

After configuring the roles and if PMS portal is still logged in, make sure to logout and login again for the roles to get updated.
{% endhint %}











