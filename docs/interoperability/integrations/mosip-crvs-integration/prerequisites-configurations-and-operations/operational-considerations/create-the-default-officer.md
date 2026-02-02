# Create the Default Officer

### Overview <a href="#id-3.-create-the-default-officer" id="id-3.-create-the-default-officer"></a>

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
