# CP

### Release Summary
- **Name**: Android Registration Client v1.0.0
- **Version**: 1.0.0
- **Release Date**: 4 Sep 2024
- **Support**: Beta
- **MOSIP Compatibility**: 1.2.0

### Overview
The Android Registration Client is a tablet application offering a mobile version of the desktop Registration Client, enabling use across Android devices for field mobility.

### Included Features
- Operator Onboarding: Self-onboarding of operators.
- Update Operator Onboarding: Update operator biometrics.
- Dashboard: View packet creation/upload statistics, mapped users, and their status.
- (Includes prior DP1 and 0.9.0 features.)

### Repository & Tag
- Repository: android-registration-client
- Tag: v1.0.0

### Build and Deploy
Refer to the Developer Guide. [Link Placeholder]

### Configuration
Refer to the Configuration Guide. [Link Placeholder]

### User Guide
Refer to the Android Registration User Guide. [Link Placeholder]

### Known Issues
Refer to the Known Issues list. [Link Placeholder]













The following API endpoints have been **deprecated** in PMS 1.3.0-beta.3 and replaced with enhanced alternatives:
### **MISP License Management**

| Deprecated Endpoint                       | Replacement                                 | Description                        |
|-------------------------------------------|---------------------------------------------|------------------------------------|
| `POST /misps`                            | `POST /misp-licenses`                       | Generate MISP license key          |
| `PUT /misps`                             | `PATCH /misp-licenses/{partnerId}`          | Update MISP license key details    |
| `GET /misps`                             | `GET /misp-licenses`                        | Retrieve MISP license key details  |
| `GET /misps/{mispId}/licenseKey`         | `PUT /misp-licenses/{partnerId}`            | Re-generate MISP license key       |
| `POST /misps/filtervalues`               | `GET /misp-licenses`                        | Filter MISP details                |
| `POST /misps/search`                     | `GET /misp-licenses`                        | Search MISP details                |

### **Partner Registration**

| Deprecated Endpoint                       | Replacement                | Description                      |
|-------------------------------------------|----------------------------|----------------------------------|
| `POST /partners`                         | `POST /partners/v3`        | Partner self-registration        |
| `POST /partners/v2`                      | `POST /partners/v3`        | Partner registration (enhanced)  |

### **Partner API Key Management**

| Deprecated Endpoint                       | Replacement                | Description                                         |
|-------------------------------------------|----------------------------|-----------------------------------------------------|
| `GET /partner-api-keys`                   | `GET /partner-api-keys/v2` | Retrieve partner API keys with enhanced filtering    |

**Migration Notice**: Deprecated endpoints will be removed in future releases. Partners and integrators should update their implementations to use the replacement endpoints to ensure continued functionality and access to enhanced features.