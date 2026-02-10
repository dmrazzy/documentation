# Cross-Border Truck Pass Issuance and Verification

### Try It Out

Click [**here**](cross-border-truck-pass-issuance-and-verification.md#setup-requirements) to 'Try This Out' now using the given portal addresses and the demo credentials provided, or, read through the guide to understand it further about the 'Use-Case' and then explore the solution.

### Overview

TruckPass is a **verifiable credential (VC)–based digital travel pass** that enables truck drivers transporting goods across borders to move efficiently and securely between two countries (for example, _Country A_ and _Country B_). The pass is **applied for by the transport company** and **used by the driver** at border checkpoints through the Inji ecosystem.

Explore how **TruckPass Credentials**, issued as secure **Verifiable Credentials (VCs)**, enable trusted, seamless, and privacy-preserving **cross-border movement of goods and drivers** online between participating countries.

This step-by-step guide walks you through **issuing, storing, and presenting TruckPass Credentials**, demonstrating how verifiable credentials can **digitise freight travel, streamline border checks, and improve compliance** for transport authorities, logistics companies, and drivers.

### What You’ll Experience

#### Use Case 1: Driver Registration and TruckPass Issuance

* Register a driver under a pre-registered transport company using the TruckPass Issuance Portal
* Request and issue a TruckPass as a Verifiable Credential for a specific consignment
* Download and securely store the TruckPass credential in the Inji Wallet

#### Use Case 2: TruckPass Presentation and Verification at Border Checkpoints

* Present the TruckPass credential at a border checkpoint using QR code or BLE
* Experience secure verification of driver, vehicle, and consignment details
* Enable seamless cross-border clearance with limited or no internet connectivity

### Pre-Requisites

**Company Registration (Assumption)**

For the purpose of this use case, it is **assumed that the transport company is already registered and authorized** with the issuing authority.

During the **Driver Registration flow**, the user will be able to **select a transport company** from a list of **pre-registered (fictional) companies** available in the system.

**Before using the TruckPass system**

* The transport company is pre-registered with the authority
* The company is authorized to add drivers and request TruckPass credentials

**Company Details Available in the System**

The following company information is already available in the database and is displayed in the UI **when the driver selects a company during registration**:

* Company Name
* Registration Status (Active / Inactive)
* Registration Type
* Company’s Registered Email Address (used for login and OTP)

### Actors & Interfaces

#### Key Actors

* **Driver** – Applies for and holds the TruckPass VC
* **Company Representative / HR** – Requests TruckPass for consignments
* **Transport Authority** – Issues the TruckPass VC
* **Border Authority / Customs Officer** – Verifies the TruckPass

#### Interfaces Used

* **TruckPass Issuance Portal (Government Portal)**
* **Inji Certify** – Credential issuance backend
* **Inji Wallet (Mobile / Web)** – Credential storage and presentation
* **Inji Verify** – Border verification application

### Setup Requirements

Ensure you have:

#### Devices

* **1 mobile devices** — for VC download
* **1 laptop/desktop** — to access portals

#### Access to

* Driver Registration Portal
* Truck Pass Portal
* [**Inji Wallet App**](https://mosip.atlassian.net/wiki/spaces/Inji/pages/edit-v2/2444296193#Wallet-Downloads)

#### Wallet Downloads

* **Android APK** — Download [**here**](https://mosip.atlassian.net/wiki/external/NmRmMzE3NDJlYjI4NGMxZThmNWUwZGU4MzI4ZjNiNTE)
* **iOS TestFlight** — Download [**here**](https://testflight.apple.com/join/7FTAdjLe)

#### Sample Truck Pass Portal Credentials

* **OTP for Truck Pass Portal:** 111111

Follow the instructions and steps below it to begin your experience! Let’s get started!

### Use Case 1: Driver Registration and TruckPass Issuance

#### Step 1: Register Driver via TruckPass Issuance Portal

**As a Driver / Company Representative**

**1.1 Log in to the TruckPass Issuance Portal**

Log in to the TruckPass Issuance Portal\<Link for portal in collab> using the registered company email address as an authorized transport company representative or driver.

**1.2 Select Transport Company**

* Navigate to **Driver Registration**
* Select a transport company from the dropdown list\
  &#xNAN;_(For this use case, a fictional company is pre-registered and available for selection)_

**1.3 Authenticate Driver Using National ID (UIN)**

* Enter the driver’s **UIN / National ID**
* The system redirects to **eSignet** for authentication
* Authenticate using OTP received on the registered channel
* On successful authentication, the following details are auto-filled:
  * Full Name
  * UIN
  * Phone Number
  * Gender
  * Email ID
  * City
  * Face Image / Photo

**1.4 Enter Additional Driver Details**

* Enter **Driver’s License Number and additional driver details**

**1.5 Submit Driver Registration**

* Review all entered and auto-filled details
* Click **Submit**
* The system validates the information and creates the driver profile
* The driver is linked to the selected transport company

#### Step 2: Request and Issue TruckPass for a Consignment

**As an Authorized Company Representative**

**2.1 Initiate TruckPass Request**

* Log in to the TruckPass Issuance Portal \<Link for truck pass portal collab link>
* Navigate to **Request TruckPass**

**2.2 Select Registered Driver**

* Search for the driver using **UIN or name**
* Select the driver
* Driver details are auto-populated for review

**2.3 Enter Consignment and Vehicle Details**

* Enter mandatory **consignment details** (Invoice, Waybill/CMR, Customs documents)
* Enter **vehicle details** (Vehicle type, axle size, registration documents, license plate)
* Provide **cross-border travel details**:
  * Exporter and Importer
  * Entry / Exit Point
  * Country of Origin and Destination
  * Date of Departure and Return

**2.4 Submit TruckPass Request**

* Submit the application
* The system generates a **TruckPass Verifiable Credential (VC)**
* The VC is digitally signed using the **Transport Authority’s DID**
* An email notification is sent to the registered driver

### Step 3: Download & Store TruckPass in Inji Wallet

**As a Driver**

**3.1 Access Inji Wallet**

* Install and open **Inji Wallet** on your mobile device
* Complete wallet authentication

**3.2 Download TruckPass Credential**

* Authenticate using **UIN** or
* Scan the **QR code** shared by the company
* The TruckPass Verifiable Credential is downloaded
* The credential is securely stored in the wallet

### Step 4: TruckPass Verification at Border Checkpoint

**As a Border Authority / Customs Officer**

**4.1 Present TruckPass Credential**

* Open Truck Pass verifier portal \<Link for verifier portal in collab>
* The driver presents the TruckPass VC using:
  * QR code
  * BLE transfer
  * File upload

**4.2 Verify Credential Using Inji Verify**

* Scan or upload the credential in **Inji Verify**
* The system validates:
  * Issuer DID signature
  * Schema compliance
  * Expiry and revocation status
* Driver, vehicle, and consignment details are displayed in a readable format

**4.3 Grant or Reject Entry**

* Entry is granted if verification is successful
* Entry is rejected if the credential is invalid or expired

### Use Case Recap

As a **Truck Driver and Transport Company**, you experienced:

* Registering a driver under a pre-registered transport company
* Issuing a **TruckPass as a Verifiable Credential**
* Downloading and securely storing the TruckPass in the Inji Wallet
* Presenting the TruckPass at a border checkpoint
* Verifying the credential using Inji Verify with or without internet connectivity

This walkthrough demonstrates how **TruckPass leverages verifiable credentials** to enable **secure, consent-based, and privacy-preserving cross-border freight movement**, powered by **MOSIP, Inji, and eSignet**.

Thank you for exploring the TruckPass use case. Stay connected for more innovations in digital identity and cross-border mobility 🚛🌍
