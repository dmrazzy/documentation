---
description: >-
  Configure credential-sharing policies and WebSub notifications for MOSIP–CRVS
  integration.
hidden: true
---

# Configuration & Customization

### Overview

This page covers policy-level configuration for MOSIP–CRVS integration. Use it to control what MOSIP shares with CRVS. This applies after packet processing. It focuses on credential sharing and WebSub notifications.

#### What you configure

* How CRVS is set up as a MOSIP partner for receiving notifications
* What identifier MOSIP shares with CRVS (PSUT vs UIN vs VID)
* Which WebSub topics CRVS subscribes to (credentials vs packet status)
* Country-specific policy and privacy choices

{% hint style="info" %}
CRVS is the **source of truth** for vital events. MOSIP trusts CRVS-provided event data. MOSIP focuses on technical validations.
{% endhint %}

### Partner policy configuration

CRVS must be onboarded as a MOSIP partner before MOSIP can push events. At a minimum, configure:

* A **credential sharing policy** that defines what MOSIP can disclose.
* WebSub **subscription(s)** pointing to CRVS callback endpoints.

See also:

* [Security & Authentication](prerequisites/security-and-authentication.md)
* [Notifications & Event Handling](../notifications-and-event-handling.md)

### Credential sharing policy

#### Policy Options

Countries can customize credential sharing based on their requirements:

1. **Default (Recommended)**: Share PSUT token

PSUT is a partner-scoped token. It avoids exposing the resident’s raw identifier. It supports credential printing and correlation within CRVS.

* Protects UIN privacy
* Allows credential printing
* Minimizes PII exposure

2. **Option 2**: Share UIN directly

* Simplest approach
* Higher privacy risk and broader blast radius
* Use only if CRVS has strong data protection

3. **Option 3**: Share VID

* Temporary identifier
* Better privacy than UIN
* Requires VID management

4. **Option 4**: Share PSUT + VID

* Balanced approach
* Flexibility for different use cases

{% hint style="warning" %}
Sharing **UIN** is not recommended unless legally mandated. Prefer PSUT and/or VID to reduce exposure of sensitive identifiers.
{% endhint %}

### WebSub topic configuration

MOSIP publishes two event streams that CRVS may subscribe to:

* **Credential issuance notifications** (e.g., identity created after an infant birth flow).
* **Packet status updates** (e.g., processed / rejected with reason codes).

Keep credential sharing and packet tracking as separate subscriptions. This simplifies filtering and operational troubleshooting.

### Country-specific customizations

Common customization points:

* Choose the identifier to share (PSUT, VID, UIN).
* Adjust what attributes are included in the credential payload.
* Support multiple CRVS systems by onboarding each as a separate partner.
* Implement local governance rules for retention, audit, and access controls.

***

### Learn more

* [Core Integration Principles](../integration-overview-and-context/core-integration-principles.md)
* [Notifications & Event Handling](../notifications-and-event-handling.md)
* [Operational Considerations](operational-considerations/)
