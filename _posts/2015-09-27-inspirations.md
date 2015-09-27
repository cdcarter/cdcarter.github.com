---
layout: post
title: "Y'all Inspire Me"
description: "Two months of a great community"
category: roundup
tags: []
---
{% include JB/setup %}

My blog has been a bit quiet lately, and that's mostly because I'm just overcome with so many ideas and things that I want to work on and blog about that...nothing happens.

While I'm working on some really (I think) cool posts about documentation and designing for Lightning right now, I wanted to just share what I've been up to in the past two months and the inspiration I've received from those moments!

## PatronManager Community Meeting
In Late August, I had the pleasure of flying out to NYC to attend the PatronManager Community Meeting. For those dear readers unfamiliar with it, [PatronManager](http://patrontechnology.com) is a full featured ticketing and fundraising system built on top of Salesforce. PatMan is how I got my start on the platform and their staff encouraged and taught me and mentored me in untold ways. It was amazing to meet so many of the people I'd been emailing with and tweeting at for months.

### Building a Lightning Page

![Early Morning Consulting](https://pbs.twimg.com/media/CNg14VPW8AAxZ_L.jpg "Building the App")

[Caroline Renard](http://twitter.com/cprenard) (a never ending inspiration, I might add!) and I gave a very fun presentation on using the Salesforce1 App to manage your patron relationships. We talked about a lot of standard features, but at the very end we demo'd our idea for a Lightning App that allowed quick access to the day-of-show ticket list. 

One attendee, Melissa from the [American Shakespeare Center](http://www.americanshakespearecenter.com/) was so excited by what we demo'd that she convinced Caroline and I to skip sessions the next morning and build it in her org right there in the lobby. This turned out to be great! She asked questions and pushed us for features we hadn't considered when we first spec'd the demo. The most exciting thing for her? Quickly being able to Chatter on related records &mdash; it turns out that the operations staff really loved Chatter but having to be by a computer to use it while literally running everything was a bit of a difficulty. We put Chatter in her pocket!
### Documentation?
The best part of the conference for me (besides seeing dear friends) though was sitting in on the Hands on Track room, and hearing what kinds of questions people were asking. PatronManager, just like the NPSP, is really just a starting point and each org configures it to suit their needs. 

Claire Randall and I had a fantastic conversation about the need for each organization to document their specific uses, their fields, the way they use terms, and more. This helps keep your data clean, and this helps the organization out when an admin moves on to somewhere new. This has been a huge inspiration for me, and I've been working on a post about starting your "Data Dictionary" as a first step in documenting your org. 

## The Blogs
The non-profit Salesforce blog world keeps getting larger and larger, and that is hugely exciting to someone like me! After all, what am I going to read when my code is compiling or my tests are running?
### Michelle and Ally do Page Layouts!
[Michelle Paul](https://twitter.com/fuzzydinosaur) and [Allison Klein](https://twitter.com/nyalli) launched their great new blog, Spotlight on Salesforce, just before Dreamforce. They have written a ton of great posts, but one struck my attention. [Spring Cleaning](http://www.spotlightonsalesforce.com/blog/2015/8/7/cleaning-up-fields-on-a-contact-record-need-better-title) describes their process of cleaning up a very very messy Salesforce org, and walks us through decisions they made. The post focuses on cleaning up what is probably the most important page in a non-profit's Salesforce instance, the Contact Detail page. If this post doesn't get you firing up a sandbox, I don't know what will!

I recently pushed a client project into a Winter '16 sandbox to see JUST how "Lightning Friendly" I was being, and I quickly realized that the best thing I could do to make this project ready to go with Lightning was design more intuitive page layouts. This post gave me the tools I needed.
### Cooking with Code &mdash; SOQL Aggregates
I was lucky enough to be named a Hub Hero in July alongside [Kieran Jameson](https://twitter.com/kierenjameson) and holy cow she is a HERO! A little behind the times, I recently stumbled on her three part series on SOQL, and [the third part](http://womencodeheroes.com/2015/04/cooking-with-code-a-sweet-intro-to-soql-part-three/) is just GENIUS. I'm a pretty SOQL savvy guy, but I never did really understand when and where to use aggregates. Turns out, I just needed someone even smarter to explain them to me!

## The Community
### Vered on the Hub!
My Admin Keynote buddy and generally #AwesomeAdmin [Vered Meir](https://twitter.com/saltyfem) got inspired at Dreamforce and started working on a very complicated Process Builder/Flow combination to reduce some of her Admin work. Go read [this post](https://powerofus.force.com/0D58000002I4VUT), and see the community spring into action!
### Crowd Sourced Process in the Client Community
And in the PatronManager Client Community, people are discovering Process Builder. People are blown away by the power of workflow automation... 

## Dreamforce!
Here's the big one. I went to my first Dreamforce, and WHOA guys. That's a pretty intense week. Why didn't you warn me? Oh wait, you all did. 

I had a pretty fantastic time, though I'll admit I spent almost as much time hiding away in the Moscone West Speaker Prep Room having quiet conversations than I spent in formal sessions. But even doing that, I learned so much from this week, and I met SO MANY great people. Nonprofit MVPs and #AwesomeAdmins were running around everywhere, and I was excited to see so many people from the Hub in real life finally.

### Ashima Saigal and Tracy Kronzak
These two get their own special shout out. 

I had the absolute joy of joining [Ashima Saigal](http://twitter.com/ghandilover) and my co-worker V in their presentation [Triggers for Admins: A Five-step Framework for Creating Triggers](http://salesforce.vidyard.com/watch/Fj8YhFJhsRvM1RjjwGKC_g) as the prop boy. Though my (dare I say hilarious) performance wasn't captured in the video &mdash; I'm not upset. Meeting Ashima and talking about mindfulness, consultant empathy, learning styles, teaching techniques, and ice cream was a total treat. I didn't know a lot about [Database Sherpa](http://databasesherpa.com/) before, but now I am VERY inspired by this model for client success.

![A great shirt](https://pbs.twimg.com/media/CPJtFMHVAAAG1LG.jpg "my favorite shirt")

At the Wednesday night "Meet the MVPs" event, I finally got to meet the one and only [Tracy Kronzak](https://twitter.com/tracykronzak). You may remember her from the incredibly campaign she waged to get non-profit voters in the Idea Exchange, or you might just know her from being ALL over the Hub and always helping out. At Parker Harris' True to the Core, I jumped up to ask about the Opportunity Contact Role idea that Tracy championed, and though Parker may have disappointed us with the answer, this gave Tracy and I plenty to talk about the next day. Make sure to follow her on the Hub.

### Open Source Development of Managed Packages

The team that develops the Nonprofit Starter Pack is pretty much the coolest group of people in Salesforce, and they all came together in one room for [Open Source Development of Managed Packages](http://salesforce.vidyard.com/watch/Zseynwpk0fl4ENYxya3kPQ). This session was full of tricks that Jason, Judi, Kevin, and the community as a whole learned over the past few years, and I was so thankful to learn from them.

I also had the pleasure of being featured in this presentation for a TWO-CHARACTER [pull request](https://github.com/SalesforceFoundation/Cumulus/pull/1661) I submitted in July. Reminded of how easy it is to fix the NPSP, I actually just submitted a [*one-character* pull request](https://github.com/cdcarter/Cumulus/commit/be31cc4ee4e4a2681c330b8d9582e9babdde7eef). Does this mean I'm not allowed to write ANY code at the upcoming Developer Sprint?

### NPSP on the Go!

And my last moment of Dreamforce (literally, I left this session to get on a plane) was my own! Caroline Renard somehow convinced both me and the Foundation that I should present a version of our PMCM talk, and the result was [Nonprofit Starter Pack on the Go!](https://success.salesforce.com/Ev_Sessions?eventId=a1Q30000000DHQlEAO#/session/a2q30000001CIf7AAG) Since this was my first Dreamforce, this was also my first time presenting at Dreamforce. It was exhilarating to show off something I'd been working on for a long time, and it was so inspiring to have people line up after my session to ask questions about the mobile app.

## Seriously
This community is amazing to be a part of. Thanks for inspiring me!
