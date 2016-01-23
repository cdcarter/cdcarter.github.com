---
layout: post
title: "Exception Reporting: The Accidental Admin's Best Friend"
description: "Clean up that data"
category: admin
tags: [reporting]
---
{% include JB/setup %}

This week, I've been working in an org that was first built out almost 8 years ago. They're a tiny staff, and don't have a dedicated admin. Now, [having an admin is absolutely crucial to long term success with Salesforce](http://biggerboatconsulting.com/why-you-need-a-salesforce-administrator/), but sometimes that just doesn't happen. And if you don't have an admin, or you've found yourself an accidental admin, **that's ok**. 

There are a lot of tricks in the admin toolbag, list views, login flows, duplicate management, and of course Process Builder and Visual Flow. But if you don't have time to learn all that, there's one thing I want to make sure you do know.

## Exception Reporting

Exception Reporting isn't really a Salesforce feature, it's just a technique, it's a way of thinking. Exception reports are designed to do one thing: help you trust your data. By using standard Salesforce reporting features and an analytical brain, you'll be able to crank out great exception reports that keep your business flowing.

> Exception reports are designed to do one thing: help you trust your data.

So what do I mean by "exception"? You may have used an unstable package and seen Apex exceptions in your Salesforce org, but today we're talking about data that is exceptional. Data that is exceptionally bad. Exception reports help us sniff out bad data, clean it up, and move on with our lives.

## There are many types of exceptions

There are many different types of exceptions that could happen in your org, which means there are many different types of exception reports you might build. Let's focus on the two types of exceptions that are most important to find (and the types I was working on with my client this week):

 - Missing Data
 - Contradictory Data
 
 If you need to brush up on your reporting basics, make sure to work through the [Reports and Dashboards Trailhead Module](https://developer.salesforce.com/trailhead/module/reports_dashboards), and then let's get started!

### Missing Data

Missing data is the scourge of anybody who needs to pull a quarterly report. What do you mean, we didn't enter demographic information for any clients in October?! Your org probably has fields that are "de facto required". Nobody has made them required in Salesforce, there's no validation rule to make sure they're filled out, but you need the data...

It's definitely a good idea to go make that "Veteran Status" picklist required, *right now*, but you still need to go back and fix old records. This is as simple as making a report of all Contacts & Accounts with a filter for records where Veteran Status is blank!

<img src="http://i.imgur.com/3mvdJZr.png" width="70%"/>

You'll also find a reason to make use of [Cross Filters](https://help.salesforce.com/apex/HTViewHelpDoc?id=reports_cross_filters.htm&language=en) for missing data. In this org, we have a "Client Status" picklist on Contact, and a Program Enrollment object to capture details about the services they receive. But sometimes, when our case workers are enrolling clients, they fill in all the contact info, set the client status, but forget to create that enrollment object. We can use a Contacts & Accounts report with a cross filter to find all contacts that are active clients, but don't have an active program enrollment.

<img src="http://i.imgur.com/ayMAGZz.png" width="70%"/>

### Contradictory Data

More devious than missing data is contradictory data. As databases evolve over time, you're going to find better ways to track the information you need. And your funders are going to change up the data they want to hear about, which is always a reporting challenge. 

> Contradictory data is devious, skews your numbers, and is incredibly easy to identify.

For example, recently, one major funder went from asking what % of clients were Vietnam Vets to asking what % of clients were any type of Veteran. Because of this, this org went from a checkbox field named "Vietnam Vet?" to a picklist called "Veteran Status". But this transition wasn't communicated perfectly to their staff, so some case workers weren't updating the picklist immediately. After a few weeks, everything got figured out and they stopped tracking who was a Vietnam Vet by removing that checkbox from the page layout, but there are still a fair amount of clients who are marked as Vietnam Vets but aren't marked as Veterans!

<img src="http://i.imgur.com/oEOrO5A.png" width="70%"/>

Exception reporting to the rescue -- we built a report of all the clients who were marked as Vietnam Vets but didn't have a "Yes" veteran status. In a few minutes, the staff was able to clean up those records and then was able to ditch that pesky old checkbox.

## Not just cleanup

Now, for this org, we were focused on cleaning up old entries and "rescuing" their data, but it's not always about clean up. Data is always going to be messy, so you want a way to keep an eye on it going forward. 

<img src="http://i.imgur.com/TiZLnU6.png" width="70%"/>

I like to create a report folder called "Exception Reports", so it is super easy to find and run these reports when it's time to check on your data.

The last tool in your exception reporting toolbox? [Subscribe to your reports](https://help.salesforce.com/htviewhelpdoc?id=reports_notifications_home.htm&siteLang=en_US). Salesforce gives us a great tool that will email you when report data changes. Subscribe to your most important exception reports, and get emails whenever bad data is sneaking in. 

<img src="http://i.imgur.com/QhX6MT5.png" width="70%"/>

This is just one way you can let Salesforce do some of that admin work for you!

