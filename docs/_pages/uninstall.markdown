---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

permalink: /uninstall/
layout: home
title: Uninstalling the IBM TRIRIGA Assistant
description: How to remove the assistant and it's services.

sidebar:
  nav: "docs"
---


### Step 1 - Remove Chat UI from Workplace Service app

Remove the `ibm-TriAssistant` HTML element by editing the files modified when following the installation instructions in steps F and G.


### Step 2 - Remove access to the models

Using the Security Group Manager, edit the security groups modified (or delete if new groups were created) when following the installation instructions in steps J and K to remove access to the `ibmTriAssitant` and `ibmTriAssistantPortal` models. 


### Step 3 - Delete the models, model and views, views and applications

Using the Application Designer, Model Designer, Web View Designer and Model And View Designer tools, delete everything with name `ibmTriAssistant` and `ibmTriAssistantPortal`.


### Step 4 - Deleting IBM TRIRIGA Assistant services

There currently is no automated way to delete the data on IBM TRIRIGA Assistant Services, please contact your IBM representative that assisted you with the provisioning process or submit a support ticket.