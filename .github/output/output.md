## Validation Report

Status: PASS (rendered)

Deterministic checks:
- RULE-001 Required sections: PASS
- RULE-002 Version format (semver): PASS (`1.3.0`)
- RULE-003 Date format (`YYYY-MM-DD` in YAML): PASS
- RULE-005 Placeholder text (TBD/TODO/FIXME/[UPDATE THIS]): PASS
- RULE-006 Migration guide for breaking changes: PASS (no breaking changes identified)
- RULE-008 Language and clarity: PASS (plain language, active voice)
- RULE-011 Documentation links for new/enhanced items: PASS
- RULE-013 Prohibited internal/sensitive content: PASS

Notes:
- Source content did not provide explicit bug-fix records or API breaking changes; these sections are intentionally omitted from YAML content.
- Compatibility statements are retained from source inputs.

## Change Classification

| Category | Classified Items |
|---|---|
| ✨ New Features | Operator biometric restriction during applicant registration; Editable scheduled job cron setting; Additional sync and batch-job controls |
| 🐛 Bug Fixes | Not explicitly listed in source content |
| 💥 Breaking Changes | None identified |
| 🔄 Migration Guide | Not required (no breaking changes identified) |
| 🔧 Internal / Chores | Audit parity improvements with desktop registration client; Configuration externalization for fixed system values |

## YAML (Source of Truth)

```yaml
metadata:
	version: "1.3.0"
	releaseDate: "2026-04-11"
	releaseType: "Minor"
	productName: "Android Registration Client"
	productNameAbbr: "ARC"
	rolloutTimeline: "Beta rollout; date to be announced"
	targetAudience:
		- "Registration Operators"
		- "Supervisors"
		- "Platform Administrators"

summary:
	headline: "Android Registration Client 1.3.0 improves configurability, auditability, and registration integrity"
	paragraphs:
		- text: "This release improves operational flexibility by moving previously fixed system values into configuration settings. Teams can adapt behavior across environments without source code edits, which shortens turnaround time for routine operational updates."
			sectionLabel: "Purpose"
		- text: "The release also adds broader audit logging and enforces operator biometric restrictions during applicant registration. Together, these changes improve traceability, strengthen accountability, and reduce the risk of incorrect biometric capture."
			sectionLabel: "ThemesAndFocus"
		- text: "This beta update is intended for operators, supervisors, and administrators running registrations in mobile and remote deployment contexts where reliability, transparency, and policy compliance are critical."
			sectionLabel: "Audience"
	keyBenefits:
		- "Operational changes can be applied through configuration instead of source edits"
		- "Expanded audit visibility improves compliance tracking and troubleshooting"
		- "Operator biometric restriction strengthens identity data integrity"

content:
	newFeatures:
		- name: "Operator Biometric Restriction"
			description: "The client now rejects operator biometrics during applicant registration to prevent operator data from being captured as applicant identity data."
			useCase: "Prevent accidental or intentional misuse of operator biometrics in enrollment workflows."
			keyCapabilities:
				- "Blocks operator biometric acceptance in applicant capture flow"
				- "Preserves applicant biometric authenticity"
				- "Improves compliance for biometric handling"
			documentation:
				title: "Android Registration Client v1.3.0"
				url: "https://docs.mosip.io/1.2.0/releases/release-notes"
			relatedIssues:
				- "RCF-351"

		- name: "Scheduled Job Cron Configurability"
			description: "Scheduled job cron values can now be adjusted from configuration, reducing operational friction during cadence changes."
			useCase: "Allow administrators to tune sync and batch schedules without development intervention."
			keyCapabilities:
				- "Editable cron configuration"
				- "Faster operational schedule tuning"
			documentation:
				title: "Android Registration Client Configuration Guide"
				url: "https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-configuration"
			relatedIssues:
				- "RCF-1278"

		- name: "Additional Sync and Batch Job Controls"
			description: "The release adds sync and batch-job capabilities to improve control over data movement and scheduled processing."
			useCase: "Support deployment teams that require tighter control over synchronization and batch execution windows."
			keyCapabilities:
				- "Expanded sync handling"
				- "Batch job support improvements"
			documentation:
				title: "Android Registration Client Developer Guide"
				url: "https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-developer-guide"
			relatedIssues:
				- "RCF-1275"

	enhancedFeatures:
		- name: "Configuration Externalization"
			improvement: "Several fixed system values were moved into configuration settings so administrators can adapt behavior per environment without code updates."
			previousBehavior: "Multiple operational values required development updates to change behavior."
			newBehavior: "Administrators can adjust those values through configuration files and settings."
			impactOnExistingUsers: "Faster operational updates and lower dependency on development teams for routine tuning."
			migrationRequired: false
			documentation:
				title: "Android Registration Client Configuration Guide"
				url: "https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-configuration"

		- name: "Audit Logging Coverage"
			improvement: "Audit logging now captures more key actions and workflows to strengthen traceability and compliance evidence."
			previousBehavior: "Audit visibility was lower and less aligned with desktop parity expectations."
			newBehavior: "Critical workflow actions are logged with better trace context for support and governance."
			impactOnExistingUsers: "Teams can diagnose issues faster and maintain clearer accountability records."
			migrationRequired: false
			documentation:
				title: "Android Registration Client v1.3.0"
				url: "https://docs.mosip.io/1.2.0/releases/release-notes"

	knownIssues:
		- title: "Open issues remain under the ARC release parent"
			description: "Some issues are still open and may affect specific edge flows depending on deployment profile."
			workaround: "Track and apply mitigations documented in the issue tracker before broad rollout."
			issueTracker: "https://mosip.atlassian.net/issues/?jql=parent%3Drcf-31%20and%20issuetype%3Dbug%20and%20status%20not%20in%20%28closed%2C%20Canceled%29%20and%20labels%21%3DARC_Real_Device"

	supportResources:
		documentation:
			releasePage: "https://docs.mosip.io/1.2.0/releases/release-notes"
			upgradeGuide: "https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-developer-guide"
			troubleshootingGuide: "https://mosip.atlassian.net/issues/?jql=parent%3Drcf-31%20and%20issuetype%3Dbug%20and%20status%20not%20in%20%28closed%2C%20Canceled%29%20and%20labels%21%3DARC_Real_Device"
			apiReference: "https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-developer-guide"
		supportChannels:
			shortTermSupport: "Available during beta rollout window"
			longTermSupport: "To be announced after GA"
			issueTracker: "https://mosip.atlassian.net/issues/?jql=parent%3Drcf-31%20and%20issuetype%3Dbug%20and%20status%20not%20in%20%28closed%2C%20Canceled%29%20and%20labels%21%3DARC_Real_Device"
```

## Rendered Markdown (Publishable)

```markdown
# Release Notes

**Version**: 1.3.0  
**Released**: 11 April 2026  
**Release Type**: Minor  
**Product**: Android Registration Client (ARC)

> **📋 Quick Reference**
> - **Timeline**: Beta rollout; date to be announced
> - **Target Audience**: Registration Operators, Supervisors, Platform Administrators

## Release Summary

### Android Registration Client 1.3.0 improves configurability, auditability, and registration integrity

This release improves operational flexibility by moving previously fixed system values into configuration settings. Teams can adapt behavior across environments without source code edits, which shortens turnaround time for routine operational updates.

The release also adds broader audit logging and enforces operator biometric restrictions during applicant registration. Together, these changes improve traceability, strengthen accountability, and reduce the risk of incorrect biometric capture.

This beta update is intended for operators, supervisors, and administrators running registrations in mobile and remote deployment contexts where reliability, transparency, and policy compliance are critical.

**Key Benefits**:
- ✨ Operational changes can be applied through configuration instead of source edits
- ✨ Expanded audit visibility improves compliance tracking and troubleshooting
- ✨ Operator biometric restriction strengthens identity data integrity

## What's New

### New Features

#### Operator Biometric Restriction

The client now rejects operator biometrics during applicant registration to prevent operator data from being captured as applicant identity data.

- **Use Case**: Prevent accidental or intentional misuse of operator biometrics in enrollment workflows.
- **Key Capabilities**:
	- Blocks operator biometric acceptance in applicant capture flow
	- Preserves applicant biometric authenticity
	- Improves compliance for biometric handling
- **Documentation**: [Android Registration Client v1.3.0](https://docs.mosip.io/1.2.0/releases/release-notes)
- **Related Issues**: RCF-351

#### Scheduled Job Cron Configurability

Scheduled job cron values can now be adjusted from configuration, reducing operational friction during cadence changes.

- **Use Case**: Allow administrators to tune sync and batch schedules without development intervention.
- **Key Capabilities**:
	- Editable cron configuration
	- Faster operational schedule tuning
- **Documentation**: [Android Registration Client Configuration Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-configuration)
- **Related Issues**: RCF-1278

#### Additional Sync and Batch Job Controls

The release adds sync and batch-job capabilities to improve control over data movement and scheduled processing.

- **Use Case**: Support deployment teams that require tighter control over synchronization and batch execution windows.
- **Key Capabilities**:
	- Expanded sync handling
	- Batch job support improvements
- **Documentation**: [Android Registration Client Developer Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-developer-guide)
- **Related Issues**: RCF-1275

### Enhancements

#### Configuration Externalization

Several fixed system values were moved into configuration settings so administrators can adapt behavior per environment without code updates.

| Aspect | Details |
|--------|---------|
| **Previous Behavior** | Multiple operational values required development updates to change behavior. |
| **New Behavior** | Administrators can adjust those values through configuration files and settings. |
| **User Impact** | Faster operational updates and lower dependency on development teams for routine tuning. |

**Documentation**: [Android Registration Client Configuration Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-configuration)

#### Audit Logging Coverage

Audit logging now captures more key actions and workflows to strengthen traceability and compliance evidence.

| Aspect | Details |
|--------|---------|
| **Previous Behavior** | Audit visibility was lower and less aligned with desktop parity expectations. |
| **New Behavior** | Critical workflow actions are logged with better trace context for support and governance. |
| **User Impact** | Teams can diagnose issues faster and maintain clearer accountability records. |

**Documentation**: [Android Registration Client v1.3.0](https://docs.mosip.io/1.2.0/releases/release-notes)

## Compatibility

The Android Registration Client has the following stated compatibility:

| MOSIP Platform Version | Compatibility |
|---|---|
| [1.2.0](https://docs.mosip.io/1.2.0/releases/release-notes) | Compatible |
| 1.3.0 | Not compatible |

## ⚠️ Known Issues

### Open issues remain under the ARC release parent

Some issues are still open and may affect specific edge flows depending on deployment profile.

**Workaround**: Track and apply mitigations documented in the issue tracker before broad rollout.

**Issue Tracker**: [Jira Query](https://mosip.atlassian.net/issues/?jql=parent%3Drcf-31%20and%20issuetype%3Dbug%20and%20status%20not%20in%20%28closed%2C%20Canceled%29%20and%20labels%21%3DARC_Real_Device)

## Support & Resources

### 📚 Documentation
- [Release Page](https://docs.mosip.io/1.2.0/releases/release-notes)
- [Upgrade Guide](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-developer-guide)
- [Troubleshooting](https://mosip.atlassian.net/issues/?jql=parent%3Drcf-31%20and%20issuetype%3Dbug%20and%20status%20not%20in%20%28closed%2C%20Canceled%29%20and%20labels%21%3DARC_Real_Device)
- [API Reference](https://docs.mosip.io/1.2.0/modules/android-registration-client/android-registration-client-developer-guide)

### 🤝 Support & Community
- **Short-Term Support**: Available during beta rollout window
- **Long-Term Support**: To be announced after GA
- [Issue Tracker](https://mosip.atlassian.net/issues/?jql=parent%3Drcf-31%20and%20issuetype%3Dbug%20and%20status%20not%20in%20%28closed%2C%20Canceled%29%20and%20labels%21%3DARC_Real_Device)
```
