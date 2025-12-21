# Packet Creator Setup

Packet Creator - Setup & Run Guide\
**Purpose:** Step-by-step instructions to set up, build and run the MOSIP Packet Creator (used by DSL automation). This document assumes you are using the mosip-automation-tests repository and will produce a runnable JAR for the packet creator.

**Prerequisites**

* Git installed and configured (SSH or HTTPS access to the repository).
* Java (compatible version) installed. (Project historically used Java 11 and above; confirm your environment.)
* Maven installed (for building projects): mvn on PATH.
* Optional: An editor (VS Code / IntelliJ) and basic command-line familiarity.

**Note:** As part of MOSIP Packet Creator setup, you may need to generate a private key for your machine. Example expected filename: api-internal.cellbox1.mosip.net.88947.reg.key. Place the generated key in the private key folder inside the mosip-packet-creator project (document this step for the machine you're using).

**Repository & Paths**

**Repository (root):** [https://github.com/mosip/mosip-automation-tests.git](https://github.com/mosip/mosip-automation-tests.git)

**Packet Creator resources folder (contains centralized folder):** mosip-packet-creator/src/main/resources/dockersupport/centralized/mosip-packet-creator

Inside that path you will see a centralized folder. You should copy/replace the centralized folder into the target environment where you will run the packet creator jar.

**High-level Steps**

1. Clone the repository.
2. Build data-provider module (if applicable) first.
3. Build mosip-packet-creator module to produce the JAR (found under target/).
4. Copy and replace the centralized folder to the run location.
5. Place the newly created **JAR** and the **startup script** in the packet creator folder.
6. Update the startup script to point to the correct **JAR path** and set the required **environment variables**.
   * Use .bat for **Windows**
   * Use .sh for **Linux/macOS**
7. Execute the startup script to run the packet creator locally.
8. Verify the service is up using Swagger UI: [http://localhost:8080/v1/packetcreator/swagger-ui/index.html#/](http://localhost:8080/v1/packetcreator/swagger-ui/index.html#/)

Guidelines to Setup

1. **Clone repository**

**As a first step, clone this repository to your local machine/system** to obtain the Packet Creator source code and required resources.

[https://github.com/mosip/mosip-automation-tests.git](https://github.com/mosip/mosip-automation-tests.git)

```
cd mosip-automation-tests
```

2. **Build data-provider (if required)**

Some setups require building the Packet Creator service locally before running it.

**Example: Build the Packet Creator module**

```
mvn -DskipTests clean install
```

or to build a specific module, from the repo root:

```
cd mosip-packet-creator
mvn -DskipTests clean install
```

Use -DskipTests to speed up local builds. Remove if you want tests to run.

3. **Locate generated JAR**

After build, the packet creator JAR will appear in:

```
mosip-packet-creator/target/
```

Look for a file with a name like: mosip-packet-creator-\<version>.jar.

4. **Prepare centralized folder**

From the repo path:

mosip-packet-creator/src/main/resources/dockersupport/centralized/mosip-packet-creator

Copy the entire centralized folder to the folder where you'll run the packet creator. For example, create a directory on your machine:

Ensure the startup script (.bat or .sh) refers to the JAR using a **relative path**, for example:

./mosip-packet-creator.jar

Place the centralized content inside that folder (so the folder structure expected by the bat script and JAR is preserved).

5. **Place the JAR and .bat file**

Inside your run folder (example above), place the newly built JAR (from target) and the .bat file provided in the repository. The bat file typically contains the command to run the JAR with required JVM args and configuration paths.

**Important:** If the repo includes sample .bat scripts, open the .bat and edit any hard-coded paths.

6. **Edit the .bat (example placeholders)**

A typical Windows batch entry to run the JAR looks like this (example you will edit in the .bat):

```
set JAVA_HOME="C:\Program Files\Java\jdk-<version>"
set PATH=%JAVA_HOME%\bin;%PATH%
REM Path to JAR
set JAR_PATH=..\target\mosip-packet-creator-<version>.jar
REM Any additional arguments or environment variables
set CONFIG_DIR=./centralized
java -jar "%JAR_PATH%" --spring.config.location=%CONFIG_DIR%\application.yml
```

Edit JAR\_PATH and CONFIG\_DIR to point to the correct locations on your machine.

7. **Run packet creator**

Double-click the edited .bat or run it from a command prompt. Watch the console logs for startup messages.

8. **Verify using Swagger**

Open the browser and go to:

[http://localhost:8080/v1/packetcreator/swagger-ui/index.html#/](http://localhost:8080/v1/packetcreator/swagger-ui/index.html#/)

You should see the Packet Creator Swagger UI; if so, the service is up.

**Generating Private Key (machine-specific)**

If your setup requires a machine-specific private key, follow the internal [MOSIP instructions](https://docs.mosip.io/1.2.0/id-lifecycle-management/supporting-components/keymanager/keys#device-specific-keys) to generate it. Example filename pattern used historically:

api-internal.\<machine-hostname>.reg.key

Place this key in the private key folder inside the mosip-packet-creator project before running the service. Document the exact private-key generation command or tool that your infra uses and store keys securely.

### Create packet

#### Step 1 : **Server context file**

Before creating packets, we must initialize a **server context file** in Packet Creator.\
This is done by calling the **Create Server Context** API:

POST /v1/packetcreator/context/server/{contextKey}

The API stores all the properties you send in the request body into a context file named with the contextKey.\
The same contextKey must be used in all other Packet Creator APIs.

***

2. **Endpoint Details**

Use the below endpoint to initialize the server context, which is a mandatory prerequisite for creating a packet in Packet Creator.

* **Method:** POST
* **URL (example):**\
  [http://localhost:8080/v1/packetcreator/context/server/{contextKey}](http://localhost:8080/v1/packetcreator/context/server/%7BcontextKey%7D)
* **Path parameter:**
  * contextKey – Any string. This will be the filename of the context (for example, released-context, uat-context).

***

* **Request Body**
  * **Content-Type:** application/json
  * **Description:**\
    JSON object containing all environment-specific and packet-specific properties.

> **Create the request body by providing the required properties based on your environment and packet requirements. This request initializes the server context, which is required before creating a packet.**

* **Example request body:**

```
{

"mosip_regproc_client_secret": "",

"mosip.test.persona.locationsdatapath": "/profile_resource/location_data",

"Male": "MLE",

"userid": "solid1",

"mosip_resident_client_id": "mosip-resident-client",

"mosip.test.persona.datapath": "/profile_resource/",

"db-server": "api-internal.released.mosip.net",

"invalidEncryptedHashFlag": "",

"mosip_regprocclient_app_id": "regproc",

"mosip_crvs1_client_id": "mosip-crvs1-client",

"invalidOfficerIDFlag": "",

"admin_userName": "solid0",

"machineid": "28355",

"invalidDateFlag": "",

"scenario": "50:Resident Minor Child walks into registration center to get UIN card. Later tries to get another UIN by providing same Guardian",

"Female": "FLE",

"uin": "UIN",

"usePreConfiguredEmail": "",

"skipBiometricClassificationFlag": "",

"email_otp": "111111",

"mosip_idrepo_app_id": "idrepo",

"mosip.test.persona.facedatapath": "/profile_resource/face_data",

"mosip.test.persona.templatesdatapath": "/profile_resource/templates_data",

"introducerBiometrics": "introducerBiometrics",

"mosip.test.regclient.supervisorid": "solid1",

"IDSchemaVersion": "IDSchemaVersion",

"name": "fullName",

"mosip.test.regclient.userid": "solid1",

"introducerName": "introducerName",

"mosip_resident_client_secret": "",

"invalidCertFlag": false,

"mountPath": "../mountvolume",

"mosip.test.persona.irisdatapath": "/profile_resource/iris_data/",

"mosip_crvs1_client_secret": "",

"invalidCheckSum": "",

"ageCategory": "{\u0027INFANT\u0027:\u00270-5\u0027,\u0027MINOR\u0027:\u00276-17\u0027,\u0027ADULT\u0027:\u002718-200\u0027}",

"preconfiguredOtp": "111111",

"mosip.test.persona.documentsdatapath": "/profile_resource/documents_data/templates/",

"generatePrivateKey": false,

"introducerRID": "introducerRID",

"urlBase": "https://api-internal.released.mosip.net/",

"dob": "dateOfBirth",

"gender": "gender",

"signature": "valid",

"langCode": "eng",

"mosip_admin_client_id": "mosip-admin-client",

"individualBiometrics": "individualBiometrics",

"invalidIdSchemaFlag": "",

"user1": "solid1",

"mosip.test.persona.largedocumentpath": "/profile_resource/documents_data/",

"mosip_regproc_client_id": "mosip-regproc-client",

"emailId": "email",

"packetPath": "",

"otpTargetEmail": "",

"templateIDMeta": "/profile_resource/templates_data/IDMetaInfo.json",

"mosip.test.regclient.centerid": "34510",

"mosip_admin_client_secret": "",

"mosip.test.temp": "/packets/",

"enableDebug": "yes",

"keycloak_UserName": "admin",

"introducerUIN": "introducerUIN",

"mosip_admin_app_id": "admin",

"mosip.test.persona.fingerprintdatapath": "/profile_resource/fp_data",

"mosip.test.prereg.centerid": "10005",

"mosip.test.persona.namesdatapath": "/profile_resource/names_data",

"usePreConfiguredOtp": "false",

"mosip.test.regclient.supervisorpwd": "Techno@123",

"mosip.test.regclient.machineid": "28355",

"admin_password": "Techno@123"

}
```

**Note:** For a new environment, copy this JSON and update the values as per that environment.

***

4. **Important Fields to Configure per Environment (optional)**

When preparing the request body for a new environment:

* **Keycloak / user configuration**
  * "userid": "solid1"
  * "mosip.test.regclient.userid": "solid1"
  * "mosip.test.regclient.supervisorid": "solid1"
  * "admin\_userName": "solid0"
  * "user1": "solid1"
  * "keycloak\_UserName": "admin"\
    → Use valid users that exist in Keycloak and are mapped correctly for that environment.
* **Environment URLs and servers**
  * "db-server": "[api-internal.released.mosip.net](http://api-internal.released.mosip.net)"
  * "urlBase": "[https://api-internal.released.mosip.net/](https://api-internal.released.mosip.net/)"\
    → Replace with the API base URL of the current environment.
* **Machine and center mapping**
  * "machineid": "28355"
  * "mosip.test.regclient.machineid": "28355"
  * "mosip.test.regclient.centerid": "34510"\
    → Use machine ID and center ID created in **Admin UI / Master Data**.\
    Machine, center and users must be mapped to each other.
* **Client IDs and secrets (from environment)**
  * "mosip\_resident\_client\_secret": ""
  * "mosip\_crvs1\_client\_secret": ""
  * "mosip\_regproc\_client\_secret": ""\
    → Take these from the corresponding environment’s client configuration.
* **Paths**
  * All mosip.test.persona.\*datapath and mountPath, mosip.test.temp etc.\
    → Ensure these paths exist in the Packet Creator container / filesystem.

***

5. **Steps to Create Context Using Swagger UI**
6. Open Swagger UI for Packet Creator, e.g.\
   [http://localhost:8080/v1/packetcreator/swagger-ui/index.html](http://localhost:8080/v1/packetcreator/swagger-ui/index.html)
7. Expand **POST /context/server/{contextKey} – Initialize the server context**.
8. In the **contextKey** field, enter a name for this environment\
   (for example released-context, uat-context, local-context).
9. In the **Request body** section:
   * Select application/json.
   * Paste the JSON body (updated for the current environment).
10. Click **Execute**.
11. On success, Packet Creator creates a context file with the name {contextKey} and stores all these properties.
12. Use the same contextKey in all subsequent packet creation APIs.

***

&#x20;

#### Step 2 : Generate Persona (Resident) Data

&#x20;

After creating the **server context**, the next step is to generate **persona (resident) data**.\
This API creates demographic + biometric data and returns the **path to a JSON file** containing that data.

**1 Endpoint**

* **Method:** POST
* **URL (example):**\
  [http://localhost:8080/v1/packetcreator/persona/generate/{contextKey}](http://localhost:8080/v1/packetcreator/persona/generate/%7BcontextKey%7D)
* **Path parameter:**
  * contextKey – Use the **same contextKey** that you used while creating the server context.

**2 Request Body**

* **Content-Type:** application/json
* **Description:**\
  Specifies what type of resident data should be generated (age group, which biometrics are present, etc.).

**Example request body**

```
{
  "requests": {
    "PR_ResidentAttribute": {
      "Iris": "true",
      "Finger": "true",
      "Gender": "Male",
      "Face": "true",
      "Age": "RA_Adult"
    }
  }
}
```

**Field description (PR\_ResidentAttribute)**

* "Iris": "true"
  * true → Iris biometric will be generated.
  * false → Iris will **not** be generated and will be marked as an exception.
* "Finger": "true"
  * true → Fingerprint biometric will be generated.
  * false → Fingerprint will **not** be generated and will be marked as an exception.
* "Face": "true"
  * true → Face biometric will be generated.
  * false → Face will **not** be generated and will be marked as an exception.
* "Gender": "Male"
  * Gender of the resident. Typical values: "Male" or "Female".
* "Age": "RA\_Adult"
  * Age category of the resident. Common values (depending on configuration):
    * "RA\_Infant" – infant
    * "RA\_Minor" – minor
    * "RA\_Adult" – adult

You can adjust these values based on the scenario you want to test (for example, minor with only fingerprint and no iris).

***

**3 Response**

* On success, the API returns a JSON response that includes the **file path** of the generated resident data JSON.
* That JSON file contains:
  * Demographic data (name, age, gender, address, etc.)
  * Biometric data (face, iris, fingerprint) as per the flags in the request.

This JSON file path will be used in the **next step** of packet creation (while generating the registration packet).

***

#### Step 3 : Create Packet Template <br>

Once the resident persona JSON is generated, the next step is to create a **packet template**.\
This template will later be used to generate and upload the final registration packet.

**1 Endpoint**

* **Method:** POST
* **URL (example):**\
  [http://localhost:8080/v1/packetcreator/packet/template/{process}/{qualityScore}/{genarateValidCbeff}/{contextKey}](http://localhost:8080/v1/packetcreator/packet/template/%7Bprocess%7D/%7BqualityScore%7D/%7BgenarateValidCbeff%7D/%7BcontextKey%7D)**2 Path Parameters**

1. **process** (string – required)\
   Type of registration process. Typical values:
   * "NEW" – New registration
   * "UPDATE" – Update my data
   * "LOST" – Lost/damaged card, etc.
2. **qualityScore** (string – required)\
   Minimum biometric quality score required (e.g. 70, 80).\
   This is used to validate biometric samples.
3. **genarateValidCbeff** (boolean – required)
   * true → Generate valid CBEFF for biometrics.
   * false → Do not generate CBEFF.\
     For normal packet creation, set this to **true**.
4. **contextKey** (string – required)\
   Use the **same contextKey** that you created in **Step 1 (Create Server Context)**.

***

**3 Request Body**

* **Content-Type:** application/json
* **Description:**\
  List of persona JSON files that should be used to create the packet template.

You can either:

* Use the **persona JSON path** returned from **Generate Resident Data** API, or
* Use the path of any existing persona file that already contains demographic + biometric data.

**Example request body**

```
{
  "personaFilePath": [
    "/tmp/residents_10144790471760567615/9290063606.json"
  ]
}
```

* personaFilePath – Array of string paths.\
  Each entry is the full path of a persona JSON file.\
  (The example above uses the file path returned by the previous _Generate Resident Data_ call.)

***

**4 Response**

On success, the API returns a response containing the **packet template path**.\
This path points to the generated template that will be used in the **next step** to actually create and upload the registration packet.

***

#### Step 4 : Create Packet <br>

After generating the **packet template**, the next step is to create the actual **registration packet (ZIP)**.

**1 Endpoint**

* **Method:** POST
* **URL (example):**\
  [http://localhost:8080/v1/packetcreator/packetcreator/{contextKey}](http://localhost:8080/v1/packetcreator/packetcreator/%7BcontextKey%7D)

**2 Path Parameter**

* contextKey – Same context key used in all previous steps (server context, persona, template).

***

**3 Request Body**

* **Content-Type:** application/json

**Example**

```
{
  "process": "NEW",
  "idJsonPath": "/tmp/packets_13561113746835624521/9290063606/REGISTRATION_CLIENT/NEW/rid_id/ID.json",
  "source": "REGISTRATION_CLIENT",
  "templatePath": "/tmp/packets_13561113746835624521/9290063606"
}
```

**Field description**

* "process": "NEW"
  * Registration process type. Should match what you used while creating the packet template.
  * Typical values: "NEW", "UPDATE", "LOST".
* "templatePath"
  * Folder path of the packet template generated in **Step 3 (Create Packet Template)**.
  * Example: /tmp/packets\_13561113746835624521/9290063606
* "idJsonPath"
  * Path of ID.json inside the same template folder.
  * In general:\
    idJsonPath = \<templatePath> + "/REGISTRATION\_CLIENT/\<process>/rid\_id/ID.json"
  * Example:\
    /tmp/packets\_13561113746835624521/9290063606/REGISTRATION\_CLIENT/NEW/rid\_id/ID.json
* "source": "REGISTRATION\_CLIENT"
  * Source of packet creation. For standard registration packets, use REGISTRATION\_CLIENT.

***

**4 Response**

On success, the API returns the **created packet path**, which points to the final ZIP file.\
Example:

/tmp/pktcreator2394404641898443616/10272102581008020251121060057-10272\_10258-20251121060057.zip

This ZIP file is the registration packet that can be synced/uploaded to MOSIP (next steps: packet sync / upload APIs).

***

#### Step 5 : Sync RID from Packet

&#x20;

After the packet ZIP is created, we need to **sync the packet to MOSIP** and obtain the **RID (Registration ID)**.\
This is done using the ridsync API.

**1 Endpoint**

* **Method:** POST
* **URL (example):**\
  [http://localhost:8080/v1/packetcreator/ridsync/{contextKey}](http://localhost:8080/v1/packetcreator/ridsync/%7BcontextKey%7D)

**2 Path Parameter**

* contextKey – Same context key used in all previous steps.

***

**3 Request Body**

* **Content-Type:** application/json

**Example**

```
{
  "process": "NEW",
  "phone": "phone",
  "name": "name",
  "supervisorComment": "supervisorComment",
  "containerPath": "/tmp/pktcreator2394404641898443616/10272102581008020251121060057-10272_10258-20251121060057.zip",
  "supervisorStatus": "APPROVED",
  "email": "email"
}
```

**Field description**

* "process": "NEW"
  * Same process value used earlier (NEW, UPDATE, LOST, etc.).
* "containerPath"
  * Full path of the **packet ZIP file** generated in **Step 4 (Create Packet)**.
  * Example:\
    /tmp/pktcreator2394404641898443616/10272102581008020251121060057-10272\_10258-20251121060057.zip
* "name"
  * Name of the operator / user performing the sync (can be any valid string).
* "phone"
  * Phone number of the operator (for audit/logging).
* "email"
  * Email ID of the operator.
* "supervisorStatus": "APPROVED"
  * Approval status from supervisor. Typical values: "APPROVED" / "REJECTED".
* "supervisorComment"
  * Any comment from supervisor (optional text).

***

**4 Response**

* On success, the API returns the **RID (Registration ID)** generated for the packet.
* This RID can then be used in downstream flows (registration processing, status check, resident portal, etc.).

#### Step 6 : Sync RID from the ZIP Packet

&#x20;

After the packet ZIP is generated, call this API to **upload/sync the packet** and get the **RID (Registration ID)**.

**1 Endpoint**

* **Method:** POST
* **URL (example):**\
  [http://localhost:8080/v1/packetcreator/ridsync/{contextKey}](http://localhost:8080/v1/packetcreator/ridsync/%7BcontextKey%7D)
* **Path parameter:**
  * contextKey – Same context key used in all previous steps.

***

**2 Request Body**

* **Content-Type:** application/json

```
{
  "process": "NEW",
  "phone": "phone",
  "name": "name",
  "supervisorComment": "supervisorComment",
  "containerPath": "/tmp/pktcreator2394404641898443616/10272102581008020251121060057-10272_10258-20251121060057.zip",
  "supervisorStatus": "APPROVED",
  "email": "email"
}
```

**Fields**

* "process" – Registration process (NEW, UPDATE, LOST, etc.).
* "containerPath" – Full path of the **packet ZIP file** created in the **Create Packet** step.
* "name" – Operator name doing the sync.
* "phone" – Operator phone.
* "email" – Operator email.
* "supervisorStatus" – e.g. "APPROVED" or "REJECTED".
* "supervisorComment" – Any approval comment.

***

**3 Response**

On success, the API returns the **RID (Registration ID)** generated for this packet.\
Use this RID for all downstream registration status and processing flows.

&#x20;

#### Step 7 : Packet Sync (Upload Packet to MOSIP)

&#x20;

After generating the packet ZIP and obtaining the RID, the next step is to **upload / sync the packet** into MOSIP Packet Receiver.\
This is done using the **packetSync** API.

**1 Endpoint**

* **Method:** POST
* **URL (example):**\
  [http://localhost:8080/v1/packetcreator/packetsync/{contextKey}](http://localhost:8080/v1/packetcreator/packetsync/%7BcontextKey%7D)

**2 Path Parameter**

* contextKey – same context key used throughout all earlier steps.

***

**3 Request Body**

* **Content-Type:** application/json

**Example Request**

```
{
  "personaFilePath": [
    "/tmp/pktcreator2394404641898443616/10272102581008020251121060057-10272_10258-20251121060057.zip"
  ]
}
```

**Field Details**

* "personaFilePath"
  * This must contain the **path to the final packet ZIP file** (created in Step 4).
  * The value must be inside an array (list), even if only one ZIP is being uploaded.

***

**4 Expected Response**

A successful sync returns a status message like:

```
{

  "status": "Packet has reached Packet Receiver"

}
```

**Meaning**

Packet upload was successful\
Packet has been received by MOSIP Packet Receiver\
Packet is now eligible for processing by Registration Processor pipeline

***

#### Step 8 : Check RID Status

&#x20;

After syncing the packet, use this API to check whether the **RID is processed successfully** or if there is any issue with the packet.

**1 Endpoint**

* **Method:** GET
* **URL (example):**\
  [http://localhost:8080/v1/packetcreator/resident/status/{rid}/{contextKey}](http://localhost:8080/v1/packetcreator/resident/status/%7Brid%7D/%7BcontextKey%7D)

**2 Path Parameters**

* rid – RID returned by the **ridsync** API.
* contextKey – Same context key used for all previous steps.

No request body is required.

***

**3 Response**

On success (HTTP 200), the API returns the **current status of the RID**, for example:

```
{
  "rid": "10272102581008020251121060057",
  "status": "PROCESSED",
  "message": "Packet successfully processed"
}
```

Possible values for "status" (examples):

* "PROCESSED" – Packet completed successfully.
* "IN\_PROGRESS" – Packet is still under processing.
* "FAILED" / "RE\_REGISTER" – Packet has some problem (validation failure, de-duplication failure, etc.).\
  For example you may see messages like:\
  "message": "Packet failed – re-register required".

&#x20;

***

#### Step 9 : Get UIN from RID <br>

This API is used to fetch the **UIN** generated for a successfully processed RID.

**Endpoint**

**Method:** GET\
**URL:**

/resident/uin/{rid}/{contextKey}

**Parameters (Path)**

| **Name**   | **Description**                         |
| ---------- | --------------------------------------- |
| rid        | The RID you received after packet sync  |
| contextKey | Same context key used in previous steps |

**Request Details**

* **No request body required**
* Only pass RID and contextKey in the URL

**Example Call**

GET /resident/uin/10272102581008020251121060057/releasedContext

**Response Format**

The response will directly return the **UIN number only**, for example:

2062157103

&#x20;

&#x20;

***

### Closure Notes – Packet Creation and Processing Flow

### &#x20;

You have now successfully completed the full end-to-end lifecycle of MOSIP Packet Creation using Packet Creator:

**What has been achieved**

✔ Environment-specific context initialized\
✔ Persona (resident) data generated\
✔ Packet template created\
✔ Final packet ZIP generated\
✔ Packet synced/uploaded to MOSIP\
✔ RID received\
✔ RID processing status checked\
✔ UIN retrieved

***

**Important Reminders & Best Practices**

**1. Same contextKey must be used throughout**

If a different contextKey is used, APIs will not find:

* persona data
* template data
* packet zip
* RID history

***

**2. Machine, Center and Users must be mapped**

Packet processing will fail if:

* machineId is wrong
* centerId not linked
* user not mapped to center
* supervisor not mapped

***

**3. Environment-specific fields must be updated**

Especially:

* db-server
* urlBase
* client secrets
* Keycloak usernames
* machineId and centerId

***

**4. Biometrics rules affect packet acceptance**

Examples:

* Missing biometrics → may trigger "Re-Register"
* Low quality score → validation failure
* Age + gender combinations must be valid

***

**5. Common processing outcomes**

| **Result**        | **Meaning**                              |
| ----------------- | ---------------------------------------- |
| **PROCESSED**     | Packet is valid and completed            |
| **UIN generated** | Registration successful                  |
| **RE-REGISTER**   | Failure – resident must re-enroll        |
| **FAILED**        | Data, checksum, schema, or mapping issue |
| **IN-PROGRESS**   | Wait and retry later                     |

***

**Recommended Validation Before Using Packet**

* Try New Registration packet with below scenarios:
  * Try one Adult with Full biometrics
  * Try one Minor with guardian needed
  * Try one Infant with no biometrics
  * Try one Adult with biometric exception
* Try one Update packet
* Try one Lost UIN packet
* Try one biometric correction packet

***

**🛠 Troubleshooting Quick Guide**

**If packet fails at sync stage**

check containerPath\
check ZIP exists\
check permissions

**If packet fails processing**

verify machine–center mapping\
verify schema version\
verify biometrics flags

**If UIN not generated**

check RID status first\
wait if still processing
