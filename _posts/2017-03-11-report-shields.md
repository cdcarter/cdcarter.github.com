---
layout: post
category: appbuilder
title: 'Report Shields - Action Has Context'
tags:
  - admin
  - frosting
  - formula
published: true
---

<meta name="twitter:card" content="summary" />
<meta name="twitter:site" content="@cdcarter" />
<meta name="twitter:title" content="Report Shields - Action Has Context" />
<meta name="twitter:description" content="Look before you lead with report shields" />
<meta name="twitter:image" content="http://i.imgur.com/3gFm2LW.gif" />

In the tradition of awesome formula image tricks, I wanted to introduce [shields.io](http://shields.io/), an awesome service you maybe have seen used on Github repositories.

It might express a version:
<img src="https://img.shields.io/homebrew/v/cake.svg"/>

or the build status: 
<img src="https://img.shields.io/shippable/5444c5ecb904a4b21567b0ff.svg"/>

Shields are useful, because you just drop an image link in in your README and they render. Wait a second, can't we drop an image link in a formula field and see something cool? We've done that before with [Da Button Factory](https://dabuttonfactory.com), let's try it with a shield.

The URL pattern for a shield starts simple:

      https://img.shields.io/badge/<SUBJECT>-<STATUS>-<COLOR>.svg

and after reading about the [url parametes](http://shields.io/#styles) that Shields.io supports I came up with a version that looks at home and on brand in the Lightning Experience.

      https://img.shields.io/badge/NAME-STATUS-NULL.svg?style=flat-suare&colorB=0070d2

At first, I thought let's try a shield as the record header! By making a field the first field in the compact layout, it gets made big and into a header.

At Bigger Boat, I have to track my time in a time sheet, where I say how many hours I spent on each project and what I did. Let's take a look at making the Time Log object (which used to have an ugly auto-number) into something cuter.

<img src="http://i.imgur.com/3FfJP9r.png"/>

It looks nice. Hell, Matt Bertuzzi, author of [Lightning Sales Ops](https://www.amazon.com/dp/B06XMR93HC) wrote:

> What the hell?! Love it

and it's easy to implement:

    HYPERLINK(
        "#/sObject/"&Id&"/view",
        IMAGE(
            "https://img.shields.io/badge/" & 
              SUBSTITUTE(Account__r.Name," ","_") & 
              "-" & 
              TEXT(Duration__c) & 
              "hrs-ignored.svg?style=flat-square&colorB=0070d2", 
            Account__r.Name & " - " & TEXT(Duration__c) & "hrs"),
        "_self")
        
That said, I'm just not sold on this as the best place to use shields in my Salesforce app. I know, I know, I have a solution in search of a problem.

I hopped on a screenshare with [Jason Lett](https://twitter.com/jasonblett), a Product Manager at CrowdCompass, and demo'd what I'd built. 

We talked for a while, and he made sure to ask me important questions about the context in which these shields would be seen. He had me open up a few more views and it became pretty apparent that the first section of the shield really shouldn't vary too much in length if you'll see it in a list view.

<img src="http://i.imgur.com/bCxNsQN.png" width="70%"/>

It's just messy and distracting. We talked about different ways that a shield could still be a timelog record header. What if the first section show's the date? That's always the same length! Jason reminded me that the cool thing about the shield in an open source README file is that it shows a summary of the data it links through to. LIGHTBULB.

But as we were looking at [themes throughout the app](http://www.designkit.org/methods/5), there were a lot of user stories where we wanted to link out to a filtered report. This is a long time Salesforce "best of the rest" feature, that in Spring 17 is officially supported. [Beth Breisnes](https://twitter.com/bethbrains) wrote up a fantastic post about how to build a report link in Lighting [on the Bigger Boat Blog](http://biggerboatconsulting.com/supporting-report-filter-url-hacks-in-lightning-and-classic/). Her post includes the deep details on how to make a formula link work in both Classic and Lighting, so check it for details if you need to support both environments.

I love this technique, but is "This Account's Opportunities" really the best way to show what you're linking off to? 

## Introducing Report Shields
<img src="http://i.imgur.com/xydtt23.png"/>

Instead of just linking to a report, how about we use a button in order to make it clearer that this is something you can click on and get more details? In fact, what if our app established that a shield means you're going to a place where you see more detail?

A shield for "This Account's Opportunities" (or in my timesheet app, "Account Hours Breakdown") would be a little bit more visible than a link, right? 

Wait, but a shield has a second component. Not just a label, but...something on the right. A value. 

We all know that [Lightning Components are data aware](http://www.tahoedreamin.com/sessions/take-action-lightning/). Well, here's a declarative field you can build that's data aware too. Let's build a shield that links to the hours report, but shows the total too.

I built a simple rollup on Account for how many hours had been billed. The criteria for this rollup must EXACTLY match the criteria for your report's grand total, but that's okay.

I then built a detail report that would show the hours breakdown for that account with a grand total:
<img src="http://i.imgur.com/8mXaBdn.png" width="70%"/>

Notice how we add a filter but leave the value blank, as described in Beth's post! Then, we create our Report Shield formula field:

    HYPERLINK(
        "#/sObject/<YOUR REPORT ID>/view?fv0=" & Id,
        IMAGE(
          "https://img.shields.io/badge/Hours_Breakdown-" &
            TEXT(Hours_billed__c) &
            "_hrs-ignored.svg?style=flat-square&colorB=0070d2",
          "link to account report"),"_self")
          
Boom, we now have a report button that also can tell us a little bit more about what we're gonna see when we click through.
<img src="http://i.imgur.com/1vkNsJw.png"/>

<img src="http://i.imgur.com/hR3l7kY.gif" width="70%"/>

Damn, that's smooth. And now my users know that any shield with a summary will take them to a report or list view with a lot more detail. We'll be adding a lot more of these, I think.
