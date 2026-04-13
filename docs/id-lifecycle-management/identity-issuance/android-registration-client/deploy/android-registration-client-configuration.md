# Configuration Guide

## Overview

This Android Reg Client Configuration Guide lists down important properties that can be configured based on preference. The properties listed here are not exhaustive, but a checklist to review properties that are likely to be configured.

All the properties are synced to the Android Registration Client from registration-default.properties file.

## Configuration Files

application-default.properties

registration-default.properties

## SBI related configurations

Timeouts in milliseconds set during any http calls to SBI.

```
mosip.registration.mdm.connection.timeout=10000
mosip.registration.mdm.RCAPTURE.connection.timeout=40000
mosip.registration.mdm.MOSIPDINFO.connection.timeout=5000
mosip.registration.mdm.MOSIPDISC.connection.timeout=5000

```

Quality score threshold based on modality, Possible values 1 to 100

```
mosip.registration.leftslap_fingerprint_threshold=40
mosip.registration.rightslap_fingerprint_threshold=40
mosip.registration.thumbs_fingerprint_threshold=40
mosip.registration.iris_threshold=60
mosip.registration.face_threshold=9

```

Quality score threshold based on modality for operator authentication. Possible values 1 to 100

```
mosip.fingerprint_authentication.quality_score=30
mosip.iris_authentication.quality_score=30
mosip.face_authentication.quality_score=30

```

## Batch Size

Jobs like RID sync, packet upload, status sync is carried out in batches, number of registration records to be executed in a batch on every trigger.

```
mosip.registration.rid_sync_batch_size=5
mosip.registration.packet_upload_batch_size=5
mosip.registration.status_sync_batch_size=5

```

## Scheduled Jobs

Default CRON expression for scheduling the Jobs.

`mosip.registration.sync_jobs_restart_freq=0 0 */11 ? * *`

### GPS Device Connection

Enable GPS device for capturing the geo-location. If y, GPS device will be enabled. If n, GPS device will be disabled. `mosip.registration.gps_device_enable_flag=N`

### Default Values to be used for Auditing

Application ID `mosip.registration.audit_application_id=REG`ß

Application Name `mosip.registration.audit_application_name=REGISTRATION`

Default HOST IP Address, if unable to get the Host IP address `mosip.registration.audit_default_host_ip=120.0.0.0`ß

Default Host Name, if unable to get the Host Name `mosip.registration.audit_default_host_name=localhost`

### Global Properties

```
mosip.kernel.prid.length=${mosip.kernel.prid.length}
mosip.kernel.uin.length=${mosip.kernel.uin.length}
mosip.kernel.vid.length=${mosip.kernel.vid.length}
```

## Other Configurations

Enables / disables reviewer authentication on any biometric exception during registration

`mosip.registration.reviewer_authentication_configuration=Y`

If enabled cross-checks of residents biometrics with locally stored operator biometric templates.

`mosip.registration.mds.deduplication.enable.flag=N`

Minimum disk space required to create a packet - in MB

`mosip.registration.disk_space_size=5`

Maximum no. of days for approved packet pending to be synced to server beyond which Registration Client is frozen for registration

`mosip.registration.last_export_registration_config_time=100`

No. of days beyond audit creation date to delete audits

`mosip.registration.audit_log_deletion_configured_days=10`

Maximum duration to which registration is permitted without sync of master data

`mosip.registration.sync_transaction_no_of_days_limit=5`

Allowed number of invalid login attempts

`mosip.registration.invalid_login_count=50`

{% hint style="info" %}
**Note**_: As we are using **Keycloak** for login authentication, we need to add the same configuration for **login attempts** and **account lock (retry) time**._
{% endhint %}

Configuration used to check if any sync job is missed / failed beyond expected days, this configuration is checked everytime operator clicks on any registration process. We follow below convention to create this config key.

`mosip.registration.job api name as in sync_job_def table.frequency=value in days`

Maximum no. of days for a packet pending EOD approval beyond which client is frozen for registration `mosip.registration.reg_pak_max_time_apprv_limit=50`

Maximum no. of days without running Registration User Mapping Sync Job beyond which client is frozen for registration `mosip.registration.regUserMappingSyncJob.frequency=190`

Maximum no. of days without running Pre-Registration Packet Deletion beyond which client is frozen for registration `mosip.registration.preRegistrationPacketDeletionJob.frequency=190`

Maximum no. of days without running Registration Packet Sync Job beyond which client is frozen for registration `mosip.registration.registrationPacketSyncJob.frequency=190`

Maximum no. of packets pending EOD approval beyond which client is frozen for registration `mosip.registration.packet.maximum.count.offline.frequency=70`

System will capture the lattitue and longitude of machine every time when a new registration is performed `mosip.registration.geo.capture.frequency=y`

used to fill env in the MDM rcapture request `mosip.registration.server_profile=Staging`

File Type of the Document Scanned `mosip.registration.document_scanner_doctype=PDF`

Maximum length of the Password to be entered `mosip.registration.username_pwd_length=50`

Max Document size in Bytes allowed to be uploaded `mosip.registration.document_size=2000000`

Maximum age limit `mosip.registration.max_age=150`

fields that should NOT be overridden with prereg packet data, it is comma separated list of field IDs: `mosip.registration.fields.to.retain.post.prid.fetch=consent,consentText,preferredLang` Registration Packet Local Storage File Path `mosip.registration.registration_packet_store_location=..//PacketStore`

offline jobs, which will be not part of manual sync, add by comma (,) separator if to add more jobs `mosip.registration.jobs.offline=DEL_J00013,RDJ_J00010,ADJ_J00012,PVS_J00015`

restart jobs, which requests application to be restarted post the job success, add by comma (,) separator if to add more jobs `mosip.registration.jobs.restart=RCS_J00005`

Untagged jobs, which will be not part of manual sync but only from scheduler, add by comma (,) separator if to add more jobs `mosip.registration.jobs.unTagged=PDS_J00003`

Timeout used in MDM request. `mosip.registration.capture_time_out=10000`

## Date Formats

Date format to be displayed on acknowledgement slip, default value - dd/MM/yyyy hh:mm a

`mosip.registration.application_date_format`

Date format to be displayed on Registration Client dashboard, default format - dd MMM hh:mm a

`mosip.registration.dashboard_date_format`

## Supporting properties for 1.1.5.x server compatibility

As 1.1.5.x versions did not have ui-specs, in android regclient, the backward compatibility is handled by migrating 1.1.5.x schema to LTS UI Spec structure.

To handle this migration, we have added some configurations and templates to support the 1.1.5.x server compatibility. Those configurations are listed below.

* mosip.registration.consent-screen-template-name=reg-consent-screen-content-template
  * Consent screen is not a part of 1.1.5.x schema structure. So, we are completely fetching this consent screen content from “master.template" table, and the templateTypeCode for the consent screen content is mentioned in the above configuration.
* mosip.registration.individual-biometrics-id=individualBiometrics
  * The id of individual biometrics should be mentioned in the above property according to the configured 1.1.5.x schema.
* mosip.registration.introducer-biometrics-id=guardianBiometrics
  * The id of guardian / introducer biometrics should be mentioned in the above property according to the configured 1.1.5.x schema.
* mosip.registration.infant-agegroup-name=INFANT
  * The age-group name for infants (aged below 5 years) which is configured in the configured server should be mentioned in the above property.
* mosip.registration.agegroup-config={"INFANT":{"bioAttributes":\["face"],"isGuardianAuthRequired":true},"ADULT":{"bioAttributes":\["leftEye","rightEye","rightIndex","rightLittle","rightRing","rightMiddle","leftIndex","leftLittle","leftRing","leftMiddle","leftThumb","rightThumb","face"],"isGuardianAuthRequired":false},"SENIOR\_CITIZEN":{"bioAttributes":\["leftEye","rightEye","rightIndex","rightLittle","rightRing","rightMiddle","leftIndex","leftLittle","leftRing","leftMiddle","leftThumb","rightThumb","face"],"isGuardianAuthRequired":false\}}
  * The above property indicates list of age-groups, required bio-attributes, and a flag which indicates guardian authentication is required or not. This property should be changed according to the server configuration and requirements.
* mosip.registration.allowed-bioattributes=leftEye,rightEye,rightIndex,rightLittle,rightRing,rightMiddle,leftIndex,leftLittle,leftRing,leftMiddle,leftThumb,rightThumb,face
  * The above property defines the list of bio-attributes that are allowed for scanning during registration. If there are any changes in the server, it should be changed accordingly.
* mosip.registration.default-app-type-code=000
  * The above property defines the default applicantTypeCode. In LTS, we have applicanttype.mvel script to fetch the documents according to the age, gender and some other attributes. Based on the applicant details, the script returns an applicantTypeCode which can be any value from “000” to “014”, and respective documents will be fetched from master.applicant\_valid\_document table. But we do not have this script defined in 1.1.5.x, to handle this, we have added a default applicantTypeCode.

**Templates:**

Ensure that preview and acknowledge templates are present in template table of mosip\_master database with the following type code

`reg-android-preview-template-part`

`reg-android-ack-template-part`

### Logout:

Logout from ARC will check for any running background tasks in the background. Ask the user if the user still wants to logout from the application.

* If the user clicks on logout on the popup, all the jobs running and scheduled jobs will stop.
* If no jobs are running in the background, the user will simply log out and navigate to the login screen.
* No configuration changes are required to log out of ARC.
