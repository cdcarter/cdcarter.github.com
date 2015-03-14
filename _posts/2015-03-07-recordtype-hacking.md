---
layout: post
title: "RecordType Hacking"
description: "Another challenge: how do you have different create and save layouts?"
category: 
tags: [process-builder]
---
{% include JB/setup %}

## Our Challenge: Separate create and save Page Layouts?
> How can my "new" record layout be different from my "edit" record layout?

You're clicking around Setup, trying to find a way to let your "new"p record layout look different from when you're editing a record. Maybe on a new record you only want to capture key data, or the requirements for a new record are just more stringent than some historical data. You're pretty familiar with SFDC, so you know that Cases allow you to have a new layout for closed cases. How can you do something like that on your own instance?

Well, it's prettily easily done with VisualForce, SFDC's own markup language, but VF has a  learning curve. Plus, then you'd have to maintain one page layout in the standard Setup pages, and one layout in VisualForce. Even if you are a VF-hero... this makes your maintenance even harder.

Thankfully, you can solve it with a little bit of deductive reasoning and the new Process Builder. Let's think it through, the way you determine what page layout is displayed is via `RecordType`. With a little bit of wacky thinking, we can see that what we are going to do is create a new record with one `RecordType`, and then on save, change the `RecordType` to something else!

### Step one: define our problem

In my case, we're working with [a slightly non-standard instance of Salesforce][http://patronmanager.com], where each `Opportunity` represents one (and only one) payment. If you take in multiple payments, they each get their own `Opportunity`, and in order to have multiple payments all link to one pledge (pretty standard in many organizations), you need to create a parent `Pledge` object. If you're on NPSP, this specific problem won't bite you, but any situation where an online donation creates a new `Opportunity` will create a need here.

So, we have `Pledge__c`, and we've added an additional relationship, `Opportunity` has a lookup to `Pledge`. When we create a new payment (`Opportunity`) by hand, we want it to automatically copy over many fields that are stored on the pledge (Type, Primary Campaign, Fund, Fiscal Year Applied, etc...). 

In fact, when we create a new `Opportunity` from the Pledge's record page, we want to ONLY show a few key fields, and the rest will be autopopulated.

### Step two: create a new `RecordType` and Page Layout.

Let's create a new `RecordType` called `PledgePaymentINTERNAL`. We're putting "internal" in all caps there, so that we can quickly visually see when any record is left in that type.  Then, create a new Page Layout called "New Pledge Payment" that only has the fields we need: Name, Amount, Close Date, Stage, Probability, Account, and Pledge). We will assign this Page Layout to the `RecordType` we created earlier for all profiles.

### Step three: create a custom button on `Pledge`

In Setup > Customize > Opportunities > Buttons, Links, and Actions we want to create a custom button of type "List Button". We're going to do some "URL Hacking" to make this button go to a new Donation record of type `PledgePaymentINTERNAL`. Ray Dehler wrote a [great writeup][http://raydehler.com/cloud/clod/salesforce-url-hacking-to-prepopulate-fields-on-a-standard-page-layout.html] on URL Hacking, which is worth reading. The general method is you want to find a working URL to what you need, and then modify as you see fit.

I hope to get another post up about URL hacking in some time.

My final URL for the button looked like this, yours will need some modification (you'll have to find Custom Field IDs, check out Ray's writeup):
> https://na10.salesforce.com/006/e?CF00NF000000CbzdM={!Pledge__c.Name}&CF00NF000000CbzdM_lkid={!Pledge__c.Id}&retURL=%2F{!Pledge__c.Id}&RecordType=012F00000019gFf&ent=Opportunity

### Step four: Build your process

Now the fun part! Building a process to change the `RecordType`. To do this, you'll need to make note of the `RecordType` IDs for your standard Donation type as well as  `PledgePaymentINTERNAL`. These IDs can be found in Setup > Customize > Opportunities > RecordTypes. Click on the type you want, and it's ID will be in the URL. For my payment type, the URL is

> https://na10.salesforce.com/setup/ui/recordtypefields.jsp?id=012F00000019gFf&type=Opportunity&setupid=OpportunityRecords

So my ID is `012F00000019gFf`.

Head on over to Setup > Create > Workflows & Approvals > Process Builder and click New for a new Process. Give it a descriptive name like "ChangeOpportunityRecordType" and begin.

1. Set the object we are working on to `Opportunity` and start the process "when a record is created or edited".
2. Create your first Criteria and name it "Is Payment?"
   1. Set filter conditions `[Opportunity].RecordTypeId` "equals" whatever your `PledgePaymentINTERNAL` ID was.
3. Create a new Immediate Action of type "Update Records"
   1. Name it "Update RecordType"
   2. Set Object to `Opportunity`
   3. Select field `Opportunity Record Type` and update the value to your normal donation `RecordType` ID.
4. Create any other actions you may want (such as updating other fields)
5. Activate the process!

## Step five: test it out.

As always, you want to test this! Try a few donations, and make sure you've set your IDs correctly and that everything is coming out happily.

When it is, a new record will show only the fields you want, and once you hit save, you'll see all your normal fields.

The big caveat here is that you're overloading `RecordTypes` in a way they aren't fully intended to be used. Make sure you still set all your appropriate sales processes and picklist values for both `RecordTypes` and regularly check that no data is persisting in the INTENRAL type.    



