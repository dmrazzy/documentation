---
icon: book
---

# Collab Getting Started Guide

## Overview

MOSIP Collab is the latest implementation environment for MOSIP products and modules. It allows partners, developers, contributors, and evangelists to explore reference implementations, validate integrations, and test against the latest platform APIs and behaviors.

## Why Use Collab

* Validate integrations against the latest MOSIP implementation.
* Test module interoperability in a shared environment.
* Explore reference implementations to understand final product behavior.
* Build and evaluate partner or developer solutions using current APIs.

## Who Should Use This Guide

* Implementing partners
* Developers and system integrators
* Community contributors
* Product evangelists and ecosystem advocates

## Access Collab

1. Open [MOSIP Collab](https://collab.mosip.net/).
2. Sign in and verify your access.
3. If access is unavailable, request support through the [MOSIP Community](https://community.mosip.io/).
4. Confirm that the required modules are visible in the Collab portal.

## Prerequisites

### UIN Credential (Required)

A UIN is required for most Collab journeys and module testing.

1. Open [MOSIP Collab](https://collab.mosip.net/).
2. Select **Get UIN** to open [Self Registration](https://self-register.collab.mosip.net/).
3. Complete the self-registration form and submit.
4. Check your email for the issued UIN.

If self-registration is not suitable for your case, request UIN through this [request form](https://docs.google.com/forms/d/e/1FAIpQLSc2I0CQqlYRIrEmcJ3J3tKlYOVNcYNj88YZe4MMwU2RZTrjOA/viewform).

For full details, refer to [Generating Demo Credentials Guide](https://docs.mosip.io/1.2.0/general/collab-getting-started-guide/generating-demo-credentials).

### Module-Specific Prerequisites

Some modules may require additional setup. Follow the module guide linked in the module section below.

## Common Setup Reused Across Modules

Complete the following once, then reuse it across module exploration:

1. Collab access confirmation
2. UIN generation and validation
3. Basic environment readiness for your selected module

This common baseline reduces repeated setup for each module.

## Quick Start Workflow

1. Access [MOSIP Collab](https://collab.mosip.net/).
2. Generate or obtain a UIN.
3. Select a module.
4. Follow the module setup guide.
5. Run an end-to-end scenario.
6. Record outcomes and integration observations.

## Reference Implementation Exploration Path

This path is intended for evangelists and stakeholders who want a product experience view.

1. Access Collab and complete UIN setup.
2. Launch key user journeys for selected modules.
3. Observe end-user and operator workflows.
4. Capture product insights for demos, workshops, and ecosystem enablement.

## Modules in Collab

### Pre-registration

* **What to explore**: Resident pre-enrollment, document upload, and appointment booking flow.
* **Setup guide**: [Pre-registration Setup Guide](https://docs.mosip.io/1.2.0/collab-getting-started-guide/collab-pre-registration-guide)
* **Product guide**: [Pre-registration User Guide](https://docs.mosip.io/1.2.0/modules/pre-registration/pre-registration-user-guide)

### Registration Client

* **What to explore**: Resident enrollment operations, synchronization, and packet lifecycle handling.
* **Setup guide**: [Registration Client Setup Guide](https://docs.mosip.io/1.2.0/collab-getting-started-guide/collab-reg-client-setup-guide)
* **Product guide**: [Registration Client User Guide](https://docs.mosip.io/1.2.0/modules/registration-client/registration-client-user-guide)

### Resident Portal

* **What to explore**: UIN-linked resident self-service workflows.
* **Setup guide**: [Resident Portal Collab Guide](https://docs.mosip.io/1.2.0/collab-getting-started-guide/collab-resident-portal-guide)
* **Product guide**: [Resident Portal User Guide](https://docs.mosip.io/1.2.0/id-lifecycle-management/identity-management/resident-services/test/resident-portal-user-guide)

### Admin Portal

* **What to explore**: Administrative operations for master data and system controls.
* **Access note**: Admin portal access in Collab may be limited. Use [MOSIP Community](https://community.mosip.io/) for access requests.
* **Product guide**: [Admin Portal User Guide](https://docs.mosip.io/1.2.0/modules/administration/admin-portal-user-guide)

### Partner Management System

* **What to explore**: Partner onboarding and policy management workflows.
* **Setup guide**: [Partner Management Collab Guide](https://docs.mosip.io/1.2.0/collab-getting-started-guide/collab-pmp-guide)
* **Product guide**: [Partner Management Portal Guide](https://docs.mosip.io/1.2.0/modules/partner-management-services/partner-management-portal)

### Inji Stack

* **What to explore**: Credential issuance, wallet usage, and verification journeys.
* **Setup guide**: [Inji Try It Out](https://docs.inji.io/readme/try-it-out)
* **Product guides**:
  * [Inji Wallet](https://docs.mosip.io/inji/inji-mobile-wallet/overview)
  * [Inji Web](https://docs.mosip.io/inji/inji-web/overview)
  * [Inji Verify](https://docs.mosip.io/inji/inji-verify/overview)

### eSignet

* **What to explore**: Authentication and consent-driven identity access flows.
* **Setup guide**: [eSignet Collab Setup Guide](https://docs.mosip.io/1.2.0/collab-getting-started-guide/collab-esignet-setup-guide)
* **Product guide**: [eSignet End User Guide](https://docs.esignet.io/esignet-end-user-guide)

## Developer Integration Path

Developers can use Collab to validate application integrations against the latest MOSIP APIs.

1. Select the target module and review its API and product documentation.
2. Connect your application to Collab endpoints and credentials.
3. Execute integration scenarios and interoperability checks.
4. Use the [MOSIP Community](https://community.mosip.io/) to discuss findings and implementation questions.

## Validation Checklist

* Access to Collab is confirmed.
* UIN is generated and usable in selected journeys.
* At least one end-to-end module scenario is completed.
* Integration behavior aligns with expected latest API responses.
* Observations and issues are documented for follow-up.

## Support and Next Steps

If you encounter issues during setup or testing:

* Raise queries through the [MOSIP Community](https://community.mosip.io/).
* Continue with module-specific guides for deeper configuration and testing.
