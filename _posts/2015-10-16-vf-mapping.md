---
layout: post
title: "Mapping, A Power of Us Success!"
description: "Or, your first Visualforce Page..."
category: visualforce
tags: []
---
{% include JB/setup %}

#### Or your first Visualforce Page...
Ever since Summer '15, standard address fields in Salesforce have been accompanied with simple and useful Google Maps below them, showing where that address is in the world. This is a feature that users LOVE. Being able to quickly see in what neighborhood a Contact is, or where an organization you partner with on the map, it's pretty cool.

But... it's limited to standard address fields on Leads, Contacts, and Accounts. If you've got a custom Address field on a custom Object (for example, job location, or venue!) there's no map, and there's no way to add one.

[Joe Katzman](https://powerofus.force.com/_ui/core/userprofile/UserProfilePage?u=00580000009Jv2r) of the [Society of Manufacturing Engineers, Silicon Valley Chapter](http://connect.sme.org/smesiliconvalley/home) ran into this issue. He was conducting some [user interviews](http://www.usability.gov/how-to-and-tools/methods/individual-interviews.html) with his Salesforce users, and found out that the venues that his organization produces events in are something that they don't have a great process around. He writes:

> In the course of talking about how we handle events, it turned out that managing the event venues was a major pain: finding them, correlating with expected attendance, are they suitable (accessible? near a stadium or something that could be a problem? got wifi? etc.) As you can see, there are many variables. Yet events are the lifeblood of any industry-related nonprofit, so sorting through them all is a recurring problem.

In his organization, the event coordinators were managing these things "in their heads". Not even in spreadsheets. That's not sustainable!

> 1. Recurring pain point, which is a real inconvenience.
2. With serious consequences for some kinds of failure.
3. No other way of doing things as competition.
4. A win here gets some of our most active people used to the idea that Salesforce is worth entering data into, and then serves as your reference point. that's more than half the implementation battle right there, and with some of our less technical people.

> That's all 4 rows of the slot machine, right there. I would never have predicted this, but nowhere else came close as a place to start.

[So he came to the Hub, seeking a way to put a map on his custom venues object!](https://powerofus.force.com/0D58000002K28Iv)

And if you go read the thread, you'll see that he followed [some best practices for writing questions in a technical forum](https://www.biostars.org/p/75548/). He gave us context for what he was trying to accomplish, he described the problem carefully, and he showed us what he had tried so far. He asked for some additional advice on where to get started, or if there was some other way to solve the problem. The post was easy to read and courteous. The way he framed the question, I *wanted* to answer it! So I did.

### How can we put a map on the page?

The bad news? You can't accomplish this with point and click tools. It's pretty easy to [create a formula field that links to Google Maps](https://success.salesforce.com/answers?id=90630000000hcQbAAI) but embedding a map requires custom code.

The good news? It requires ZERO lines of Apex. This is a Visualforce-only problem, and even if you've never touched Visualforce in your life before, this one is pretty easy to figure out. You see, in Summer 15, when Salesforce introduced Google Maps components on standard objects, they also introduced the stupid simple [`apex:map`](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/pages_compref_map.htm) component. The docs themselves give a 90% working example! 

You're going to need to create a new Visualforce page. You can open up the Developer Console, or just use `Setup -> Develop -> Visualforce Pages` and hit `New`. Now you'll be faced with one of the hardest problems in Salesforce -- naming conventions. I'd recommend naming the page something intuitive like `VenueMapForPageLayout`. It may be a bit verbose, but it's clear what it does.

Here's the code for the page:

    <apex:page standardController="Venue__c">
        <apex:map width="350px" height="350px" mapType="roadmap" 
                  center="{!Venue__c.City__c},{!Venue__c.State__c}">
              <apex:mapMarker title="{! Venue__c.Name }" 
                              position="{!Venue__c.Street__c},{!Venue__c.City__c},{!Venue__c.State__c}"/>
        </apex:map>
    </apex:page>

7 lines, and pretty straightforward at that. You may need to make a few changes. If your custom object isn't API named `Venue__c`, you'll need to change every time `Venue__c` is in that code to the API name of your object (usually just `Object_Name__c`). Similarly, if your address fields aren't just `Street__c`, `City__c`, and `State__c` you'll need to update those references too.

Then, hit save! You shouldn't have any errors (but if you do, ask a question in the Hub!)

### But where do I see it?

Now it's time to fire up the page layout editor for your custom object. up in the field selector, along the left side, below fields, actions, etc... there should be an option for Visualforce pages. `VenueMapForPageLayout` should be one of the ones to choose from, and you can drag it right on to your layout!

![Page Layout Editor](http://i.imgur.com/6gPfFU0.png)

Once you do, when you hover over it, much like any other page layout item, a little wrench icon will appear. 

![Wrench](http://i.imgur.com/wvsLw5V.png)

Hit that, and set the component height to "350px". This is the same height we said the map would be in the Visualforce code up above, so it will ensure nothing gets cut off.

![Set height](http://i.imgur.com/xkT6Abv.png)

Now, save your page layout and bear witness to the fruit of your labors!

![A map](http://i.imgur.com/gH11b4n.png)

It even looks great in the Lightning Experience, right out of the box.

![LEX](http://i.imgur.com/Yn4E8X7.png)

### A Hub Success!

Back in the Hub, Joe tried out my solution. We worked on the code together, and helped make some changes to make sure it worked right for HIS organization. He actually helped catch the issue with some of the map getting cut off, which is why we know now to hit that wrench icon! The Hub is a community where we are always learning from one another. If I step in to answer a question, I look forward to someone commenting on a better way to solve it, or even just a different one.

Joe reported back:

>  This is going to be an awesome start to Salesforce for our organization - interviews showed that a working "Venues" object was actually the Minimum Viable Product, and would have the effect of hooking some of the less technical people. 

> Thanks to you, the final piece is in place, and I have something that will wow them.

And thanks to Joe, now more organizations can implement a simple feature that will wow their users!

*An important note is that Visualforce mapping is only available in Production and Sandbox orgs. It is not available in Developer Edition.*
