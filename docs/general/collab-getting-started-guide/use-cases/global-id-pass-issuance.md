# Global ID Pass Issuance

### Try It Out

Click [**here**](global-id-pass-issuance.md#what-youll-experience) to 'Try This Out' now using the given portal addresses and the demo credentials provided, or, read through the guide to understand the steps and then then explore the solution.

### **Overview**  <a href="#welcome-to-the-global-id-pass-self-experience-demo-guide" id="welcome-to-the-global-id-pass-self-experience-demo-guide"></a>

**Global ID Pass Credential** is a **government-issued digital identity**, implemented as a **Verifiable Credential (VC)** in the **Republic of Utopia** _(a fictional country used for demonstration purposes)_. It enables residents to securely prove their identity and access government services both **online and offline**, while preserving privacy and user consent. Stored in the Inji Wallet, the credential showcases how modern digital identity systems can enable trusted, seamless, and connectivity-independent service delivery.

Explore how **Global ID Pass Credentials**, issued as secure **Verifiable Credentials (VCs)**, enable **trusted, seamless, and privacy-preserving access to government services** — both **online and offline** across the **Republic of Utopia**.

This step-by-step guide walks you through **issuing, storing, and using Global ID Pass Credentials**, showcasing how digital identity can transform citizen service delivery.

### What You’ll Experience <a href="#what-youll-experience" id="what-youll-experience"></a>

#### **Use Case 1: Online Registration and Global ID Pass Issuance** <a href="#use-case-1-online-registration-and-global-id-pass-issuance" id="use-case-1-online-registration-and-global-id-pass-issuance"></a>

* Register and issue a **Global ID Pass** via the [**Global ID Pass Admin Portal**](https://globalid-pass.collab.mosip.net/)
* Download and securely store credentials in the [**Inji Wallet**](global-id-pass-issuance.md#wallet-downloads)**.**
* Use your credentials to **log in and access services** on the [**Republic of Utopia Portal**](https://utopia.collab.mosip.net/)

#### **Use Case 2: Offline Sharing of Global ID Pass via Bluetooth Low Energy (BLE)** <a href="#use-case-2-offline-sharing-of-global-id-pass-via-bluetooth-low-energy-ble" id="use-case-2-offline-sharing-of-global-id-pass-via-bluetooth-low-energy-ble"></a>

* Experience **offline credential sharing** using BLE
* Enable secure identity sharing **without internet connectivity**

### Setup Requirements <a href="#setup-requirements" id="setup-requirements"></a>

Ensure you have:

#### Devices <a href="#devices" id="devices"></a>

* **2 mobile devices** — for BLE demo
* **1 laptop/desktop** — to access portals

> **Note:** The BLE demo requires **two mobile devices**. This can be **either two Android devices or one Android and one iOS device**. A combination of **two iOS devices is not supported** for this demo.

#### Access to <a href="#access-to" id="access-to"></a>

* [**Global ID Pass Admin Portal**](https://globalid-pass.collab.mosip.net/)
* [**Republic of Utopia Portal**](https://utopia.collab.mosip.net/)
* [**Inji Wallet App**](https://mosip.atlassian.net/wiki/spaces/Inji/pages/edit-v2/2444296193#Wallet-Downloads)

#### Wallet Downloads <a href="#wallet-downloads" id="wallet-downloads"></a>

* **Android APK** — Download [**here**](https://mosip.atlassian.net/wiki/external/NmRmMzE3NDJlYjI4NGMxZThmNWUwZGU4MzI4ZjNiNTE)
* **iOS TestFlight** — Download [**here**](https://testflight.apple.com/join/7FTAdjLe)

#### Sample Global ID Pass Admin Login Credentials <a href="#sample-global-id-pass-admin-login-credentials" id="sample-global-id-pass-admin-login-credentials"></a>

* **Username:** compass
* **Password:** compass@123

Observe the user flow and follow the instructions and steps below it to begin your experience! Let’s get started!

### **Flow 1 — Issuance + Wallet Setup + Online Login** <a href="#flow-1-issuance--wallet-setup--online-login" id="flow-1-issuance--wallet-setup--online-login"></a>

<figure><img src="../../../.gitbook/assets/image-20260128-153336.png" alt=""><figcaption></figcaption></figure>

### Flow 2 — Offline Credential Sharing (BLE) <a href="#flow-2-offline-credential-sharing-ble" id="flow-2-offline-credential-sharing-ble"></a>

<figure><img src="../../../.gitbook/assets/image-20260128-154141.png" alt=""><figcaption></figcaption></figure>

## Use Case 1: Online Registration and Global ID Pass Issuance <a href="#use-case-1-online-registration-and-global-id-pass-issuance.1" id="use-case-1-online-registration-and-global-id-pass-issuance.1"></a>

### **Step 1: Issue Global ID Pass via Admin Portal** <a href="#step-1-issue-global-id-pass-via-admin-portal" id="step-1-issue-global-id-pass-via-admin-portal"></a>

**As an Admin**

#### **1.1 Log in to Global ID Pass Admin Portal** <a href="#id-1.1-log-in-to-global-id-pass-admin-portal" id="id-1.1-log-in-to-global-id-pass-admin-portal"></a>

Log in to admin [**portal**](https://globalid-pass.collab.mosip.net/) using the credentials provided above as an **authorized Government Agent.**

#### **1.2 Create a New Application** <a href="#id-1.2-create-a-new-application" id="id-1.2-create-a-new-application"></a>

* Navigate to **“New Application”**
* Enter citizen details manually
* Review details on the preview screen
* Click **Submit** to complete the application

#### **1.3 Issue Global ID Pass Credential** <a href="#id-1.3-issue-global-id-pass-credential" id="id-1.3-issue-global-id-pass-credential"></a>

* Submit the application
* The system generates a **Global ID Pass Verifiable Credential**

### Step 2: Download & Store Global ID Pass in Inji Wallet <a href="#step-2-download-and-store-global-id-pass-in-inji-wallet" id="step-2-download-and-store-global-id-pass-in-inji-wallet"></a>

**As a Resident**

#### **2.1 Install the Global ID Pass Wallet** <a href="#id-2.1-install-the-global-id-pass-wallet" id="id-2.1-install-the-global-id-pass-wallet"></a>

Install the wallet on your **Android or iOS** device using the links [above](global-id-pass-issuance.md#wallet-downloads).

#### **2.2 Download Global ID Pass Credential** <a href="#id-2.2-download-global-id-pass-credential" id="id-2.2-download-global-id-pass-credential"></a>

* Open **Inji Wallet**
* Select the **Global ID Authority Issuer**
* Authenticate using static **OTP: 111111**
* Credential is **securely stored in the wallet**

### Step 3: Access Services on the Republic of Utopia Portal <a href="#step-3-access-services-on-the-republic-of-utopia-portal" id="step-3-access-services-on-the-republic-of-utopia-portal"></a>

#### **3.1 Open Republic of Utopia Portal** <a href="#id-3.1-open-republic-of-utopia-portal" id="id-3.1-open-republic-of-utopia-portal"></a>

Access the [**portal**](https://utopia.collab.mosip.net/) on a **desktop browser** or **another mobile device**.

#### **3.2 Login Using Global ID Pass** <a href="#id-3.2-login-using-global-id-pass" id="id-3.2-login-using-global-id-pass"></a>

* Click **“Login with Global ID Pass”**
* A **QR code** appears on screen

#### **3.3 Scan QR Code from Inji Wallet** <a href="#id-3.3-scan-qr-code-from-inji-wallet" id="id-3.3-scan-qr-code-from-inji-wallet"></a>

* Open **Inji Wallet**
* Tap **Scan**
* Scan the QR code displayed on the portal

#### **3.4 Face Authentication & Consent** <a href="#id-3.4-face-authentication-and-consent" id="id-3.4-face-authentication-and-consent"></a>

* Complete **Face Authentication**
* Provide **consent to share credentials**

#### **3.5 Access Government Services** <a href="#id-3.5-access-government-services" id="id-3.5-access-government-services"></a>

You are now securely logged in and can access available services on the **Republic of Utopia Portal**.

## Use Case 2: Offline Sharing of Global ID Pass via BLE <a href="#use-case-2-offline-sharing-of-global-id-pass-via-ble" id="use-case-2-offline-sharing-of-global-id-pass-via-ble"></a>

### **Step 1: Offline Credential Sharing for Remote Sharing** <a href="#step-1-offline-credential-sharing-for-remote-sharing" id="step-1-offline-credential-sharing-for-remote-sharing"></a>

Enable **secure, internet-free verification** using Bluetooth ideal for remote or low-connectivity environments.

#### **1.1 Prepare Devices** <a href="#id-1.1-prepare-devices" id="id-1.1-prepare-devices"></a>

Enable **Bluetooth** on both devices:

* **Holder Device:** Resident’s Inji Wallet
* **Verifier Device:** Service Provider / Agent’s Inji Wallet

#### **1.2 On Resident’s Device (Wallet Holder)** <a href="#id-1.2-on-residents-device-wallet-holder" id="id-1.2-on-residents-device-wallet-holder"></a>

* Tap **Share** in navigation bar
* Grant permissions
* Scan QR code displayed on verifier’s device
* Bluetooth connection is established

**Select a Credential to Share**

* **Share** → Sends credential
* **Share with Selfie** → Captures selfie and verifies face against VC before sending

#### **1.3 On Verifier’s Device (Android Only)** <a href="#id-1.3-on-verifiers-device-android-only" id="id-1.3-on-verifiers-device-android-only"></a>

* Go to **Settings → Receive Cards**
* Display QR code
* Receive credential securely
* View received credentials under **Settings → Received Cards** _(view-only)_

## Use Case Recap <a href="#use-case-recap" id="use-case-recap"></a>

As a **Resident of the Republic of Utopia**, you experienced:

* Receiving a **Global ID Pass** issued by government authorities
* Downloading and securely storing credentials in the **Global ID Pass Wallet**
* Logging into the **Republic of Utopia Portal** using verifiable credentials
* Sharing credentials **offline via Bluetooth (BLE)**
* Experiencing **secure, consent-based, and privacy-preserving identity verification**

This experience showcases the power of secure **foundational digital identity** and **verifiable credentials**, in enabling hassle-free access to essential services. We hope this walkthrough gave you valuable insights into the potential of trusted **digital identity ecosystems**, powered by [MOSIP](https://docs.mosip.io/1.2.0), [Inji](https://docs.inji.io/), and [eSignet](https://docs.esignet.io/).

Thank you for participating and stay connected for more exciting innovations ahead!
