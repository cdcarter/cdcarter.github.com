---
layout: post
title: "The Lookdown Lookup"
description: "New Feature in DLRS"
category: admin
tags: [dlrs]
---
{% include JB/setup %}

The Lookdown Lookup is a Salesforce technique I find myself implementing more and more frequently. When employed properly, you can overcome email template cross-object issues, you can include all sorts of data into list views, you can report on some criteria more easily, and kind of totally expand your view of data.

Okay, that was a bit baloney, the Lookdown Lookup can do all of that, but for a very specific and annoying circumstance. This is a technique you should use when you have a lot of related records to an object, but you care a lot more about one.

For example, if you're tracking the residencies of your clients, most of the time you only really care about their current residency. When looking at a volunteer's record, you care most about their next upcoming shift. Wouldn't it be great to pull a list view of all your volunteers, sorted by the month their next shift is? 

## The Data Model

So what's the technique? The Lookdown Lookup is structurally fairly simple. On a parent object (like `Contact`) that has child records (like `Residency__c`), you create a lookup relationship that "looks down" to the one you care about (e.g. `Current_Residency__c`). Then, on your parent object, you can create formulas that reach over to that key record `Current_Residency__r.Unit__r.Unit_Number__c`. Or, you include fields from that record you care about in a report.

## How To

In the past, you could implement this with a simple Process Builder/Flow combo or a custom trigger. But this was a problem I kept running into, and I knew it'd fit right in with my favorite package, Andy Fawcett's [Declarative Lookup Rollup Summary](https://github.com/afawcett/declarative-lookup-rollup-summaries) tool (DLRS). So, I did what any good community member does, and I wrote a patch to the tool to support this feature. It got merged into the latest release, so now anybody can take advantage of it.

Let's look at how we set up the current residency with DLRS. We'll assume that you've already installed DLRS and a remote site setting, and we're working with the following schema:

<img src="https://dl.dropboxusercontent.com/spa/q8pc7mthv83x9i1/dlrs-lookdown-lookup/images/docs/dlrs-lookdown-lookup/schema-builder-~-salesforce---developer-edition.png" alt="Contact, Residency, and Rentable Unit Schema" width="100%"/>

Here we have a junction object named Residency between Rentable Unit and Contact. Residency has a start date and end date, which calculate the "Current?" formula field. There's also a Lookdown Lookup from Contact to Residency named "Current Residency".

### Setup DLRS

Now let's pop over to DLRS and start a new rollup summary

<img src="https://dl.dropboxusercontent.com/spa/q8pc7mthv83x9i1/dlrs-lookdown-lookup/images/docs/dlrs-lookdown-lookup/lookup-rollup-summary-edit--new-lookup-rollup-summary-~-salesforce---developer-edition.png" alt="Rollup Setup" width="70%"/>

We give our rollup a name, set the parent and child objects, and set a relationship criteria that we only want to deal with Current residencies. This will keep the query more performant.

<img src="https://dl.dropboxusercontent.com/spa/q8pc7mthv83x9i1/dlrs-lookdown-lookup/images/docs/dlrs-lookdown-lookup/lookup-rollup-summary-edit--new-lookup-rollup-summary-~-salesforce---developer-edition-1.png" alt="Rollup Details" width="70%"/>

Then we move down to the rollup details. We want to aggregate the `Id` field of the child record into the lookdown field on Contact. Then, select the Aggregate Operation "First" and set the order by phrase to sort by Start Date, descending. That will get us the *most recent* "Current" residency (yes, there should never be more than one current residency, but it's still good to know what our code is doing if there is weird data).

<img src="https://dl.dropboxusercontent.com/spa/q8pc7mthv83x9i1/dlrs-lookdown-lookup/images/docs/dlrs-lookdown-lookup/lookup-rollup-summary--set-current-residency-lookdown-~-salesforce---developer-edition.png" alt="Manage Child Trigger" width="70%"/>

Once saved, hit the "Manage Child Trigger" button and then the "deploy" button to put a trigger into your org. Note: DLRS also supports a Process Builder mode ([which I wrote about a while ago](http://cdcarter.github.io/2015/03/22/lookup-rollup/)) as well as a scheduled mode. If you see performance issues with the trigger, you may want to use scheduled mode.

<img src="https://dl.dropboxusercontent.com/spa/q8pc7mthv83x9i1/dlrs-lookdown-lookup/images/docs/dlrs-lookdown-lookup/lookup-rollup-summary--set-current-residency-lookdown-~-salesforce---developer-edition-1.png" alt="Active checkbox" width="70%"/>

After you've deployed the trigger, you need to activate the rollup record.

### Try It Out

It's done, so let's go create a client and some residencies!

<img src="https://dl.dropboxusercontent.com/spa/q8pc7mthv83x9i1/dlrs-lookdown-lookup/images/docs/dlrs-lookdown-lookup/contact--andy-akimbo-~-salesforce---developer-edition.png" alt="Client Record" width="70%" />

<img src="https://dl.dropboxusercontent.com/spa/q8pc7mthv83x9i1/dlrs-lookdown-lookup/images/docs/dlrs-lookdown-lookup/contact--andy-akimbo-~-salesforce---developer-edition-2.png" alt="Residencies" width="70%" />

And once those are built, check out our client's current residency!

<img src="https://dl.dropboxusercontent.com/spa/q8pc7mthv83x9i1/dlrs-lookdown-lookup/images/docs/dlrs-lookdown-lookup/contact--andy-akimbo-~-salesforce---developer-edition-1.png" alt="Rollup Worked" width="70%" />

Now we can easily grab residency information for formula fields, list views, and more.

## Other uses

Where else could you put a Lookdown Lookup? Here are some real world examples of where organizations have needed this kind of information.

* If you get client demographic information on a child record, like a survey result object, you can set a lookdown lookup to the most recent survey result with demographic information filled out. Then, a contact report can include their most recent demographics.
* Just the other day on the Power of Us Hub, a nonprofit wanted to pull details about the most recent campaign a donor had participated in onto the Contact record. This is easy to do with a Lookdown to Campaign.
* The "Primary Affiliation" field in the NPSP is a perfect example of the Lookdown Lookup. You can easily pull over information about how to reach a donor at work thanks to that additional lookup.
