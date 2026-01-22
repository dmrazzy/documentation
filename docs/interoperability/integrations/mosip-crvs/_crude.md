Integrating a Civil Registration and Vital Statistics (CRVS) system with a Foundational Identity (ID) system is a critical task in Digital Public Infrastructure (DPI). There is no single "API doc" because these systems vary by country, but there are several **authoritative technical frameworks and integration guides** used by global experts.

Here are the most important integration documents and models you should refer to:

### 1. The World Bank ID4D Practitioner’s Guide

The **ID4D (Identification for Development)** initiative provides the most comprehensive framework for "Linking ID and Civil Registration."

* **Key Concept:** The "Stock and Flow" approach. The ID system handles the "Stock" (the existing population), while the CRVS handles the "Flow" (new births and deaths).
* **Integration Points:** * **Birth to UIN:** Linking ID creation directly to birth registration.
* **Death Notifications:** Automatically "retiring" a foundational ID when a death is registered in the CRVS to prevent fraud.


* **Source:** [World Bank ID4D - Linking ID and Civil Registration](https://id4d.worldbank.org/guide/linking-id-and-civil-registration)

### 2. OpenCRVS National ID Integration Documentation

If you are looking for actual **technical/API-level documentation**, **OpenCRVS** is the leading open-source standard. They provide a production-ready integration library for **MOSIP** (Modular Open Source Identity Platform).

* **Technical Details:** * Uses **OAuth credentials** for API security.
* Features a **National ID Registration integration** workflow.
* Supports **In-form authentication/verification** where the CRVS queries the ID system during a life event registration.


* **Source:** [OpenCRVS Interoperability Documentation](https://documentation.opencrvs.org/technology/interoperability/national-id)

### 3. The African Union Interoperability Framework

The AU has published a very specific **Interoperability Framework for Digital ID** (2025/2026 version) which specifically addresses how national CRVS systems should feed into foundational ID systems across the continent.

* **Focus:** It defines the "Legal Identity" lifecycle, starting from the CRVS birth record as the **authoritative source** for the Foundational ID.
* **Source:** [AU Interoperability Framework](https://au.int/sites/default/files/documents/43393-doc-AU_Interoperability_framework_for_D_ID_English.pdf)

### 4. UNICEF Strategic Framework (2025)

UNICEF’s recent guidance focuses on the "Right to Legal Identity." It provides a strategic roadmap for integrating birth registration with digital ID systems, specifically in the East and Southern Africa region (ESAR).

* **Accelerator:** It identifies **Digitalisation and Interoperability** as the two primary accelerators for "closing the gap" between a child’s birth certificate and their future national ID.
* **Source:** [UNICEF Birth Registration Framework 2025](https://www.unicef.org/esa/media/15701/file/UNICEF-Birth-Registration-Program-Framework-ESA-2025.pdf)

---

### 🏛️ Summary Table: How they connect

| Integration Point | Triggering System | Subscribing System | Action |
| --- | --- | --- | --- |
| **Birth Registration** | CRVS | Foundational ID | Generates a UIN (Unique ID Number) for the child. |
| **Identity Proofing** | Foundational ID | CRVS | ID system queries CRVS to verify the birth certificate is real. |
| **Death Registration** | CRVS | Foundational ID | ID system flags the record as "Deceased" and revokes credentials. |
| **Name Change** | CRVS | Foundational ID | ID system updates the legal name on the digital ID. |

### 💡 My Suggestion:

If you are designing an integration, I recommend starting with the **OpenCRVS documentation** because it contains the most granular technical details (APIs, webhooks, and data schemas).

**Would you like me to find a specific API schema or a list of "Best Practices for Data Sharing" between these two systems?**