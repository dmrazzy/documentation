## Prompts


Android Registration Client GA Release v1.0.0
- Here is the release notes for Android Registration Client GA Release v1.0.0
- This is typical one and minimum one shared by the Product Owner
- However I want to improve this one as this is GA -General Availability and Major release
- Here is the PR raised - https://github.com/mosip/documentation/pull/1014 , learn from this PR as well - as it contains route for you to learn what all changed
- Also on this page only the JIRA links for Features Delivered, Bug Fixes and Improvements and Known issues are also provided, The provided JIRA tickets are open JIRA tickets and publically visioble, Go through each and learn what all changed
- Also refer to the the Documentation for Android Registration Client which happened over last 2 weeks and PR I already shared with you which is https://github.com/mosip/documentation/pull/1014
- Now help me improve following, Also provide links to the documentation that happened recently finding it from the PR I shared with you (https://github.com/mosip/documentation/pull/1014) which should be very specific to the features:
1.  **GPS Tracking:** Tracks the location where a registration packet is
    created and measures its distance from the device's mapped
    registration center.

2.  **Applicant Biometric Correction:** Allows issuing a temporary ID
    when biometric capture fails, enabling the applicant to return for
    recapture before an AID is generated.

3.  **Settings:** Provides access to device details, scheduled job
    configurations, and global/local configuration settings within the
    ARC.

4.  **Support for Landscape Mode:** Allows the Android Registration
    Client to function seamlessly in landscape orientation.

5.  **Support for Phone Screens:** Optimizes the Android Registration
    Client for effective use on smaller mobile screens.

6.  **Auto Logout:** Automatically logs out the user after a
    configurable period of inactivity for security.




# Registration Client UI Spec
- Read through the Registration Client UI Spec attached here
- Also read through Spec from https://docs.mosip.io/1.1.5/modules/registration-client/ui-specification-for-registration-client - this is more human readable format and contains details in tabular format as well as description on usage as to how to use the UI components
- Create a prompt for generating Registration Client UI guide based on the above two documents





# PMS Features
- I want to create Features page based on The _feature-page.md template and the _features-page-guidelines.md attache here
- Go through the existing PMS features page attached here
- Also go through the _features-revised.md attached here for PMS 
- Also go through the PMS End User Guide and the individual guide for partner admin, ftm chip provider etc attached here
- Follow the features template and features guidelines attached and extyract informtion in separate markdown file as per the template






# MISP guide Improvements
- Observe the guides in PMS
- It always use second person subject, that is You
- Also, the I had introduced Interface Overviews to acquaint users of the interface such that the initial first steps User should not have to mention how to navigate and user breadcrumb like arrow based navigation to reach the section on inteface
- Do not remove any factual Information
- Improve the steps as per screenshots as well as second person you as well as minimal initial navigation steps 






## MOSIP Infra Release Documentation Tasks
- Read docs/roadmap-and-releases/releases/0.1.0-beta-mosip-rapid-deployment-infrastructure.md
- docs/roadmap-and-releases/releases/1.2.0.4.md
- Read the readmes from https://github.com/mosip/infra/blob/release-0.1.0/README.md#github-actions-workflow-parameters-reference
- Read the overview page guidelines from unsegregated/_templates/_overview-page-guideline.md also from unsegregated/_templates/_overview-page-template.md
- Create an overview page for MOSIP Infra module following the guidelines mentioned in the above files and add the content in a markdown file





## Registration client features

* MOSIP is foundational ID platform
* Registration Client is a MOSIP Module to register an applicant
* You have to extract features content only from
  * 'Registration Client' Overview page attached here -
  * 'Registration Client' End User Guide attached here, i.e.
  * 'Registration Client' Release notes

* 'Registration Client' Overview page attached here has features put up in way you can call it a User's or Operatorer positive flow, we have to maintain this approach of putting up features in a positive flow, as it is more user centric and easy to understand

* Do not makeup any feature which is not mentioned in the documentation and the files attached here
Read the guidelines we have put in for features-writing-guidelines.md to write 'features' page

* For Information Architecture and other guidelines refer to _features-writing-guidelines.md



## Documentation Gap in Rapid CI/CD Releases

Many software organizations face the challenge of keeping documentation up-to-date with rapid CI/CD releases. Often, new features are only briefly mentioned in release notes, especially if they are technical, database, or code improvements that do not immediately impact the UI or user guides.

How do leading organizations address this documentation gap? Where should technical features and improvements be documented for clarity and consistency?

Suggest best practices and recommended locations within documentation to capture and detail such updates, drawing from examples in top software product documentation.

## Landing page of Pre-Registration Module

MOSIP is foundational ID platform
Pre-registration or Pre Registration is a MOSIP Module
This is the overview or landing page for Pre-Registration
What should be the ideal structure of a landing page for a Module like this
Suggest from your research and understanding of top documentation site of some of the best software products
Add the proposed structure at the bottom of Overview page attached here
Do not remove any content above


## features page of Pre-Registration Module - advantages of the current features page
This features page has gone under a comprehensive iterations of refinement to make it worth it
We have also taken help from copilot which has kept it in line with similar to standard software features page at par with other such top software product's feature's page
The whole MOSIP documentation has such pages for several other modules but not written in a standard and consistent way
Can you read through the docs and also how other software products have written their features page In their docs and Identify the merits this page now brings





