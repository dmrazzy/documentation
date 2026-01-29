# MOSIP CRVS Integration Guide

```sh
pandoc \
  docs/interoperability/integrations/test/README.md \
  docs/interoperability/integrations/test/integration-overview-and-context/README.md \
  docs/interoperability/integrations/test/integration-overview-and-context/core-integration-principles.md \
  docs/interoperability/integrations/test/integration-overview-and-context/integration-principles-boundaries-and-real-world-implications.md \
  docs/interoperability/integrations/test/integration-patterns-and-workflow/README.md \
  docs/interoperability/integrations/test/integration-patterns-and-workflow/integration-flows/README.md \
  docs/interoperability/integrations/test/integration-patterns-and-workflow/integration-flows/birth-registration-and-uin-issuance.md \
  docs/interoperability/integrations/test/integration-patterns-and-workflow/integration-flows/death-registration-and-identity-status-update.md \
  docs/interoperability/integrations/test/integration-patterns-and-workflow/integration-flows/demographic-data-updates.md \
  docs/interoperability/integrations/test/integration-patterns-and-workflow/rare-scenarios/README.md \
  docs/interoperability/integrations/test/integration-patterns-and-workflow/rare-scenarios/fraudulent-birth-registrations-national-id-deactivation-request-from-crvs.md \
  docs/interoperability/integrations/test/integration-patterns-and-workflow/rare-scenarios/reactivation-of-deactivated-national-id.md \
  docs/interoperability/integrations/test/integration-patterns-and-workflow/rare-scenarios/fraud-death-case-reversal-of-the-death-flag.md \
  docs/interoperability/integrations/test/configurations-and-operations/README.md \
  docs/interoperability/integrations/test/configurations-and-operations/policy-configuration-and-customization.md \
  docs/interoperability/integrations/test/configurations-and-operations/operational-considerations.md \
  docs/interoperability/integrations/test/api-reference-and-data-models.md \
  docs/interoperability/integrations/test/security-and-authentication.md \
  docs/interoperability/integrations/test/notifications-and-event-handling.md \
  docs/interoperability/integrations/test/error-handling-and-reconciliation.md \
  -o mosip-crvs-integration-guide.docx \
  --metadata title="MOSIP-CRVS Integration Guide" \
  --metadata author="MOSIP Documentation Team" \
  --metadata date="$(date +%Y-%m-%d)" \
  --toc \
  --toc-depth=3 \
  --highlight-style=tango \
  --standalone

```