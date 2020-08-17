---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

permalink: /changelog/
layout: home
title: IBM TRIRIGA Assistant - Changelog
description: New and exciting changes for IBM TRIRIGA Assistant can be found here. Follow our changelog for exciting version update info.

sidebar:
  nav: "docs"
---

## Version 1.0.5

1. New Feature - Teams bot code pattern: [https://github.com/IBM/tririga-assistant-teamsbot](https://github.com/IBM/tririga-assistant-teamsbot)

2. New Feature - Assistant now knows the floors in all buildings so can respond correctly when asked to reserve a room on floors that don't exist

3. Enhancement - Room reservation start times rounded to nearest 15 minute interval

4. Enhancement - Better name search for people and rooms by filtering on whole words

5. Fixes - 30+ fixes and improvements to better process information provided to the assistant during digressions, clarification and changes.

Conversing with TRIRIGA Assistant through Microsoft Teams:
![alt]({{ site.url }}{{ site.baseurl }}/assets/images/teams/TeamsScreenshot.png)


## Version 1.0.4

In this update we explore the possibilities of analytics with IBM TRIRIGA
Assistant. With Version 1.0.4 we are introducing a new dashboard for TRIRIGA
Administrators to help understand how the Assistant is being consumed by end users.

This new reporting dashboard helps visualize usage statistics like daily concurrent users, or satisfaction scores over the course of a month. We hope to expand this to even more exciting metrics in updates to come.

### Examples

Monthly View:
![alt]({{ site.url }}{{ site.baseurl }}/assets/post-images/2020-06-16/reports-1.png)

Daily View:
![alt]({{ site.url }}{{ site.baseurl }}/assets/post-images/2020-06-16/reports-2.png)
