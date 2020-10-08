---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

permalink: /installation/
layout: home
title: Installing TRIRIGA Assistant
description: Steps for requesting, provisioning and installing IBM TRIRIGA Assistant for your IBM TRIRIGA solution.

sidebar:
  nav: "docs"
---

## [GET THE OM PACKAGE!]({{ site.github.repository_url }}/tree/master/om-package)

## Estimated time
This installation should take 1-2 hours to complete, not including the time for IBM to provision the TRIRIGA Assistant skill.

## Steps
### Part 1 - Import OSLC resources, create assistant user and test
#### A) IMPORT THE OM PACKAGE
1.  Create new Object Migration import package selecting the `tri-assistant-*.zip` file provided in the om-package folder.
2.  Validate and Import the OM package.

#### B) SET UP THE ASSISTANT USER.
In order to allow the assistant user to create location reservations and service requests on behalf of other users, we have to create the user with the proper groups and licenses.
1.  Create a user with user with the following:
    Groups:
    - TRIRIGA Request Central
    - TRIRIGA Request Central - Fundamentals
    - TRIRIGA Request Central - Reserve
    - TRIRIGA Request Central - Reserve - Fundamentals
    - WAS Reserve OSLC

    License:
    - IBM Facilities and Real Estate Management on Cloud Self Service, or
    - IBM TRIRIGA Request Central, or
    - IBM TRIRIGA Workplace Reservation Manager
2.  Make note of the user name and password because it will needed in the provisioning step below.

#### C) TEST THE OSLC ENDPOINTS.
Execute a quick `curl` command to validate OSLC endpoints used during provisioning.  Follow these steps to make sure you have the necessary info and objects in place.
1.  You'll need the base64 encoded representation of the usernmame and password for the assistant account created earlier.  The `base64` unix command will generate this string for you given the username and password.
   `echo -n "username:password" | base64`
2.  Execute the following `curl` command and replace TRIRIGA_URL and ENCODED_USERNAME_AND_PASSWORD with your info.
    `curl --location --request GET 'TRIRIGA_URL/oslc/spq/ibmWAProvisionQC?oslc.select=*' --header 'Authorization: Basic ENCODED_USERNAME_AND_PASSWORD' --header 'User-Agent: PostmanRuntime/7.23.0' --insecure`
The response should have some `triParentBuildingTX` values returned.  If you get an OSLC error, check that at least one of the groups added to the assistant account has an organization set.  If you can't solve the problem on your own, contact your IBM representative.  Getting results from the curl command is required for the provisioning form to work in the next part of these instructions.
Also, optionally, if you understand Postman and would like to test all the OSLC calls, then you can test by using [the Postman collection]({{ site.github.repository_url }}/tree/master/postman) provided in the postman directory.  You will need to provide values for the variables building, space, etc. used in those payloads to match what is in your TRIRIGA instance. A successful test of the OSLC APIs when there are no OSLC errors.

### Part 2 - Submit the Provisioning Request
#### D) COMPLETE THE PROVISIONING FORM
The OM package imported contains a form that will execute a process of gathering data for the Assistant and providing that data to IBM.  The form asks for the user name and password for the account created in an earlier step.  This is needed so the IBM's TRIRIGA Assistant services can make an OSLC call back into your TRIRIGA instance and gather building and room names.
1. From the TRIRIGA Main Page, click on My Reports > System Reports, and filter on the Business Object column for `ibmAssistantProvisionOrder`.
2. Execute the `TRIRIGA Assistant Provision Record Query`.
3. Click on the only record found.  Don't add a record, just modify the existing record that was provided in the OM package.
2. Enter the URL for your TRIRIGA instance. Note: The instance must be accessible without a VPN and through HTTPS. Include the complete path, starting with `https://`.
3. Enter the user name and password for the assistant account you created earlier.
4. Enter your name and email address so we can contact you when the assistant services have been provisioned for your instance.
5. Click Submit.

#### E) WAIT FOR INTEGRATION ID FROM IBM
Once the information has been successfully received by the IBM TRIRIGA Assistant services, you will be contacted through email at the email address provided on the form.  Once you receive the Integration ID, you may proceed with the rest of the steps.  If you do not receive a response from IBM in a week, please contact your IBM representative.

### Part 3 - Add Assistant UI to Workplace Services apps
#### F) EDIT THE WORKPLACE SERVICES APP TO ADD CHAT UI ACCESS.
1.  Once you have received your Integration ID, open the Workplace Service view by going to Tools > Web View Designer > triWorkplaceServices.
2.  Copy the value in `Development Filename` and paste it as the value in the `Production Filename`. (Note: This is done for testing purposes and can be reversed after testing passes. A link to instructions on how to do "vulcanization" is below.)
3.  In the View Files section, click on `/trilazy-imports.html`.
4.  Click on the Download View File icon.
5.  Edit the `trilazy-importshtml` file and add the following below the last `<link>` tag line:
    ```html
    <link rel="import" href="../ibmTriAssistant/ibmTriAssistant.html">
    ```
6.  Upload the changes by clicking on the Upload view file icon.
7.  Click `Save & Close` button in upper right corner.
8.  In the View Files section, click on `/triview-workplace-services-dev.html`.
9.  Click on the Download View File icon.
10. Edit the `triview-workplace-services-devhtml` file and at the bottom of the template section, paste the following lines of code above the `</template>` so the following HTML is between the `<template>` and `</template>` lines:
**Make sure to replace `PLACE_INTEGRATION_ID_HERE` with the Integration ID provided.**

    ```html
    <ibm-TriAssistant integration-id="PLACE_INTEGRATION_ID_HERE" region="us-south" model-and-view="ibmTriAssistant" instance-id="-1" online="[[online]]"> </ibm-TriAssistant>
    ```
11. Upload the changes by clicking on the Upload view file icon.
12. Click `Save & Close` button in upper right corner.

#### G) (OPTIONAL) EDIT THE ROOM RESERVATION AND SERVICE REQUEST VIEWS.
From the "Web View Designer", repeat the same steps directly above for the other views:
    - triRoomReservation View (set the `Production Filename`, edit files `trilazy-imports.html` and `triview-room-reservation-dev.html`).
    - triServiceRequest View (set the `Production Filename`, edit files `trilazy-imports.html` and `triview-service-request-dev.html`).

#### H) (OPTIONAL) VULCANIZE THE VIEWS.
If you feel that your workplace service apps are loading much slower after the edits, then you can "vulcanize" the apps [following these instructions](https://www.ibm.com/search?lang=en&cc=us&q=How+to+vulcanize+your+UX+application).  If you do this, make sure you undo the change like F2 that sets the `Production Filename` to the `Development Filename`.

#### I) ADD INTEGRATION ID to Assistant Portal UX app
A UX app, call the Assistant Portal, is provided in the OM package that will allow you to view some metrics on the usage of the Assistant.  Follow these steps to get the Portal working.
1.  Open the Portal view by going to Tools > Web View Designer > ibmTriAssistantPortal.
2.  In the View Files section, click on /ibm-tri-assistant-portal.html
3.  Click on the Download View File icon.
4.  Edit the `ibm-tri-assistant-portalhtml` file and on the first line of code, replace `PLACE_INTEGRATION_ID_HERE` with the Integration ID provided.
5.  Upload the changes by clicking on the Upload view file icon.
6.  Click `Save & Close` button in upper right corner.
A Navigation Item, named ibmTriAssistantPortal, has been provided in the OM package that you can then add to any menu you want, but the portal can be accessed using the /p/web/triAssistantPortal URL.

### Part 4 - Configure permissions for users

#### J) ADD ACCESS TO ibmTriAssistant MODEL
The OM package imported contains a new model for the UX apps.  Non-admin users need to be given proper access to this model.  To accomplish this, you can either create a new security group or modify an existing.  The steps below modify the `TRIRIGA Request Central - Fundamentals` security group to allow users, that have this group, to read, update, create and delete the `ibmTriAssistant` model.
1.  From the TRIRIGA Main UI, go to `Tools > Adminstration > Security Manager`.
2.  Click on the `TRIRIGA Request Central - Fundamentals` security group.
3.  In the window appears, click on the `Access` tab.
4.  Scroll down and expand the `Models` root and select `ibmTriAssistant`.
5.  In the "Model Access" panel on the right, select `Read,Update,Create and Delete`.
6.  Click Save & Close.


#### K) ADD ACCESS TO ibmTriAssistantPortal MODEL
For admin users that need to see the usage statistics of the assistant, access to the ibmTriAssistantPortal model needs to be added to a security group for administrators (or create a new group for this).  Follow the same steps as above to add the `ibmTriAssistantPortal` model.


#### L) TEST THE WORKPLACE SERVICES APPS.
It's time to test with the Assistant Chat UI available from the Workplace Services app.  Make sure the user you are using has a primary location set, isn't the system or assistant user, and has the security group modified or created in step K.  If all edits were done correctly, you should see a chat icon appear at the bottom right of the Workplace Services apps.  If you know a room name, then try out the service request functionality by typing "the ________ room has a broken chair" and if you have reserve functionality, try "book a room".

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/installation/screenshot.png)
