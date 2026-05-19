---
icon: file-certificate
---

# License

## Introduction

MOSIP is built on a strong foundation of open-source principles: transparency, modularity, reusability, and global collaboration. To maintain consistency and compliance across all repositories, MOSIP adopts licensing practices that allow safe integration of external dependencies while staying true to its core licensing model.

MOSIP’s primary licenses across its repositories are the Mozilla Public License 2.0 (MPL 2.0) and the MIT License. These licenses support modularity, file‑level copyleft (in the case of MPL), and broad reuse (in the case of MIT). Their flexibility allows MOSIP to integrate permissive and certain weak‑copyleft components while maintaining a clear and consistent compliance framework.

## Licensing in MOSIP Repositories

> All MOSIP code is owned and maintained by International Institute of Information Technology, Bangalore, on behalf of MOSIP.

MOSIP follows a structured licensing approach across repositories.

1. MPL 2.0

* The MPL 2.0 is the primary license used across all core MOSIP repositories. It is a file‑level weak copyleft license, meaning:
* Only the files that contain MPL‑licensed code must remain under MPL.
* Other files within the same repository or project may use different licenses, making MPL well‑suited for large, extensible systems like MOSIP.

**Why MOSIP uses MPL 2.0**:

* Supports modular architecture by allowing individual components to evolve independently.
* Guarantees openness of improvements since modifications to MPL‑licensed files must be published.
* Compatible with many permissive licenses such as MIT, BSD, Apache 2.0.
* Enables proprietary or differently‑licensed modules to coexist in the same repository, as long as MPL obligations are respected.
* Aligns with digital public goods (DPG) principles by encouraging reuse while maintaining some safeguards.

**Key obligations under MPL 2.0**:

* Modified MPL‑licensed files must be released under MPL 2.0.
* The license text must be included with the distributed software.
* Clear marking of any modified MPL‑licensed files.
* Attribution and source code availability only for modified MPL files, not the entire project.

2. MIT License (Permissive License)

* The MIT License is a lightweight permissive license used in MOSIP for modules intended for broad reuse and easy integration such as sample applications, helper utilities, SDKs, and educational/reference implementations.

**Why MOSIP uses MIT**:

* Minimal restrictions, enabling reuse across governments, partners, and vendors.
* Extremely easy for external contributors to adopt.
* Ideal for distributable components and community-driven tools.

**Key Obligations:**

* Retain copyright and license notice.

**Best suited for:**

SDKs, sample apps, developer tools, UI components, reference implementations. Used in helper or reference implementations, sample apps, or where contributions from external developers are larger.

3. Creative Commons Attribution-ShareAlike 4.0 International License

* The Creative Commons Attribution-ShareAlike 4.0 International License is used only for MOSIP non-code assets such as documentation, training content, diagrams, and sample datasets.
* Why MOSIP uses CC-BY SA 4.0 tribution or legal barriers. - Perfect for documentation and educational materials.

**Obligations:**

* Attribute our work, and Distribute your adapted work under the same license, without placing additional restrictions.

**Best suited for:**

* Docs, training guides, communication assets, sample datasets.
* Used for documentation, design assets, or datasets intended for unrestricted reuse.

**Each repository includes**:

* LICENSE.txt
* /third-party-licenses/ folder for dependency licenses

## Dependency Integration Process in MOSIP

All MOSIP developers must follow the below steps when adding any third‑party dependency.

### Step 1: Evaluate License Compatibility

Use the Compatibility Matrix detailed here: Open Source Licenses and their Compatibility. If incompatible, do not use the dependency.

### Step 2: Include the Dependency’s License File

* Add the third‑party license text into:
* /licenses/ folder, or
* Next to the main LICENSE file.

### Step 3: Attribution Requirements

Update or create a NOTICE file if the license (e.g., Apache 2.0, ISC, Unicode) requires attribution.

\*\***Sample NOTICE entry**:

This project includes third‑party software:

* Library Name — Licensed under Apache License 2.0

### Step 4: Preserve Your Project’s License (MPL 2.0)

Even with third‑party components, MOSIP projects remain under MPL 2.0.

**Add clarity in LICENSE file**:

This project is licensed under MPL 2.0. It includes components licensed under other open-source licenses. See /licenses and NOTICE for details.

## SBOM (Software Bill of Materials) and Compliance Review

MOSIP generates an SBOM for each release using tools such as CycloneDX.

**What SBOM Ensures**:

* Identification of every dependency
* Detection of incompatible licenses
* Compliance verification before release Compliance Checks Include:
* Conflicting licenses
* Missing license files
* Missing NOTICE requirements

## Contribution Compliance in MOSIP

MOSIP enforces several governance mechanisms across all GitHub repositories.

1. Developer Certificate of Origin (DCO)

* Required for every contributor
* Enforced automatically via GitHub checks
* Every commit must be signed-off using: Signed-off-by:

2. Corporate Contributor License Agreement (CCLA)

* Used when MOSIP moved ARC license from MPL → MIT
* Email consent recorded for contributing organizations

3. External Contributions Log

**Maintained at**:

* Google Sheet (external contributors)
* MOSIP Docs → Contributions page

These logs track contributions from both individuals and rganizations.

## Best Practices for MOSIP Developers

* Prefer permissive licenses when choosing third‑party libraries.
* Always review compatibility before integration.
* Maintain up‑to‑date third‑party license folders.
* Ensure SBOM is regenerated after adding dependencies.
* Follow MOSIP’s repository structure and licensing templates.
