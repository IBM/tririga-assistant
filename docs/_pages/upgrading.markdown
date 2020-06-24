---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

permalink: /upgrading/
layout: home
title: Upgrading IBM TRIRIGA Assistant
description: How to upgrade your IBM TRIRIGA Assistant. Detailed steps to use the latests features and upgrades.

sidebar:
  nav: "docs"
---

## Reminders
- Please be sure to **backup** your TRIRIGA instance prior to any changes
- Be aware that your Assistant will take time to provision and upgrade


### Part 1 - OM Package Import

1.	Create new Object Migration import package selecting the `tri-assistant-*.zip` file provided in the om-package folder.
2.	Validate and Import the new OM package.


### Part 2 - Submit the Provisioning Request

#### Step A) COMPLETE THE PROVISIONING FORM

The OM package imported in the part previous contains a form that will execute a process of gathering data for the Assistant and providing that data to IBM.  The form asks for the user name and password for the account created during installation.  This is needed so the IBM's TRIRIGA Assistant services can make an OSLC call back into your TRIRIGA instance and gather building and room names.

1. From the TRIRIGA Main Page, click on My Reports > System Reports, and filter on the Business Object column for `ibmAssistantProvisionOrder`.
2. Execute the `TRIRIGA Assistant Provision Record Query`.
3. Click on the only record found.  Don't add a record, just modify the existing record that was provided in the OM package.
2. Enter the URL for your TRIRIGA instance. Note: The instance must be accessible without a VPN and through HTTPS. Include the complete path, starting with `https://`.
3. Enter the user name and password for the assistant account you created earlier.
4. Enter your name and email address so we can contact you when the assistant services have been provisioned for your instance.
5. Click Submit.


#### Step B) WAIT FOR INTEGRATION ID FROM IBM

Once the information has been successfully received by the IBM TRIRIGA Assistant services, you will be contacted through email at the email address provided on the form.  Once you receive the Integration ID, you may proceed with the rest of the steps.  If you do not receive a response from IBM in a week, please contact your IBM representative.
