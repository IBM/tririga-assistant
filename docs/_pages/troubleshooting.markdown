---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

permalink: /troubleshooting/
layout: home
title: Troubleshooting
description: Get help diagnosing issues and details on how to fix more frequently encountered issues with IBM TRIRIGA Assistant.

sidebar:
  nav: "docs"
---

## Common Problems


1.  If a popup appears saying "You do not have permission to access this page" when you access the Workplace Services apps, then the user doesn't have a security group that has the permissions documented in step J.
2.  If the chat icon appears but you don't see the introduction similar to what is shown in the image at the very top of this doc, then check that the user you are using has a primary location set in the user's profile.
3.  If the chat icon doesn't appear in the bottom right corner of the Workplace Services app, then check for errors using the Console tab of the Inspector (right click in webpage and choose `Inspect`).
4.  If you find that you reservations made through the assistant do not appear in the Workplace Services home page (/p/web/workplaceServices), then check that the security groups assigned to that user have an Organization set.  The security groups mentioned in Step D don't have an Organization set by default.  There might also be issues with the Organization set on the assistant account as well as Organization set on the user and the buildings.
5.  If you are unable to view the TRIRIGA Assistant Portal UI (found at /p/web/ibmTriAssistantPortal), then check that access to the `ibmTriAssistantPortal` model is added to security group on the user's profile.  See step K for instructions.
6.  If you see the error code `om-release-mismatch` when provisioning 1.0.4 or later, then make sure you cleared the ClassLoader cache as instructed on the [Upgrading page]({{ site.url }}{{ site.baseurl }}/upgrading) and try the provision again.  If you cleared that cache and continue to get the error, delete the provisioning record and add a new one.  If that doesn't solve the problem, please submit a support ticket or contact your IBM representative.
7.  If the assistant replies with "I'm sorry, I can't find the timezone for your building.", check the timezone set for the building mentioned or set as primary location. If a timezone isn't set then set one and submit the provisioning form.  If a valid timezone is set, then please submit a support ticket or contact your IBM representative.
8.  If the assistant replies with "Sorry, your user information doesn't seem to be formatted correctly, so I can't complete the request. Contact your system admin for more help.", check that the user has an Organization and a Primary location.
9.  If, during provisioning, you see the error `oslc-response-parse-error`, the OM package import did not set the properties correctly on one or more OSLC resource shapes.  To fix this problem, go to `Tools > System Setup > OSLC Resource Manager`, click on the `ibmWABuildingsTimezonesRS`, click on `Import all Fields`, located at the top right the Properties table, then click `Save & Close`.  Then try the provision again.  If you continue to see the `oslc-response-parse-error`, then one or more of the other OSLC Resource Shapes have this problem as well.  Please check the properties on the following resource shapes that are used during provisioning: `ibmWAFloorsRS` and `ibmSpaceRS`.
