---
layout: post
title: "Visual Workflow and the Finish Button"
description: ""
category: 
tags: [visual-workflow]
---
{% include JB/setup %}

Not everything can be launched or solved by the Process Builder (as much as I wish it could!) Sometimes, you just really do need to have a user intervene. Today, I built a little Flow that helps my users enter demographic information. On every ticket order we get, we request our patrons to tell us their birthdate, their ethnicity, and their gender identity. We provide open text fields for them to describe themselves however they want.

We then go through each ticket order and copy their information over to their Contact, but also categorize their demographic data into a picklist that we can report on. That way, we get the best of both worlds. We can communicate directly with that patron and use the terminology they want to describe themselves with, but we can also tell grantmakers what percentage of latino/a attendees we have.

This used to be a manual process, but Terry Cole on the PowerOfUs HUB gave me the idea that Flows are great for processing demographic data. With a little bit of work, here we are!

<img src="http://i.imgur.com/yDEDF1x.jpg" width=100%></img>

This little wizard steps through any unprocessed records and let's a member of the team categorize their data.

When we're all done, we hit the end of the flow. It has a finish button. Pressing that button...doesn't take me anywhere useful.

![finish button](http://i.imgur.com/ZdWuFyP.png)

As it turns out, there are some downsides to launching a Flow from a custom button. Most specifically, it's really hard to control where that flow goes when you finish it! In this case I want to go to the home page, nice and simple.

It also turns out, there's a simple fix, and that fix is...*VisualForce*. But never fear, the VisualForce page you are going to need is only three lines long:
```
  <apex:page >
    <flow:interview name="Process_Ticket_Order_Demographics" finishLocation="{!URLFOR('/home/home.jsp')}"/>
  </apex:page>
```

Just put the unique name of your flow under "name" and name this page something memorable. Adjust your custom button to hit up this new VF page, and *BLAMMO*, when your user finishes their flow, they actually go where you want them to.
