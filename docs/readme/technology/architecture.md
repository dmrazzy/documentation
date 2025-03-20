---
icon: grid
---

# Architecture

MOSIP is built on a modular, microservices-based architecture. Its modularity enables seamless adoption even in complex scenarios. Most [MOSIP modules](../../modules.md) are designed as robust foundational infrastructure components, making them suitable for integration into various projects.

MOSIP is designed with the following architectural principles. These architecture principles are core to the development of the system's features and have a great influence on how and why specific software design patterns are used within.

* Data Privacy
* No Vendor Lock-in
* Open Standards
* Async/ Offline First
* Commodity Computing
* Fault tolerant
* Manageable
* Secure By Default

### Architecture Overview

The diagram below provides an architectural overview, visually representing the components of the MOSIP Identity framework and its associated technology stack.

<figure><img src="../../.gitbook/assets/MOSIP-Architecture-Overview.jpg" alt=""><figcaption><p>Architecture Overview</p></figcaption></figure>

### High-Level Reference Functional Architecture

This reference blueprint provides a comprehensive vision for designing and implementing a Digital ID-led DPI infrastructure. It outlines how foundational identity systems can be leveraged alongside key DPI components, enabling various use cases across both the public and private sectors. The blueprint is structured in layers of technology, governance, and service delivery, ensuring scalability, inclusivity, and compliance with legal frameworks.&#x20;

<div align="left"><figure><img src="../../.gitbook/assets/Functional_Architecture.png" alt=""><figcaption></figcaption></figure></div>

\
Below is an explanation of its core components and their roles within the ecosystem.

#### 1. Unique Digital Identity <a href="#unique-digital-identity" id="unique-digital-identity"></a>

At the core of the system is the Unique Digital Identity, which establishes a singular, definitive identity for every individual. This digital identity forms the backbone of all operations, ensuring secure identification and verification across diverse domains.&#x20;

#### 2. ID Project Custodian <a href="#governance" id="governance"></a>

The ID Project Custodian, whether a designated authority, ministry, or department, oversees this infrastructure. This custodian is crucial for maintaining governance, regulatory compliance, and operational efficiency, ensuring that all identity services align with national policies and frameworks.

#### 3. Infrastructure Layer <a href="#infrastructure-layer" id="infrastructure-layer"></a>

Supporting the ecosystem is a robust Infrastructure Layer, consisting of cloud infrastructure and connectivity. The cloud ensures scalability, resilience, and the ability to manage large volumes of identity data. The connectivity layer ensures the system's accessibility, making it functional in both urban and remote areas.

#### 4. Foundational ID Platform <a href="#foundational-id-platform" id="foundational-id-platform"></a>

At the heart of the blueprint is the Foundational ID Platform, a centralized framework for issuing and managing foundational IDs. This platform underpins all identity-related services, ensuring secure identity handling and enabling core functions like registration, lifecycle management, verification, authentication, and e-KYC (Electronic Know Your Customer). These services are essential for building trusted interactions across all sectors.

#### 5. Core & Augmented Services <a href="#services" id="services"></a>

Now that we have established the core layers to anchor trust, we can expand functionality through augmented services, such as **eSignature** capabilities. These enable secure digital transactions and help governments transition from traditional in-person signatures. These added features enhance the system's utility, which is especially crucial for enabling paperless workflows.

#### 6. Accessibility and Inclusivity <a href="#accessibility-and-inclusivity" id="accessibility-and-inclusivity"></a>

To ensure widespread adoption and accessibility, the system features a Multi-Channel Access Layer that enables interaction through **web**, **mobile (Apps, USSD)**, and **physical interfaces(QR)**. This design ensures inclusivity by accommodating individuals with diverse backgrounds and varying levels of technological proficiency, to interact with the system effortlessly.

#### 7. Consent Management, Data Exchange and Payments <a href="#consent-management-and-data-exchange" id="consent-management-and-data-exchange"></a>

In addition to its core and augmented services, consent management and data exchange capabilities are crucial for advancing digitization and digital development. These capabilities facilitate the regulated sharing of information across both public and private platforms. Moreover, the integration of payment systems adds significant value to Government-to-People (G2P) use cases. Additionally, data exchange can be further enhanced through the use of Verifiable Credentials.

#### 8. Use Cases and Benefits Delivery <a href="#use-cases-and-benefits-delivery" id="use-cases-and-benefits-delivery"></a>

This blueprint is designed to act as a catalyst for seamless service delivery and impactful use cases across both the private and public sectors. It is essential to involve the private sector in the digitization process, as it plays a key role in enabling services such as banking, e-commerce, and telecommunications, thereby streamlining operations. In the public sector, this blueprint enhances service delivery in areas like social welfare, healthcare, taxation, and education, contributing to better governance and increased citizen engagement. By adopting an overall ID-led approach, it not only accelerates the adoption of digital identity but also ensures improved governance, streamlined operations, and enhanced citizen participation.

#### 9. Legal and Regulatory Framework <a href="#legal-and-regulatory-framework" id="legal-and-regulatory-framework"></a>

Finally, the framework operates within a robust legal and regulatory environment, complying with key legislation such as the Data Protection Act, the Cybersecurity Act, and the Electronic Transactions Act. These laws safeguard data privacy and security, ensuring that the system operates transparently, trustworthy, and in full legal compliance.

To know how MOSIP can be deployed, refer to [Getting Started](https://docs.mosip.io/1.2.0/deploymentnew/getting-started). The different installation models are detailed in the [Deployment](https://docs.mosip.io/1.2.0/deploymentnew) section.
