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