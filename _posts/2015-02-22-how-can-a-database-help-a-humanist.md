---
layout: post
title: "How can a database help a humanist?"
description: "Visual Workflow can do a lot!"
category: 
tags: [getting-started]
---
{% include JB/setup %}

## Our Challenge: Second to Last Gift

It's a not-uncommon question on the [Power of Us Hub][1]. In the NPSP, how can I calculate the second to last gift that a donor made? This can be very useful when reporting to a development committee on the board. I had to implement it in order to do an "increased gift" challenge match. There's a pretty quick and dry way of doing this, with a basic workflow or process. Scott Mostrom of [ACF Solutions][2] offered this suggestion to a user on the hub:

> One approach is to create a new custom currency field (Second to Last Gift Amount) and use a workflow rule to populate the prior value every time the Last Gift Amount changes. The workflow can be based on every time a record is created or edited and the formula ISCHANGED(Last Gift Amount). The field update populates your new field with the formula PRIORVALUE(Last Gift Amount). This should work well going forward but the shortcoming is that this won't kick in on contacts until new gifts are received from them and the current Last Gift Amount changes. So, if you wanted to get all the records up to speed now you'd need to do some data work on the existing opportunities to determine the right dates and mass update that field.

The shortcoming he points out is valid, but ther'es a slightly more dangerous one. Thomas Taylor from [roundCorner Inc.][3] fills us in:

> Scott's solution is very good and simple, but I think it's important to understand an important limitation. Note that it is not actually finding and populating the field with the value of the next-to-most-recent gift, it is merely taking the previous value of Last Gift Amount and copying it over.
> So, if you end up changing the Amount of the Opportunity represented by that Last Gift Amount, to correct a data entry error, for instance, then your second-to-last gift amount will then have the incorrect amount from the initial entry in it.

Now that's a pretty hefty pitfall. We all know that even with the best intentions, data changes a LOT. And this isn't just some ad-hoc formula on a report, we're actually saving this value onto the Account. Do you want your users worrying if the data is fresh? Do you want them second-guessing this field, causing them to lose confidence in other parts of your CRM too? At first it seems like your only option here is to hire a developer or live without it. 

## Visual Workflow and Process Builder

Actually, we can solve this problem without a single line of Apex. I'll say from the outset, this method still has the initial caveat, it only updates data going forward. There's a trick for that too, but we'll get there later. I'm going to assume you're on the NPSP Version 3.0, but nothing here is particularly tied to that.

### Step One: Define the data

Our first step is to qualify our problem a little bit more, using some of the terms we're going to see in the Flow designer. We want to set a field on `Account` called `Second_to_last_gift__c` that contains the `Amount` field from the 2nd to latest `Opportunity` with a `Stage` that has the Closed/Won property. Hop into a Sandbox and create that Currency field on `Account` to save the value in. Make sure to add it to your page layouts too. 

### Step Two: Layout your flow

We're going to use the Cloud Flow Designer to build a visual workflow. Here are the steps we'll need it to accomplish:

1. Set up an `OpportunityId` variable. This flow is going to be triggered by a Process every time a donation is created or edited, so we'll need an input-only variable to stash the ID of the `Opportunity` that was saved.
2. Save `AccountId` from the Opportunity into a variable.
3. Find _all_ the Closed/Won donations for that `Account` and stash them in an SObject Collection Variable, sorted by `CloseDate` (descending).
4. Create an index variable with the value of `0`.
5. Loop through our SObject Collection (which entails creating a loop variable, and select an "ascending" loop)
  1. If the index variable is not equal to `1`:
    * add `1` to the index variable, and return to the loop.
  2. If the index variable is equal to `1`:
    * Update the `Account` `Second_to_last_gift__c` with the `Amount` of the `Opporunity` from the collection that we are currently on.
6. Save the flow as an Autolaunched Flow.
7. Activate the flow!

### Step Three: Build a process

Now that we have our flow to do a bulk of the work, let's set up a Process to launch it. This is going to be a VERY simple process, we just need to start the flow whenever an `Opportunity` is saved!

1. Create a new Process on the `Opportunity` object, to be triggered whenever it is saved or edited.
2. Set the first Criteria to "No criteria—just execute the actions!" I named mine "No Criteria" so it would be easy to remember what is happening. 
3. Create an action that launches your Flow! Process Builder will then give you the chance to set any flow vairables. Set your `OpportunityId` input-only variable to `[Opportunity].Id`.
4. Activate your process!

### Step Four: Test it out.

Go ahead, create a household, and save a donation on it. The new field will be empty. Create a second donation, and look again! Update that first donation... Whoa! It keeps our data in sync. Now we can add it to reports, and feel confident in our data.

### That stupid caveat
As I mentioned before, this will only update the field _going-forward_. It won't go back and immediately calculate Second to Last Gift for every single one of your households. This might be OK for you, you can get it set as you go forward. If not, you have a few tactics. I can't recommend one of them without knowing you or your data, so be careful here, don't get too cocky, test things out first, and when in doubt, hire a Salesforce Partner to help you out. But, you could:

1. Write a batch job in Apex to take care of all this.
2. Use a data loader tool to trigger an inconsequential edit on all of your donations. Remember, this process launches on ANY edit to an `Opportunity`, so if you were to use [Apsona][4] to pull all your `OpportunityID`s and then update each one (perhaps only update a new checkbox called ToTriggerFlow?), you'd get all your fields updated.

Choose CAREFULLY how you'd want to do this batch work, and take an export before you begin.

### Clicks not Code
This process and flow combo is very powerful, and just one example of what you can do as a Salesforce Admin without a line of code. An Apex developer would think that it's pretty funny that you need to track your index variable manually, but you didn't have to hire a developer to do this, so how will they know to laugh?

[1]:http://powerofus.force.com
[2]:http://www.acfsolutions.com/
[3]:http://www.roundcorner.com/
[4]:http://apsona.com/pages/sfdc/index.html
