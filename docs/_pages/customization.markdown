---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

permalink: /customization/
layout: home
title: Customizing TRIRIGA Assistant
description: 

sidebar:
  nav: "docs"
---

Customization of the TRIRIGA Assistant is done by developing your own assistant using Watson Assistant.  Once you have an assistant that is functional, then you need to do the following:

### Integration process overview

1. Make minor modifications to the skill to make it "headless".  Since the TRIRIGA Assistant will be the interface to the end user, modifications to your skill need to be made so that your skill doesn't respond with a greeting and just handles request from the TRIRIGA Assistant.  More info below.

2. Stand up the tririga-assistant-proxy app which will proxy request from the TRIRIGA Assistant to your assistant.

3. Provide info about your skill and the proxy to the TRIRIGA Assistant team so they can add this info to the TRIRIGA Assistant.

4. Test and work through issues with the TRIRIGA Assistant team.

### How it works

Here's a quick overview of how requests to your assistant are routed through the TRIRIGA Assistant.

Each of your top-level choices, that you would normally provide to the user as options under the greeting, are added to the TRIRIGA Assistant.  In order for the user to access the functionality of your assistant, they must start by selecting an option.  The user cannot initiate your functionality using normal language since that would require all your intents, along with every example, to be duplicated in the TRIRIGA Assistant skill.  This, of course, would likely cause conflicts with the TRIRIGA Assistant's out-of-the-box functionality.  This may be a friction point for users at first but hopefully they will come to utilize the assistant by first selecting an option. 

Once the user has selecting an option, a string associated to that option, like create-lease, is sent to your assistant, through your tririga-assistant-proxy, and your assistant's response is passed to the user through the TRIRIGA Assistant.  If your assistant needs to access a service to fulfill the user's request, then that info must be returned via a client action so that the TRIRIGA Assistant can make that fulfillment requesnt on your behalf and provide the result back to your assistant to handle.  The diagram below describes this flow:

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/customization/HowItWorks.png)

[click here for larger view]({{ site.url }}{{ site.baseurl }}/assets/images/customization/HowItWorks.png)

### 1.a. Skill modifications to make it "headless"

Since the TRIRIGA Assistant will be responsable for the greeting message, this needs to be removed from your skill.  Then, in order to handle the strings that map to your top-level intents, all of your dialog nodes need to be placed as a child of a node with condition true.

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/customization/dialog-flow.png)

### 1.b. Skill modifications for telling TRIRIGA Assistant when in a conversation with user

In the "true node" that is the parent of all your dialog nodes, you should set a variable named `continueBPconvo` to `true`.  This variable is checked by the TRIRIGA Assistant to know if it should continue to forward all request to your skill.  Then for all nodes that handle the end of the dialog flow, you should set this variable to false.  Setting `continueBPconvo` to `false` will let the TRIRIGA Assistant know to show the starting options again to start the conversation over.

### 1.c. Skill modification for handling fulfillment

The TRIRIGA Assistant needs to be in-the-loop on all communication with your skill, so you can't do fulfillment via webhooks or cloud functions called directly from your skill.  All fulfillment must be done via the client using client actions.  You can find more information on [how to use client actions via the Watson Assistant documentation](https://cloud.ibm.com/docs/assistant?topic=assistant-dialog-actions-client). But, in order to make our code generic, we require a few additions to the required client action response.  In the `parameters` object of the client action, we require you provide the URL to your fufillment in a property named `url` and that the body to be sent to that URL to be in a property named `body`.  For example:

```
  "actions": [
    {
      "name": "meaningful-action-name",
      "type": "client",
      "parameters": {
        "url": "url-to-BP-fulfillment",
        "body": {
             "key": "value"
          }
        }
      },
      "result_variable": "meaningful-result-var-name"
    }
  ],
```

### 2. Setting up the tririga-assistant-proxy

In order to route requests to your assistant without having an API key for your Watson Assistant service, code for a proxy has been provided at [https://github.com/IBM/tririga-assistant-proxy](https://github.com/IBM/tririga-assistant-proxy).  Please follow the README in that repository for how to get the small proxy app running in your IBM Cloud environment.

### 3.a. Send information about your skill

Send the following through email to a TRIRIGA Assistant teammeber or your IBM representative.

1. List of codes you have added as examples for each of your intents.

2. For each code, provide a short phrase to be used as a button label.

### 3.b. Send information about your proxy

1. The string key that you used to replace the string `SOME_KEY_YOU_GIVE_TA_TEAM` in the ASSISTANT_INFO variable.

2. The URL to your running tririga-assistant proxy.


### 4. Testing and working out the kinks between the two assistants

Once the TRIRIGA Assistant team has added your intent codes and labels, and have done some testing to make sure communication is working, they will contact you to do more exhaustive testing together.
