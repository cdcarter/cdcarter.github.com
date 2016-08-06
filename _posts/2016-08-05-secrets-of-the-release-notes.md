---
layout: post
category: appbuilder
title: 'Secrets of the Release Notes: Summer ''16'
tags:
  - admin
  - consultant
  - releasenotes
  - secrets
published: true
---

{% include JB/setup %}


This week, I went back through the Summer '16 release notes, as we're coming up on the "two month anniversary" of the release. Now, that's not a date I mark in my calendar, but I thought it might be worth going back to re-read the release notes now that I'm used to all the new shiny features that I got excited about in May when they first came out. 

It turns out, there was a lot that I missed. So, I'm trying something I always try, which is shamelessly steal from my genius friends at [PatronTechnology](http://patrontechnology.com), [Michelle Paul](http://twitter.com/fuzzydinosaur) and [Ally Klein](http://twitter.com/nyalli). Every year, at the PatronManager Community Meeting, they run a session titled "Secrets of the Release Notes", and they run through all the things you might have missed in their product over the last year. 

I started up a [Trello board](https://trello.com/b/HZ6xCfd0/secrets-of-the-release-notes-summer-16) like I do for any project, and started pinning tabs and copying them into cards and adding commentary. As I worked through the notes, I found things that stood out at me as higher level goals of the release that I didn't notice at first. All in all, I broke my "findings" down into four categories: 

- features that it's absolutely shameful I missed
- a LOT of good support for Lightning adopters (shocking!)
- great productivity features for mobile users
- features that I was ignoring for too long, and got good!

So, without any further ramblings, here are the top ten...

## BIG DEAL ALERT! How'd I Miss These?

### Two-Factor Authentication: For Your Data's Sake

> [Save the Day by Generating a Temporary Verification Code for Users in Distress](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_security_auth_temp_codes.htm#rn_security_auth_temp_codes):

When I read the release notes last summer, I totally skimmed past a lot of the security stuff, which isn't what I'd recommend as a best practice. So I'm really glad I went back and re-read this, because this is actually one of a series of notes in this batch of release notes that solidified it for me. **You need to have two factor authentication in your org**.

It's that simple. Two-factor auth will keep your data significantly more secure from all sorts of vulnerabilities. It's easy to implement. There's a high quality app. And this release brought a lot of new features, like this one, that make using it as easy *or better* than life without it for your users. So please, turn on two-factor auth and get your users on their phones. 

### Social Customer Service

> [Social Customer Service for All](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_social_customer_service_default.htm)

It's ridiculous that I didn't know that Social Customer Service, for up to two listening accounts, is completely free for all EE customers and it's totally built in.

No more having to download apps and contacting Salesforce to try it out. 

Social Customer Service and Social Listening is very meaningful to more than just contacting Alaska Air -- for service delivery nonprofits, if your constituents are on Facebook and Twitter as their main source of communication -- you need to be there too.

### Fix Percents in Process Builder!! Thank You Shelly!!

> [Trust Percent Values in Flow sObject Variables Again (Critical Update)](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_forcecom_flow_percentage.htm)

Have you been building awkward workaround hackish awful Workflow Rules and Formula fields because percentages just don't work right in Flow and Process Builder? I have. 

Well, there's a Critical Update for that! I know, I know, I ignore critical updates all the time. But if you're just starting on your PB or Flow journey or know you don't have to worry about breaking too many processes, GO ACTIVATE THIS ONE, and save yourself some headache later.

(as vehemently as I just wrote GO ACTIVATE THIS, if you don't know what the impact of this will be, be very very careful. *Do not activate this in production without analyzing and testing every process and flow in your org.*)

## ZAP! Lightning Adoption!

### Lightning Experience Readiness Check

> [Get More Insight Into Whether You’re Ready for Lightning Experience](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_general_lex_readiness_check.htm#ren_general_lex_readiness_check)

When we were down in Chicago for the MVP Summit, Shawna Wolverton showed off the Lightning Experience Readiness Check like it was something I should have known about.

It was something I should have known about. This is incredibly useful for orgs who are considering moving to Lightning. By analyzing your org and what features you may be using that are incompatible with Lightning, it's a lot easier for you to figure out what you need to rebuild!


### Slim Mode Lightning

> [Highlights Panel Shows off More Fields](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_sales_productivity_highlights_panel.htm#rn_sales_productivity_highlights_panel)

A frequent complaint about the Lightning Experience is that it decreases information density, and takes more time to use. Throughout this release we saw Lightning get tighter and cleaner, but here's a bit that I didn't notice and wish I had. This helps a lot!

### Lightning Pages Everywhere

> [Override a View Action with Both a Visualforce Page and a Lightning Page](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_forcecom_general_action_overrides.htm)

In this release, Lightning Pages and the Lightning App Builder got a lot of love, which is a good trend for Salesforce to be moving in. 

This is just one of those things that makes Lightning adoption a lot easier. Because of this, you can have record detail page overrides that look at home in LEX in LEX and look at home in Classic in Classic. 

This allows for orgs that are adopting Lightning one business unit at a time to create new and more efficient Lightning App Pages to replace oldschool Visualforce pages, without losing that functionality for the users still in Classic.

## Are You Using Salesforce1 Yet?

### Pictures in Notes

> [Salesforce1: Just Picture It—Visual Notes in Salesforce1 for iOS](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_mobile_s1_newfeat_notes_add_images.htm)

Put a picture in a note, just like that! It's nothing novel, but this is more Salesforce1 polish that lets me recommend it in good confidence as not just a way to access App Cloud data, but as a solid productivity app.

### Images in Reports

> [Salesforce1: View Custom Formula Field Images and Links in Salesforce1 Reports](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_mobile_s1_otherfeat_reportsdashboards_custom_field_support.htm)

HOLY COW. THIS IS YUUUUUUGE. 

It used to be that all your beautiful at a glance visual indicators and frosting and record links **just didn't work** in Salesforce1. It made using many reports downright impossible from the mobile app. Gone are those troubles.

## Things That I've Been Ignoring, That I Wrote Off Too Soon

### Streaming API

> [Replay Events with Durable PushTopic Streaming (Generally Available)](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_api_streaming_classic_replay.htm)

What's that? A bunch of technical jargon? Well, yes, but this feature going GA represents something pretty big. If you're a busy consultant or admin, you can't always check everything out. Word on the street was that the Streaming API was kinda unreliable, so personally, I just ignored it and never really checked it out.

This update, durable events, makes the Streaming API a *lot* more useful for many ISVs and highly integrated orgs.

### Data Import Wizard

> [Data Import Wizard Enhancements for Matching by External ID](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_forcecom_data_diw_match_by_external_id.htm)

The Data Import Wizard -- another feature that I've just historically ignored because word on the tweet was it just wasn't sufficient. Instead, I'd reach for trusty dataloader.io.

But dataloader.io has limits for free users, they're a business after all! If you need something completely free, in my opinion, with this feature the Data Import Wizard has actually become viable for regular use. 

Plus, it uses the Bulk API which is a huge win.
