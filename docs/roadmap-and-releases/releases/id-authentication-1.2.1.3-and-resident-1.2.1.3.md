# ID Authentication 1.2.1.3 and Resident 1.2.1.3

**Release Version:** ID Authentication 1.2.1.3 and Resident 1.2.1.3

**Release Type:** Patch Release

**Release Date:** Coming Soon

### Overview

This patch delivers targeted fixes and improvements to the automation scripts used for Resident and ID Authentication testing in accordance with the upcoming release of infra 0.1.0. It enhances stability, resolves intermittent failures, and improves test accuracy to better support continuous integration test cycles.

### Major Highlights/Features

* **Improved Test Stability**\
  Refined the automation flow to prevent inconsistent behavior during Resident and ID Authentication test runs.
* **Enhanced Logging & Error Messaging**\
  Updated test logs and error reporting to facilitate faster debugging and clearer failure analysis.
* **Better Data Handling**\
  Strengthened handling of test input data and response assertions to simulate real-world authentication scenarios more accurately.

### Bug Fixes

<table><thead><tr><th width="220.26171875">JIRA</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-44154">MOSIP-44154</a></td><td>Rapid Deployment – API test rigs for Auth and Resident are failing due to a hardcoded domain name as <a href="http://mosip.net">mosip.net</a>.</td></tr><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-31156">MOSIP-31156</a></td><td>API: Due to 404-not found error and eSignet not deployed Resident testrig scenarios are failed and skipped.</td></tr></tbody></table>

### Known Issues

<table><thead><tr><th width="228.4375">JIRA</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://mosip.atlassian.net/browse/MOSIP-44276">MOSIP-44276</a></td><td>RES_UPDATE packet failing in Java 11 environment due to machine ID activation issue.</td></tr></tbody></table>

### Repositories Released

| Repository        | Tags Released |
| ----------------- | ------------- |
| id-authentication | v1.2.1.3      |
| resident          | v1.2.1.3      |
