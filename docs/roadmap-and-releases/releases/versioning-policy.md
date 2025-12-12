---
hidden: true
---

# Versioning Policy

### **Overview**

This section outlines the current approach and conventions applied to naming different types of MOSIP releases.

### Release Types

1. **Major Release/Production Release**:
   * **Example**: Version: 1.x , 2.x
   * **Purpose**: Major releases signify significant milestones in software development. They often introduce new features, enhancements, and may include architectural changes.
   * **Frequency**: Typically less frequent, often occurring annually or semi-annually.
2. **Minor Release/Interim Release**
   * **Example**: Version: 1.x.x, 2.x.x
   * **Purpose**: Minor releases focus on adding new features, functionalities, or improvements while maintaining compatibility with the existing system.
   * **Frequency**: More frequent than major releases, occurring every few months.
3.  **Patch Release**

    * **Example**: Version:1.x.x.x
    * **Purpose**: Patch releases address bugs, security vulnerabilities, and minor issues identified post-release.
    * **Frequency**: Regular and as-needed basis, often released urgently for critical fixes.


4. **Beta Release**:
   * **Example:** Version:
   * **Purpose**: Beta releases are more refined versions of software made available to a larger group of external users for testing and feedback.
   * **Characteristics**: Near-final features and functionalities with the aim of identifying and fixing remaining issues.

### Release Approaches followed in MOSIP

We follow the following approaches to manage different types of releases:

1. **Platform-wise Release** (full product release / major release) - When all repositories of the platform, including _mosip-infra_, are released together, it is considered a platform-wise release.
2. **Component-wise Release -** In this type of release, only a specific repository is updated and released. This may include major release if there is any feature enhancement or additions of new features included, and patch release if that includes bug fixes in the existing features.
3. **DSL Release -** As the name suggests, this release covers changes related specifically to DSL.
4. **Automation Release -** This release includes enhancements or updates exclusively to automation scripts.

### Naming Conventions

To ensure clarity and make it easier to distinguish between releases, we use the following naming conventions:

* **Platform-wise Release** → v1.2.x.x eg: 1.2.1.0
* **Component-wise Release** → \<module-name> 1.2.x.x eg: registration v1.2.1.2
* **DSL Release** → DSL 1.2.x.x
* **Automation Release** → Each respective module is released individually, hence follows the _component-wise convention_

### Additional Notes

* While platform release versions will continue to follow the 1.2.x.x convention, starting from platform release v1.2.1.0, each individual repository will adopt the new 1.3.x naming convention. Since MOSIP has designated the 1.2.x.x line as long-term support, we will maintain this versioning for platform releases, while component-level releases will follow the updated versioning scheme.
