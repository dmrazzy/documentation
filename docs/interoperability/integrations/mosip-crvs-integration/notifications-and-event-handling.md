---
hidden: true
---

# Notifications & Event Handling

### WebSub Overview

#### What is WebSub?

WebSub is a publish-subscribe protocol used by MOSIP to send event notifications to integrated partners like CRVS systems. It allows real-time updates on credential generation and packet status changes.

#### How Does It Work?

1. CRVS subscribes to specific WebSub topics during partner onboarding
2. MOSIP publishes events (e.g., UIN generated, packet processed) to these topics
3. WebSub hub delivers notifications to subscribed CRVS endpoints
4. CRVS processes the notification and takes appropriate action

### Credential Issuance Notifications

#### Credential Data Structure

When a UIN is generated, MOSIP publishes credential data via WebSub. The default recommendation is to share:

* PSUT Token (Print Secure UIN Token) - for printing eUIN cards
* Alternatively: UIN, VID, or PSUT token based on country policy

#### Subscription Configuration

CRVS systems must:

1. Register as credential partners in MOSIP
2. Configure policy to receive credential notifications
3. Provide callback endpoint for WebSub delivery
4. Implement retry logic for failed deliveries

### Packet Status Updates

#### Status Types

MOSIP publishes packet status updates for:

* Packet received
* Packet under processing
* Packet processed successfully
* Packet rejected (with reason)

#### Filtering

CRVS can filter status updates based on:

* Packet ID
* Status type
* Source CRVS system

### Error Notifications

(Detail error notification structure and handling)

***

### Learn More

* [Core Integration Principles](integration-overview-and-context/core-integration-principles.md) - Understanding identity credential sharing and packet status architecture
* [Policy Configuration & Customization](prerequisites-configurations-and-operations/configuration-and-customization.md) - Partner policy setup and credential sharing configuration
* [Security & Authentication](prerequisites-configurations-and-operations/prerequisites/security-and-authentication.md) - Partner onboarding, authentication mechanisms, and callback security
* [Error Handling & Reconciliation](error-handling-and-reconciliation.md) - Managing notification failures and implementing retry strategies
