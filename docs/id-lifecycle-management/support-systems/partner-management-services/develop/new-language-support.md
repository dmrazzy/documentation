# New Language Support

## New Language Support

### Overview

This guide provides step-by-step instructions for adding support for a new language in PMS application that uses react-i18next for internationalization. The i18n.js file is already configured to dynamically load language files based on the selected locale. Currently, the application supports English, French, and Arabic. For demonstration, we will add Spanish as an additional language.

### Repository

The PMS React application source code is available at: [PMP Revamp UI Repository](https://github.com/mosip/partner-management-portal/tree/release-1.2.2.x/pmp-revamp-ui).

### Steps to Add a New Language

#### 1. Add a New Translation File

Each language in the application is stored as a separate JSON file inside the /pmp-revamp-ui/public/i18n directory. To add Spanish (es), create a new translation file:

es.json

Open es.json and add the translations required, for example:

{

"dashboard": {

"welcomeMsg": "Bienvenido \{{firstName\}} \{{lastName\}}",

"accountStatus": "Estado de la cuenta"

}

}

#### 2. Add Language Locale in Keycloak

We use Keycloak for authentication, and users select their preferred language during login. Once logged in, the selected language is picked up by the i18n bundle. This requires updating Keycloak's supported locales.

**Steps to Enable Spanish in Keycloak**

1. **Log in to Keycloak Admin Console** as an administrator.
2. **Navigate to Realm Settings**:
   * Select the appropriate realm.
   * Go to the "Realm Settings" section.
   * Click on the **"Themes"** tab.

> <img src="../../../../../Users/keshavsingh/Office/pmp/new-language-support/media/media/image1.png" alt="" data-size="original">

3. **Add Spanish to Supported Locales**:
   * Ensure that "Internationalization" is enabled.
   * In the "Supported Locales" field, add es (for Spanish) to the list.
   * Click "Save" to apply changes.
   *

       ![](../../../../../Users/keshavsingh/Office/pmp/new-language-support/media/media/image2.png)
4. **Verify Language Selection in Keycloak Login Page:**
   * Navigate to the Keycloak login page.
   * Ensure that Spanish appears as an option in the language selection dropdown.
   *

       ![](../../../../../Users/keshavsingh/Office/pmp/new-language-support/media/media/image3.png)

#### 3. Rebuild the Application

After adding the new language bundle, you must **rebuild the code** to ensure the new translations are correctly loaded into the application.

**Steps to Rebuild the React Application**

1. Open a terminal or command prompt.
2. Navigate to your React project directory:

> cd partner-management-portal\pmp-revamp-ui

3. Install dependencies (if not already installed):

> npm install

4. Build the project:

> npm run build

5. Deploy the updated build to your hosting environment.

#### 4. Verify Language Support in PMS Application

1. Open the application in a browser.
   * Select Spanish from the language options on the Keycloak login page.
   * Confirm that the UI displays the correct Spanish translations after logging in.
   *

       ![](../../../../../Users/keshavsingh/Office/pmp/new-language-support/media/media/image4.png)

By following these steps, you can easily add support for any new language in your applica
