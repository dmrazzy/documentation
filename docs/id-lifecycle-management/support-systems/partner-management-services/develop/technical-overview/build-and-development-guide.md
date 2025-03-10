# Build and Development Guide

This guide contains all the information required for successful deployment and running of Partner Management Portal. It includes information about the Database and roles.

### DB scripts

Partner Management Service DB Scripts to be run: [DB scripts](https://github.com/mosip/partner-management-services/tree/develop-pmp-revamp/db\_scripts/mosip\_pms)

### Keycloak Roles

`mosip-pms-client` needs to have below roles in keycloak:

* `CREATE_SHARE`\`
* `DEVICE_PROVIDER`
* `PARTNER`
* `PARTNER_ADMIN`
* `PMS_ADMIN`
* `PMS_USER`
* `PUBLISH_APIKEY_APPROVED_GENERAL`
* `PUBLISH_APIKEY_UPDATED_GENERAL`
* `PUBLISH_CA_CERTIFICATE_UPLOADED_GENERAL`
* `PUBLISH_MISP_LICENSE_GENERATED_GENERAL`
* `PUBLISH_MISP_LICENSE_UPDATED_GENERAL`
* `PUBLISH_OIDC_CLIENT_CREATED_GENERAL`
* `PUBLISH_OIDC_CLIENT_UPDATED_GENERAL`
* `PUBLISH_PARTNER_UPDATED_GENERAL`
* `PUBLISH_POLICY_UPDATED_GENERAL`
* `REGISTRATION_PROCESSOR`
* `SUBSCRIBE_CA_CERTIFICATE_UPLOADED_GENERAL`
* `ZONAL_ADMIN`
* `view-users (from realm-management roles)`

### **Config Changes:**

Add below property to partner-management-default.properties file in mosip-config repository to Deploy PMS Revamp 1.3.0-DP.1 release in your env.

```
## This property is used by kernel-authcodeflowproxy-api to check request is coming from allowed urls not.
auth.allowed.urls=https://${mosip.pmp.host}/,https://${mosip.pmp.reactjs.ui.host}/
```




# New Language Support
# New Language Support

## Overview

This guide provides step-by-step instructions for adding support for a
new language in PMS application that uses react-i18next for
internationalization. The i18n.js file is already configured to
dynamically load language files based on the selected locale. Currently,
the application supports English, French, and Arabic. For demonstration,
we will add Spanish as an additional language.

## Repository

The PMS React application source code is available at: [PMP Revamp UI
Repository](https://github.com/mosip/partner-management-portal/tree/release-1.2.2.x/pmp-revamp-ui).

## Steps to Add a New Language

### 1. Add a New Translation File

Each language in the application is stored as a separate JSON file
inside the /pmp-revamp-ui/public/i18n directory. To add Spanish (es),
create a new translation file:

es.json

Open es.json and add the translations required, for example:

{

\"dashboard\": {

\"welcomeMsg\": \"Bienvenido {{firstName}} {{lastName}}\",

\"accountStatus\": \"Estado de la cuenta\"

}

}

### 2. Add Language Locale in Keycloak

We use Keycloak for authentication, and users select their preferred
language during login. Once logged in, the selected language is picked
up by the i18n bundle. This requires updating Keycloak's supported
locales.

#### Steps to Enable Spanish in Keycloak

1.  **Log in to Keycloak Admin Console** as an administrator.

2.  **Navigate to Realm Settings**:

    -   Select the appropriate realm.

    -   Go to the \"Realm Settings\" section.

    -   Click on the **\"Themes\"** tab.

> ![](/Users/keshavsingh/Office/pmp/new-language-support/media/media/image1.png)

3.  **Add Spanish to Supported Locales**:

    -   Ensure that \"Internationalization\" is enabled.

    -   In the \"Supported Locales\" field, add es (for Spanish) to the
        list.

    -   Click \"Save\" to apply changes.

    -   ![](/Users/keshavsingh/Office/pmp/new-language-support/media/media/image2.png)

4.  **Verify Language Selection in Keycloak Login Page:**

    -   Navigate to the Keycloak login page.

    -   Ensure that Spanish appears as an option in the language
        selection dropdown.

    -   ![](/Users/keshavsingh/Office/pmp/new-language-support/media/media/image3.png)

### 3. Rebuild the Application

After adding the new language bundle, you must **rebuild the code** to
ensure the new translations are correctly loaded into the application.

#### Steps to Rebuild the React Application

1.  Open a terminal or command prompt.

2.  Navigate to your React project directory:

> cd partner-management-portal\\pmp-revamp-ui

3.  Install dependencies (if not already installed):

> npm install

4.  Build the project:

> npm run build

5.  Deploy the updated build to your hosting environment.

### 4. Verify Language Support in PMS Application

1.  Open the application in a browser.

    -   Select Spanish from the language options on the Keycloak login
        page.

    -   Confirm that the UI displays the correct Spanish translations
        after logging in.

    -   ![](/Users/keshavsingh/Office/pmp/new-language-support/media/media/image4.png)

By following these steps, you can easily add support for any new
language in your application.

