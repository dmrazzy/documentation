# CP

# Improved
**Release Name**: Android Registration Client GA Release v1.0.0

**Release version**: 1.0.0

**Support**: GA Release

**Release Date**: 1st December, 2025

## **Overview**

The Android Registration Client is a tablet-based application that
delivers a mobile-friendly version of the traditional desktop
[Registration
Client](https://docs.mosip.io/1.2.0/modules/registration-client). It is
designed to work across all Android devices and supports the mobility
requirements of countries adopting MOSIP.

## Features and Major Highlights

**Version 1.0.0** is the General Availability (GA) release and introduces a production‑ready Android experience with the following major capabilities (each backed by recently added documentation and configuration properties):

1. **GPS Location Capture & Center Distance Compliance**  
  Captures precise latitude/longitude at packet creation and computes distance to the mapped registration center to support geo‑fencing, audit logging, and operational risk reduction. If the distance exceeds a configured threshold the system flags the packet for review.  
  - Config keys: `mosip.registration.gps_device_enable_flag` (enable / turn off), `mosip.registration.distance.from.machine.to.center` (meters)  
  - Offline behavior: Coordinates queued and synced when connectivity resumes  
  - Privacy: Location stored only as packet metadata for audit trails  
  - Docs: [Features: GPS Location](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features#gps-location), [Configuration Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-configuration)  
  - Value: Ensures operational integrity and center compliance

2. **Applicant Biometric Correction Flow (Temporary ID Recovery)**  
  Enables operators to issue a temporary ID when initial biometric capture fails (for example quality below threshold or a physical exception). The dedicated `BIOMETRIC_CORRECTION` process flow enables later re‑capture before AID issuance. Consent, conditional biometric attributes (for example face + iris for infants), and exception photo proof follow the new UI Spec templates.  
  - Flow ID: `BIOMETRIC_CORRECTION` (UI spec process JSON)  
  - Conditional attributes: Age‑based rules via `conditionalBioAttributes`  
  - Exception handling: `exceptionPhotoRequired` ensures traceability  
  - Docs: [Features: Biometrics Correction](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features#biometrics-correction), [UI Spec: Process & Field](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/develop/ui-spec-documentation#process-task-spec-json-template)  
  - Value: Reduces rejection rates and improves enrollment completion

3. **Unified Settings Console (Device, Scheduled Jobs, Global Config)**  
  Consolidates operational controls into a single Settings area:  
  - Device Settings: Machine spec, app version, network health indicators  
  - Scheduled Jobs: Visibility into job status, last run, next run and manual trigger eligibility  
  - Global Config: Read‑only view of centrally managed parameters (with future extension capability)  
  A new Settings Spec JSON template (added in documentation) standardizes multilingual labels, ordering, access control, and icon metadata.  
  - Docs: [Features: Settings](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features#settings), [UI Spec: Settings Spec Template](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/develop/ui-spec-documentation#settings-spec-json-template), [Developer Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-developer-guide)  
  - Value: Improves operator efficiency and troubleshooting speed

4. **Responsive Landscape Orientation Support**  
  Dynamic layout reflow for horizontal device use: alignment groups render side‑by‑side, biometric capture panels expand for better framing, and reduced scrolling improves form completion speed. All critical validation and mandatory markers persist across orientation changes.  
  - Strategy: CSS/FXML adaptive container + alignment group horizontal stacking  
  - Docs: [Features: Landscape Mode](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features#landscape-mode)  
  - Value: Enhances ergonomics for desk‑mounted device deployments

5. **Adaptive Small Screen (Phone) Experience**  
  Optimized UI for narrow viewports: collapsed section headers, minimized padding, prioritized mandatory fields, scalable font sizing, and simplified biometric preview cards ensure usability on mobile devices without feature loss.  
  - Layout: Progressive disclosure for secondary metadata (for example optional documents)  
  - Performance: Reduced image payloads on constrained bandwidth  
  - Docs: [Features: Phone Screen Support](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features#phone-screen-support)  
  - Value: Expands reach to field operations in mobility‑first contexts

6. **Configurable Auto Logout & Session Refresh**  
  Enforces security and compliance by automatically logging out inactive users and refreshing credentials (re‑authentication) on long‑running sessions. Differentiated idle and refreshed login timers reduce stale privilege windows while maintaining operator productivity.  
  - Config keys: `mosip.registration.idle_time` (idle timeout), `mosip.registration.refreshed_login_time` (credential refresh interval)  
  - Behavior: Grace warning before forced logout, queued in‑progress form persists locally  
  - Docs: [Features: Auto Logout](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features#auto-logout), [Configuration Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-configuration)  
  - Value: Strengthens session hygiene and reduces unauthorized access risk

> Documentation links (Feature Guide, Configuration Guide, UI Spec, Developer Guide) in PR #1014 include the new GPS, Biometric Correction, Settings Spec template, Auto Logout, Landscape, and Phone optimization sections with configuration key references.

**Note**: Compatible with [MOSIP version
1.2.0](https://docs.mosip.io/1.2.0/releases/release-notes)

## **Repository Released**

  -----------------------------------------------------------------------
  **Repositories**                          **Tags Released**
  ----------------------------------------- -----------------------------
  android-registration-client               **Insert the version**

  -----------------------------------------------------------------------

## **Features Delivered:**

  ----------------------------------------------------------------------------------------------------------------
  **Jira Ticket Link**                                      **Description**
  --------------------------------------------------------- ------------------------------------------------------
  [RCF-1281](https://mosip.atlassian.net/browse/RCF-1281)   Performance and Stability Testing of Android
                                                            Registration Client Closed

  [RCF-1185](https://mosip.atlassian.net/browse/RCF-1185)   As an Operator, when I create any packet, my GPS
                                                            location should also be sent as meta data

  [RCF-1126](https://mosip.atlassian.net/browse/RCF-1126)   Security Testing

  [RCF-784](https://mosip.atlassian.net/browse/RCF-784)     As an Operator/ Supervisor, my system should auto
                                                            logout from ARC if it is idle for a long period of
                                                            time

  [RCF-502](https://mosip.atlassian.net/browse/RCF-502)     As an Operator, I should be able to correct the
                                                            Applicant's Biometric

  [RCF-472](https://mosip.atlassian.net/browse/RCF-472)     As an Operator, I should be able to run Android
                                                            Registration Client on small screen like phone screen

  [RCF-471](https://mosip.atlassian.net/browse/RCF-471)     As an Operator, I should be able to run Android
                                                            Registration Client in landscape mode

  [RCF-93](https://mosip.atlassian.net/browse/RCF-93)       As an Operator, I should be able to access Device
                                                            Settings

  [RCF-92](https://mosip.atlassian.net/browse/RCF-92)       As an Operator, I should be able to access Global
                                                            Config Settings

  [RCF-91](https://mosip.atlassian.net/browse/RCF-91)       As an Operator, I should be able to access Scheduled
                                                            Jobs Settings

  [RCF-40](https://mosip.atlassian.net/browse/RCF-40)       As an Operator, I should be able to access Settings
  ----------------------------------------------------------------------------------------------------------------

## **Bugs Fixed:**

  ----------------------------------------------------------------------------------------------------------
  **Jira Ticket Link**                                      **Description**
  --------------------------------------------------------- ------------------------------------------------
  [RCF-1272](https://mosip.atlassian.net/browse/RCF-1272)   The database migration should support a seamless
                                                            schema upgrade.

  [RCF-1266](https://mosip.atlassian.net/browse/RCF-1266)   In the "Scheduled Jobs Settings" screen of ARC,
                                                            the page cannot be scrolled.

  [RCF-1262](https://mosip.atlassian.net/browse/RCF-1262)   In ARC, packets created with PRIDs are not being
                                                            processed.

  [RCF-1260](https://mosip.atlassian.net/browse/RCF-1260)   In ARC, the "Authenticate" button on the
                                                            authentication page is not clickable for
                                                            exception packets.

  [RCF-1258](https://mosip.atlassian.net/browse/RCF-1258)   In ARC, the "Global Config Settings" heading is
                                                            not displayed on the global configuration page.

  [RCF-1246](https://mosip.atlassian.net/browse/RCF-1246)   ARC requires license compliance updates.

  [RCF-1238](https://mosip.atlassian.net/browse/RCF-1238)   ARC has privilege escalation issues. On Hold -
                                                            Testing

  [RCF-1237](https://mosip.atlassian.net/browse/RCF-1237)   ARC does not mandate operator permissions.

  [RCF-1236](https://mosip.atlassian.net/browse/RCF-1236)   ARC contains an issue involving the use of
                                                            setAccessible(true).

  [RCF-1235](https://mosip.atlassian.net/browse/RCF-1235)   ARC has a vulnerability due to the insecure
                                                            binding mechanism used with Log4j. - Testing

  [RCF-1224](https://mosip.atlassian.net/browse/RCF-1224)   In ARC, when a user logs in with an operator
                                                            role, they are unable to start the ARC and
                                                            encounter various error messages.
  ----------------------------------------------------------------------------------------------------------

## 

## 

## **Whole list**

## 

  ----------------------------------------------------------------------------------------------------------------
  **Jira Ticket Link**                                            **Description**
  --------------------------------------------------------------- ------------------------------------------------
  [RCF-1272](https://mosip.atlassian.net/browse/RCF-1272)         The database migration should support a seamless
                                                                  schema upgrade.

  [RCF-1266](https://mosip.atlassian.net/browse/RCF-1266)         In the "Scheduled Jobs Settings" screen of ARC,
                                                                  the page cannot be scrolled.

  [RCF-1262](https://mosip.atlassian.net/browse/RCF-1262)         In ARC, packets created with PRIDs are not being
                                                                  processed.

  [RCF-1260](https://mosip.atlassian.net/browse/RCF-1260)         In ARC, the "Authenticate" button on the
                                                                  authentication page is not clickable for
                                                                  exception packets.

  [RCF-1258](https://mosip.atlassian.net/browse/RCF-1258)         In ARC, the "Global Config Settings" heading is
                                                                  not displayed on the global configuration page.

  [RCF-1246](https://mosip.atlassian.net/browse/RCF-1246)         ARC requires license compliance updates.

  [RCF-1238](https://mosip.atlassian.net/browse/RCF-1238)         ARC has privilege escalation issues. On Hold -
                                                                  Testing

  [RCF-1237](https://mosip.atlassian.net/browse/RCF-1237)         ARC does not mandate operator permissions.

  [RCF-1236](https://mosip.atlassian.net/browse/RCF-1236)         ARC contains an issue involving the use of
                                                                  setAccessible(true).

  [RCF-1235](https://mosip.atlassian.net/browse/RCF-1235)         ARC has a vulnerability due to the insecure
                                                                  binding mechanism used with Log4j. - Testing

  [RCF-1224](https://mosip.atlassian.net/browse/RCF-1224)         In ARC, when a user logs in with an operator
                                                                  role, they are unable to start the ARC and
                                                                  encounter various error messages.

  [RCF-1196](https://mosip.atlassian.net/browse/RCF-1196)         In ARC, the Attempt button should turn green and
                                                                  the captured biometrics should show a green tick
                                                                  once the biometric score crosses the threshold.

  [RCF-1177](https://mosip.atlassian.net/browse/RCF-1177)         Security testing found weak hash usage (SHA-1
                                                                  and MD5) in ARC.

  [RCF-1114](https://mosip.atlassian.net/browse/RCF-1114)         In RCF, under poor network conditions, some
                                                                  batch jobs succeed while others fail during
                                                                  application synchronization.

  [RCF-1107](https://mosip.atlassian.net/browse/RCF-1107)         In RCF's new registration flow, the biometrics
                                                                  screen appears blank when navigated from the
                                                                  document upload page of the Update UIN flow.

  [RCF-1106](https://mosip.atlassian.net/browse/RCF-1106)         In RCF's Update UIN flow, the selected identity
                                                                  proof document disappears from the dropdown when
                                                                  navigating back and returning to the document
                                                                  upload screen.

  [RCF-1099](https://mosip.atlassian.net/browse/RCF-1099)         Java modules in ARC require unit test cases.
                                                                  Testing

  [RCF-1098](https://mosip.atlassian.net/browse/RCF-1098)         In ARC, updating the UIN using an NRCID fails at
                                                                  the VALIDATE_PACKET stage. - On Hold - Testing

  [RCF-1082](https://mosip.atlassian.net/browse/RCF-1082)         In ARC, Update UIN fails with an NRCID and shows
                                                                  the error "Packet validation failed RPR-PVM-012
                                                                  → Invalid UIN -- Invalid UIN." On Hold - Dev

  [RCF-1077](https://mosip.atlassian.net/browse/RCF-1077)         The guidelines displayed on the language
                                                                  selection screen should be removed. In Progress

  [RCF-1003](https://mosip.atlassian.net/browse/RCF-1003)         In ARC, the applicant details page under pending
                                                                  approval cannot be scrolled from top to bottom.

  [RCF-1000](https://mosip.atlassian.net/browse/RCF-1000)         In ARC, pre-registration data does not always
                                                                  refresh when attempting to fetch another
                                                                  pre-registration AID.

  [RCF-968](https://mosip.atlassian.net/browse/RCF-968)           In ARC, the "Submit" button on the pending
                                                                  approvals page should be renamed to
                                                                  "Authenticate."

  [RCF-942](https://mosip.atlassian.net/browse/RCF-942)           In ARC, the image displayed on the
                                                                  authentication screen is inappropriate and needs
                                                                  to be replaced.

  [RCF-933](https://mosip.atlassian.net/browse/RCF-933)           In ARC, the Comments and Exception Type buttons
                                                                  are not removed from the Operator Onboarding and
                                                                  Update Operator Onboarding screens.

  [RCF-891](https://mosip.atlassian.net/browse/RCF-891)           In ARC's Manage Applications page, the "Created"
                                                                  and "Synched" options should be removed from the
                                                                  Client Status filter.

  [RCF-885](https://mosip.atlassian.net/browse/RCF-885)           In ARC, mandatory fields are highlighted on the
                                                                  document upload page even before submission.

  [RCF-883](https://mosip.atlassian.net/browse/RCF-883)           In ARC's portrait mode, the SNO, date, and
                                                                  operator ID are not properly aligned on the
                                                                  pending approvals page.

  [RCF-858](https://mosip.atlassian.net/browse/RCF-858)           In ARC, authentication fails for
                                                                  pre-registration data when the uploaded document
                                                                  type is a PDF.

  [RCF-857](https://mosip.atlassian.net/browse/RCF-857)           In ARC, the "Download Packet" and "Go to Home"
                                                                  buttons are not properly aligned in the French
                                                                  language interface.

  [RCF-830](https://mosip.atlassian.net/browse/RCF-830)           In ARC, when English, Tamil, or Kannada is
                                                                  selected as the demographic language, the "Menu"
                                                                  button moves out of the screen in portrait mode.

  [RCF-755](https://mosip.atlassian.net/browse/RCF-755)           In ARC, the server status does not progress
                                                                  beyond "Packet Receiver" even after UIN
                                                                  generation.

  [RCF-742](https://mosip.atlassian.net/browse/RCF-742)           In RCF, the "Introducer Details Biometrics"
                                                                  section appears for adult packet creation in the
                                                                  preview/acknowledgement screen. Re-opened

  [RCF-363](https://mosip.atlassian.net/browse/RCF-363)           In RCF, the error "Invalid machine Spec ID
                                                                  found" appears while logging into the
                                                                  registration client application.

  [RCF-192](https://mosip.atlassian.net/browse/RCF-192)           In the Android registration client,
                                                                  synchronization fails intermittently.

  [MOSIP-35259](https://mosip.atlassian.net/browse/MOSIP-35259)   In Reg-proc, the error "12 - referenceId not
                                                                  found (in ABIS)" occurs during the
                                                                  DEMOGRAPHIC_VERIFICATION stage.
  ----------------------------------------------------------------------------------------------------------------

## **Documentation**

[Feature
Documentation](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features)

[Developer
Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-developer-guide)

[UI Specification
Documentation](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/develop/ui-spec-documentation)

[Configuration
Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-configuration)

[Android Registration User
Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-user-guide)




# ARC 1.0.0 GA Release

**Release Name**: Android Registration Client GA Release v1.0.0

**Release version**: 1.0.0

**Support**: GA Release

**Release Date**: 1st December, 2025

## **Overview**

The Android Registration Client is a tablet-based application that
delivers a mobile-friendly version of the traditional desktop
[Registration
Client](https://docs.mosip.io/1.2.0/modules/registration-client). It is
designed to work across all Android devices and supports the mobility
requirements of countries adopting MOSIP.

## Features and Major Highlights

**Version 1.0.0** of the Android Registration Client is the GA release,
covering features listed below:

1.  **GPS Tracking:** Tracks the location where a registration packet is
    created and measures its distance from the device's mapped
    registration center.

2.  **Applicant Biometric Correction:** Allows issuing a temporary ID
    when biometric capture fails, enabling the applicant to return for
    recapture before an AID is generated.

3.  **Settings:** Provides access to device details, scheduled job
    configurations, and global/local configuration settings within the
    ARC.

4.  **Support for Landscape Mode:** Allows the Android Registration
    Client to function seamlessly in landscape orientation.

5.  **Support for Phone Screens:** Optimizes the Android Registration
    Client for effective use on smaller mobile screens.

6.  **Auto Logout:** Automatically logs out the user after a
    configurable period of inactivity for security.

**Note**: Compatible with [MOSIP version
1.2.0](https://docs.mosip.io/1.2.0/releases/release-notes)

## **Repository Released**

  -----------------------------------------------------------------------
  **Repositories**                          **Tags Released**
  ----------------------------------------- -----------------------------
  android-registration-client               **Insert the version**

  -----------------------------------------------------------------------

## **Features Delivered:**

  ----------------------------------------------------------------------------------------------------------------
  **Jira Ticket Link**                                      **Description**
  --------------------------------------------------------- ------------------------------------------------------
  [RCF-1281](https://mosip.atlassian.net/browse/RCF-1281)   Performance and Stability Testing of Android
                                                            Registration Client Closed

  [RCF-1185](https://mosip.atlassian.net/browse/RCF-1185)   As an Operator, when I create any packet, my GPS
                                                            location should also be sent as meta data

  [RCF-1126](https://mosip.atlassian.net/browse/RCF-1126)   Security Testing

  [RCF-784](https://mosip.atlassian.net/browse/RCF-784)     As an Operator/ Supervisor, my system should auto
                                                            logout from ARC if it is idle for a long period of
                                                            time

  [RCF-502](https://mosip.atlassian.net/browse/RCF-502)     As an Operator, I should be able to correct the
                                                            Applicant's Biometric

  [RCF-472](https://mosip.atlassian.net/browse/RCF-472)     As an Operator, I should be able to run Android
                                                            Registration Client on small screen like phone screen

  [RCF-471](https://mosip.atlassian.net/browse/RCF-471)     As an Operator, I should be able to run Android
                                                            Registration Client in landscape mode

  [RCF-93](https://mosip.atlassian.net/browse/RCF-93)       As an Operator, I should be able to access Device
                                                            Settings

  [RCF-92](https://mosip.atlassian.net/browse/RCF-92)       As an Operator, I should be able to access Global
                                                            Config Settings

  [RCF-91](https://mosip.atlassian.net/browse/RCF-91)       As an Operator, I should be able to access Scheduled
                                                            Jobs Settings

  [RCF-40](https://mosip.atlassian.net/browse/RCF-40)       As an Operator, I should be able to access Settings
  ----------------------------------------------------------------------------------------------------------------

## **Bugs Fixed:**

  ----------------------------------------------------------------------------------------------------------
  **Jira Ticket Link**                                      **Description**
  --------------------------------------------------------- ------------------------------------------------
  [RCF-1272](https://mosip.atlassian.net/browse/RCF-1272)   The database migration should support a seamless
                                                            schema upgrade.

  [RCF-1266](https://mosip.atlassian.net/browse/RCF-1266)   In the "Scheduled Jobs Settings" screen of ARC,
                                                            the page cannot be scrolled.

  [RCF-1262](https://mosip.atlassian.net/browse/RCF-1262)   In ARC, packets created with PRIDs are not being
                                                            processed.

  [RCF-1260](https://mosip.atlassian.net/browse/RCF-1260)   In ARC, the "Authenticate" button on the
                                                            authentication page is not clickable for
                                                            exception packets.

  [RCF-1258](https://mosip.atlassian.net/browse/RCF-1258)   In ARC, the "Global Config Settings" heading is
                                                            not displayed on the global configuration page.

  [RCF-1246](https://mosip.atlassian.net/browse/RCF-1246)   ARC requires license compliance updates.

  [RCF-1238](https://mosip.atlassian.net/browse/RCF-1238)   ARC has privilege escalation issues. On Hold -
                                                            Testing

  [RCF-1237](https://mosip.atlassian.net/browse/RCF-1237)   ARC does not mandate operator permissions.

  [RCF-1236](https://mosip.atlassian.net/browse/RCF-1236)   ARC contains an issue involving the use of
                                                            setAccessible(true).

  [RCF-1235](https://mosip.atlassian.net/browse/RCF-1235)   ARC has a vulnerability due to the insecure
                                                            binding mechanism used with Log4j. - Testing

  [RCF-1224](https://mosip.atlassian.net/browse/RCF-1224)   In ARC, when a user logs in with an operator
                                                            role, they are unable to start the ARC and
                                                            encounter various error messages.
  ----------------------------------------------------------------------------------------------------------

## 

## 

## **Whole list**

## 

  ----------------------------------------------------------------------------------------------------------------
  **Jira Ticket Link**                                            **Description**
  --------------------------------------------------------------- ------------------------------------------------
  [RCF-1272](https://mosip.atlassian.net/browse/RCF-1272)         The database migration should support a seamless
                                                                  schema upgrade.

  [RCF-1266](https://mosip.atlassian.net/browse/RCF-1266)         In the "Scheduled Jobs Settings" screen of ARC,
                                                                  the page cannot be scrolled.

  [RCF-1262](https://mosip.atlassian.net/browse/RCF-1262)         In ARC, packets created with PRIDs are not being
                                                                  processed.

  [RCF-1260](https://mosip.atlassian.net/browse/RCF-1260)         In ARC, the "Authenticate" button on the
                                                                  authentication page is not clickable for
                                                                  exception packets.

  [RCF-1258](https://mosip.atlassian.net/browse/RCF-1258)         In ARC, the "Global Config Settings" heading is
                                                                  not displayed on the global configuration page.

  [RCF-1246](https://mosip.atlassian.net/browse/RCF-1246)         ARC requires license compliance updates.

  [RCF-1238](https://mosip.atlassian.net/browse/RCF-1238)         ARC has privilege escalation issues. On Hold -
                                                                  Testing

  [RCF-1237](https://mosip.atlassian.net/browse/RCF-1237)         ARC does not mandate operator permissions.

  [RCF-1236](https://mosip.atlassian.net/browse/RCF-1236)         ARC contains an issue involving the use of
                                                                  setAccessible(true).

  [RCF-1235](https://mosip.atlassian.net/browse/RCF-1235)         ARC has a vulnerability due to the insecure
                                                                  binding mechanism used with Log4j. - Testing

  [RCF-1224](https://mosip.atlassian.net/browse/RCF-1224)         In ARC, when a user logs in with an operator
                                                                  role, they are unable to start the ARC and
                                                                  encounter various error messages.

  [RCF-1196](https://mosip.atlassian.net/browse/RCF-1196)         In ARC, the Attempt button should turn green and
                                                                  the captured biometrics should show a green tick
                                                                  once the biometric score crosses the threshold.

  [RCF-1177](https://mosip.atlassian.net/browse/RCF-1177)         Security testing found weak hash usage (SHA-1
                                                                  and MD5) in ARC.

  [RCF-1114](https://mosip.atlassian.net/browse/RCF-1114)         In RCF, under poor network conditions, some
                                                                  batch jobs succeed while others fail during
                                                                  application synchronization.

  [RCF-1107](https://mosip.atlassian.net/browse/RCF-1107)         In RCF's new registration flow, the biometrics
                                                                  screen appears blank when navigated from the
                                                                  document upload page of the Update UIN flow.

  [RCF-1106](https://mosip.atlassian.net/browse/RCF-1106)         In RCF's Update UIN flow, the selected identity
                                                                  proof document disappears from the dropdown when
                                                                  navigating back and returning to the document
                                                                  upload screen.

  [RCF-1099](https://mosip.atlassian.net/browse/RCF-1099)         Java modules in ARC require unit test cases.
                                                                  Testing

  [RCF-1098](https://mosip.atlassian.net/browse/RCF-1098)         In ARC, updating the UIN using an NRCID fails at
                                                                  the VALIDATE_PACKET stage. - On Hold - Testing

  [RCF-1082](https://mosip.atlassian.net/browse/RCF-1082)         In ARC, Update UIN fails with an NRCID and shows
                                                                  the error "Packet validation failed RPR-PVM-012
                                                                  → Invalid UIN -- Invalid UIN." On Hold - Dev

  [RCF-1077](https://mosip.atlassian.net/browse/RCF-1077)         The guidelines displayed on the language
                                                                  selection screen should be removed. In Progress

  [RCF-1003](https://mosip.atlassian.net/browse/RCF-1003)         In ARC, the applicant details page under pending
                                                                  approval cannot be scrolled from top to bottom.

  [RCF-1000](https://mosip.atlassian.net/browse/RCF-1000)         In ARC, pre-registration data does not always
                                                                  refresh when attempting to fetch another
                                                                  pre-registration AID.

  [RCF-968](https://mosip.atlassian.net/browse/RCF-968)           In ARC, the "Submit" button on the pending
                                                                  approvals page should be renamed to
                                                                  "Authenticate."

  [RCF-942](https://mosip.atlassian.net/browse/RCF-942)           In ARC, the image displayed on the
                                                                  authentication screen is inappropriate and needs
                                                                  to be replaced.

  [RCF-933](https://mosip.atlassian.net/browse/RCF-933)           In ARC, the Comments and Exception Type buttons
                                                                  are not removed from the Operator Onboarding and
                                                                  Update Operator Onboarding screens.

  [RCF-891](https://mosip.atlassian.net/browse/RCF-891)           In ARC's Manage Applications page, the "Created"
                                                                  and "Synched" options should be removed from the
                                                                  Client Status filter.

  [RCF-885](https://mosip.atlassian.net/browse/RCF-885)           In ARC, mandatory fields are highlighted on the
                                                                  document upload page even before submission.

  [RCF-883](https://mosip.atlassian.net/browse/RCF-883)           In ARC's portrait mode, the SNO, date, and
                                                                  operator ID are not properly aligned on the
                                                                  pending approvals page.

  [RCF-858](https://mosip.atlassian.net/browse/RCF-858)           In ARC, authentication fails for
                                                                  pre-registration data when the uploaded document
                                                                  type is a PDF.

  [RCF-857](https://mosip.atlassian.net/browse/RCF-857)           In ARC, the "Download Packet" and "Go to Home"
                                                                  buttons are not properly aligned in the French
                                                                  language interface.

  [RCF-830](https://mosip.atlassian.net/browse/RCF-830)           In ARC, when English, Tamil, or Kannada is
                                                                  selected as the demographic language, the "Menu"
                                                                  button moves out of the screen in portrait mode.

  [RCF-755](https://mosip.atlassian.net/browse/RCF-755)           In ARC, the server status does not progress
                                                                  beyond "Packet Receiver" even after UIN
                                                                  generation.

  [RCF-742](https://mosip.atlassian.net/browse/RCF-742)           In RCF, the "Introducer Details Biometrics"
                                                                  section appears for adult packet creation in the
                                                                  preview/acknowledgement screen. Re-opened

  [RCF-363](https://mosip.atlassian.net/browse/RCF-363)           In RCF, the error "Invalid machine Spec ID
                                                                  found" appears while logging into the
                                                                  registration client application.

  [RCF-192](https://mosip.atlassian.net/browse/RCF-192)           In the Android registration client,
                                                                  synchronization fails intermittently.

  [MOSIP-35259](https://mosip.atlassian.net/browse/MOSIP-35259)   In Reg-proc, the error "12 - referenceId not
                                                                  found (in ABIS)" occurs during the
                                                                  DEMOGRAPHIC_VERIFICATION stage.
  ----------------------------------------------------------------------------------------------------------------

## **Documentation**

[Feature
Documentation](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/overview/features)

[Developer
Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-developer-guide)

[UI Specification
Documentation](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-issuance/android-registration-client/develop/ui-spec-documentation)

[Configuration
Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-configuration)

[Android Registration User
Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-user-guide)


















The following API endpoints have been **deprecated** in PMS 1.3.0-beta.3 and replaced with enhanced alternatives:
### **MISP License Management**

| Deprecated Endpoint                       | Replacement                                 | Description                        |
|-------------------------------------------|---------------------------------------------|------------------------------------|
| `POST /misps`                            | `POST /misp-licenses`                       | Generate MISP license key          |
| `PUT /misps`                             | `PATCH /misp-licenses/{partnerId}`          | Update MISP license key details    |
| `GET /misps`                             | `GET /misp-licenses`                        | Retrieve MISP license key details  |
| `GET /misps/{mispId}/licenseKey`         | `PUT /misp-licenses/{partnerId}`            | Re-generate MISP license key       |
| `POST /misps/filtervalues`               | `GET /misp-licenses`                        | Filter MISP details                |
| `POST /misps/search`                     | `GET /misp-licenses`                        | Search MISP details                |

### **Partner Registration**

| Deprecated Endpoint                       | Replacement                | Description                      |
|-------------------------------------------|----------------------------|----------------------------------|
| `POST /partners`                         | `POST /partners/v3`        | Partner self-registration        |
| `POST /partners/v2`                      | `POST /partners/v3`        | Partner registration (enhanced)  |

### **Partner API Key Management**

| Deprecated Endpoint                       | Replacement                | Description                                         |
|-------------------------------------------|----------------------------|-----------------------------------------------------|
| `GET /partner-api-keys`                   | `GET /partner-api-keys/v2` | Retrieve partner API keys with enhanced filtering    |

**Migration Notice**: Deprecated endpoints will be removed in future releases. Partners and integrators should update their implementations to use the replacement endpoints to ensure continued functionality and access to enhanced features.