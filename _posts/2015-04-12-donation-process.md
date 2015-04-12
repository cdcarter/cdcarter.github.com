---
layout: post
title: "Major Gifts Process and Adoption"
description: "So...not everyone has a Salesforce license"
category: 
tags: [adoption]
---
{% include JB/setup %}

"In order to increase **adoption** of recording data in the **opportunity pipeline** we're going to use Work.com badges and **gamification** for our major gifts team."

That's a sentence full of buzzwords, but what it means is that the organization really wants the major gifts team to put more data on their asks (both successful and failed) into Salesforce. In order to do that, they're going to implement the Work.com free Thank You badges for when users really step up to the plate and put useful information into the DB.

![thanks badge](https://blog.internetcreations.com/wp-content/uploads/2015/01/work-thanks.png)

This is not a crazy idea. Many organizations don't see their teams putting every ask into Salesforce, but it can help your planning and forecasting to track "lost" donations and asks in progress. You can forecast how much you think you'll get back from year-after-year donors and you can track the efficacy of different types of asks. And this is great...when your major gifts team is all staff, and they all have Salesforce access. 

Maybe your board takes a big role in major gifts, or maybe you just don't have enough Salesforce licences to get every solicitor into the cloud. Obviously, in an ideal world you could get everybody who's making an ask to be able to create a new Opportunity, but sometimes that's just NOT POSSIBLE!

Rakesh Gupta, the master of point-and-click programming and a Salesforce MVP in Mumbai, wrote a great ["getting started" tutorial on using badges for gamification](https://rakeshistom.wordpress.com/2015/03/19/getting-started-with-process-builder-part-12-implement-gamification-to-your-salesforce/) but this technique requires all your solicitors to be up in the CRM and getting their hands dirty with data. What can we do when you've got solicitors who can't access Salesforce?

## Step one: Identify your process
If your solicitors can't access Salesforce, what do you want to get from them and how will you get it?

Let's assume that right now, your organization has a Google Spreadsheet that your solicitors have access to. Each line is an ask to make this year:
<table><tr><th>Name</th><th>2014 Gift</th><th>2014 Gift Date</th><th>2014 Appeal</th><th>2014 Notes</th><th>Lifetime Giving</th><th>2015 Ask Amount</th><th>2015 Ask Date</th><th>2015 Solicitor</th><th>2015 Notes</th><th>Won?</th></tr>
<tr><td>Christian</td><td>$500</td><td>March</td><td>Gala</td><td>Gave via Raise the Paddle</td><td>$2000</td><td>$600</td><td>Feb</td><td>Mark</td><td>Get coffee before the Gala</td><td>Yes</td></tr></table>

## Step two: quick integrations
What can we do RIGHT NOW with the data we're already collecting? Well, in this case, as long as your solicitors are following and updating the spreadsheet for each other's benefit, there's already a LOT that you can pull in. One member of your team can take a few hours and create all these opportunities in Salesforce. As people update the spreadsheet, someone can take responsibility and ownershio of updating Salesforce when new data comes in. To make it easier, you can even add a column in the doc with a link to the opportunity!

## Step three: Chatter Free
Once data is going into Salesforce, Salesforce can start to reason about it, and can give some nice thank you badges to successful solicitors! Now, your board members might not have access to the full CRM, but that's no reason not to create a Chatter Free account for them. With Chatter Free they'll be able to join groups, communicate with your staff on Salesforce, have reports easily shared with them, and more. They'll even be able to be given Work.com badges.

Though they can't access the opportunity object themselves, you can now use Rakesh's process to give them badges when a donation they are a solicitor on is complete! They don't even have to log in to Chatter, as their thank you badge will be sent by email automatically. This can be a VERY neat tool for letting them know when a gift actually arrives (instead of just a verbal promise).

## Step four...five..six: Deeper integrations
Now it's great that you have a team member processing spreadsheet updates every day and tracking the stages of your asks, but why should they be doing a robot's job? There are a few different deeper integrations we can engage in here:
1) Have Salesforce talk to the spreadsheet. With a qualified Apex developer, it's not too hard to connect your database to the spreadsheet to keep everything updated live.
2) Have the spreadsheet talk to Salesforce. If you know and love Javascript but not Apex, you can use the opposite method and use Google Sheets Script to send updated data right to Salesforce.
3) Listen to Chatter? This is a TRICKY one, but you could even use a Flow/Process Builder pairing to update an opportunity based on posts in a Chatter feed that your users DO have access to. More on this next week ;)

## What else?

How do you solve the problem of getting data into Salesforce and thanking your non-CRM users for it? Comment on the Power of Us HUB and I'll update the post with your thoughts!
