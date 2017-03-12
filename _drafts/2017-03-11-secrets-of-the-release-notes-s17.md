---
layout: post
category: appbuilder
title: 'Secrets of the Release Notes: Spring ''17'
tags:
  - admin
  - consultant
  - releasenotes
  - secrets
published: false
---

<meta name="twitter:card" content="summary" />
<meta name="twitter:site" content="@cdcarter" />
<meta name="twitter:title" content="Secrets of the Release Notes: Spring '17" />
<meta name="twitter:description" content="Digging into the top overlooked features in Salesforce Spring '17" />
<meta name="twitter:image" content="https://c1.sfdcstatic.com/content/dam/blogs/us/thumbnails/get-ready-for-the-spring-17-release/spring17logo.jpg" />

{% include JB/setup %}


[Secrets of the Release Notes](https://cdcarter.github.io/appbuilder/2016/08/05/secrets-of-the-release-notes) is back for the Spring 17 release, because there are some hidden gems in here. Spring 17 was a release that pretty much everyone was excited about. Huge jumps forward in terms of what you can do in Lightning pages out of the box, the beast that is picklist administration is finally tamed, and lightning components got a lot of love on the developer side. But you've heard and read about those already!

Doing "Secrets of the Release Notes" isn't just an excuse for me to write my blog post two months after everyone else without looking lazy. Instead, I like to take a look at the new release two months in. I go through the notes for a who-knows-how-manyth-time and I start bookmarking and commenting and noting. Have I used this feature yet? Do I understand it? Will I use it any time soon? Am I ready for my certification maintenance exams? 

When I take that look, there are always some things that stand out to me. For whatever reason, things it feels like I "missed" on the first pass through. Now, sometimes these really are late breaking features that weren't in the initial release notes, but others are just small little gems that I didn't notice or comprehend because I was busy drooling over Console-style navigation in Lightning.

## Free features!

### Lightning for Gmail / Lightning for Outlook

<a href="https://releasenotes.docs.salesforce.com/en-us/spring17/release-notes/rn_sales_lightning_for_gmail_improved_side_panel.htm#rn_sales_lightning_for_gmail_improved_side_panel"><img src="https://releasenotes.docs.salesforce.com/en-us/spring17/release-notes/release_notes/images/lfg-pane.png" width="70%"/></a>

Some of my favorite features in a new release are those that bring functionality to all customers that used to need paid add-ons or require ISV support. Over the past few releases, Lightning for Gmail and Lightning for Outlook have brought fantastic email app plugins that bring Salesforce into your inbox. They haven't really been a secret, but nobody has been drooling over them the way you really should. If you use Gmail or Outlook for email, get your reps this plugin.

<i>...and there is a little bit of a secret in these notes - not only has Lightning for Gmail gotten better and better, as of this release it's <a href="
https://releasenotes.docs.salesforce.com/en-us/spring17/release-notes/rn_forcecom_lfo_lfg.htm">included for Platform/Force.com license types now too</a> meaning all your users can use the awesome plugins.</i>

## Cheap Communities for Great Good

Force.com Sites are a fantastic way to expose data from your Salesforce instance to the public internet with Visualforce pages. Included free for most customers, and leveraged by popular apps like PatronManager and Volunteers for Salesforce, they're a big part of my toolbox. However, they haven't gotten any new features in a long time, and you can't use Lightning Components or even Lightning Out in them. 

Jodie Miners tipped me off to a neat trick in her [Setting up Knowledge in Communities](https://tdd.instawiki.com/display/SF/Setting+up+Knowledge+on+Communities) tutorial. If you or your client buy just a single communities user or license block (i.e. the cheapest option for your contract) - you can make that Lightning-based, super-powerful Napili community available to the general public. That's right. Drag and drop Community Builder, an AppExchange of Lightning Components, and deep integration, all available for a single Community user.

Even better, in this release [you can finally make custom Lightning Apps available for guest users in a community](https://releasenotes.docs.salesforce.com/en-us/spring17/release-notes/rn_lightning_apps_in_communities.htm) meaning your entire custom Lightning App bundle could be accessible in your public community. 

## The App Builder's Field Guide to Spring 17

### Content is Dead, Long Live Files (Content)

<a href="https://releasenotes.docs.salesforce.com/en-us/spring17/release-notes/rn_files_folders.htm#rn_files_folders
"><img src="https://releasenotes.docs.salesforce.com/en-us/spring17/release-notes/release_notes/images/rn_files_folders.png" width="70%"/></a>

First, they came for Content, and replaced it with Files (built on top of Content). Files is simpler than Content, more powerful than Documents, and looks great in Lightning. But what about Libraries? Content Libraries were a powerful way to organize all your Content. Well, if Content is Files, then Libraries would be Folders. So Shawna brought us Folders.

### Bulk API Powerup

The Bulk API is the preferred way for a large integration to interact with Salesforce. Trust me, I'm Certified (tm). But it had a very painful limitation. Though you could submit a SOQL query for what records you wanted to retrieve, you couldn't get anything from relationships or related fields. Even as much as grabbing a user's email address instead of ID for a bulk export meant multiple steps and merging files.

[NO MORE](https://releasenotes.docs.salesforce.com/en-us/spring17/release-notes/rn_api_bulk.htm#rn_api_bulk_queries)! The Bulk API got some query powerups, including the ability to use relationships, and I couldn't be happier.

## The Financial Services Cloud

Ok, I have a weird soft spot for the [Financial Services Cloud release notes](https://releasenotes.docs.salesforce.com/en-us/spring17/release-notes/rn_fsc.htm?edition=&impact=) and the team behind them. They're designing a fantastic account and relationship model that has been very interesting to watch grow. I'd love to write it up sometime, their concept of households is very elegant.

I like to wonder about that team, deep inside Salesforce. Do they look at the NPSP? They sure seem to have learned some data lessons from us. They get such flashy marketing in the formal release notes, which is very cool! But they can only release three times a year, and the NPSP team is pushing out new features every other week. I'll take  the tradeoff. 

### Alerts

They got a neat new feature in FSC that allows [external integrations to add Alerts to a client record](https://releasenotes.docs.salesforce.com/en-us/spring17/release-notes/rn_fsc_alerts.htm?edition=&impact=) by adding records via the API. The alert will show up when a user pulls up several possible views of the client, and they can act on it then, or schedule a reminder. This is a very clever way to allow another system to integrate with Salesforce without disrupting data.

## Conclusion

Spring 17 was a whirlwind release for the Lightning Experience. I feel so empowered in Lightning App Builder. But there are some less talked about features that I am sure you're gonna want to check out. Try out Salesforce for Gmail, [check out how amazing OAuth management got](https://releasenotes.docs.salesforce.com/en-us/spring17/release-notes/rn_security_identity_control_oauth_app_policies.htm) (paging Gorav Seth), and make sure you have moved your teams over to Files!

Happy S17.
