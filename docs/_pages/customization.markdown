---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

permalink: /customization/
layout: home
title: Customization TRIRIGA Assistant
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

[click here for larger view]({{ site.url }}{{ site.baseurl }}/assets/images/customization/HowItWorks.png)

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/customization/HowItWorks.png)

### Skill modifications

Work-in-progress.
