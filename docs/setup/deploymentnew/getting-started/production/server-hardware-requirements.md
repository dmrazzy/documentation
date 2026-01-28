# Server Hardware Requirements

## **Overview**

This document provides a step-by-step guide for using the Resource Calculator to estimate the required server hardware for a country’s MOSIP deployment. The calculator delivers a high-level overview of the hardware components needed to support MOSIP services across Registration and Authentication.

{% hint style="info" %}
**Note**: Pre-Registration is not included in this version and will be enabled in an upcoming release.
{% endhint %}

### **MOSIP Deployment Components**

A typical MOSIP deployment includes the following components:

* ID Lifecycle Management (Issuance, Verification and Management modules/components)
* Pre-Registration
* Registration
* ID Authentication

The Resource Calculator focuses on estimating hardware capacity from the Registration through to Authentication.

### **Out of Scope**

The following areas are **not covered** by this version of the calculator:

* **KYC with OTP Verification** in the Authentication flow
* **Pre-Registration** processes and related hardware requirements
* Registration Packet processing once packet is uploaded

These will be considered in future enhancements of the calculator.

### **Key Assumptions**

To generate accurate estimates, the calculator is based on the following assumptions:

#### **Weightage Calculation**

* Weightage represents the percentage distribution of how often specific scenarios occur in real time.
  * Examples:
    * 40% of authentication requests use OTP.
    * 90% of registration requests are for new packet uploads.
    * 5% of sync requests once registration packet is created and ready to be uploaded
* These weightage play a critical role in performance estimation and calculator outputs.

{% hint style="info" %}
**Note:** Users are advised to define the weightage based on their specific requirements, re-run the performance tests, and then use the calculator to derive accurate resource estimates.
{% endhint %}

#### **Performance Metrics**

Performance testing is measured using the following metrics:

* **Sync Data:** Number of packet uploads per day (Assumes packet uploads occur in the last hour of the day).
* **IDA:** Transactions per second (TPS) or authentications per hour.

#### **Scenario Scope**

* Only positive scenarios are considered.
* Negative scenarios requiring human involvement (e.g., Manual Adjudication) are excluded.

#### **External System Response Time**

* Systems such as ABIS are assumed to have a maximum response time of **300 ms**.

#### **Calculation Flexibility**

* The calculator can scale per-hour processing to:
  * Full-day operations
  * Limited working hours
  * Custom durations

#### **Load Multiplication Testing**

* Packet processing can be evaluated at **2 times** and **3 times** load to validate hardware scaling behaviour.

### **Important Considerations**

* Default values in the calculator are derived from insights gathered during MOSIP deployments in multiple countries.\
  Users may modify these values to match their actual or expected usage.
* The calculations provide approximate estimates based on internal testing.\
  Actual performance may vary depending on:
  * Workload characteristics
  * Infrastructure capacity
  * Hardware performance
  * Data complexity

### **Accessing the Calculator**

Refer to the link below to download and access the **Resource Calculator**.

#### Calculator for Release Version(s) - 1.2.1.0 and 1.2.1.X

{% file src="../../../../.gitbook/assets/resource-calculator-platform-release-1.3.0.xlsx" %}

The calculator provides two primary outputs:

1. **Total Resources Required**
2. **Total Time Required to Complete Registrations**

#### Calculator for Release Version(s) 1.2.0.1 and Earlier

{% file src="../../../../.gitbook/assets/ida-resource-calculator.xlsx" %}

{% file src="../../../../.gitbook/assets/pre-reg-resource-calculator-v2.xlsx" %}

### **Calculator Structure**

Upon opening the calculator, users will find the following three sheets as shown in the below screen:

* **Summary:**\
  Primary input sheet where users provide parameters and view the overall resource requirement.
* **Registration Upload & SyncData:**\
  Displays resource requirements and calculations for packet upload and synchronization processes.
* **ID Authentication:**\
  Contains resource requirement details and calculations for handling authentication requests.

### **User Input**

Users can update values in the **Key Inputs** section as highlighted in the below screen, based on their deployment needs.\
All resource estimates will automatically adjust according to the data entered.
