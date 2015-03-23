---
layout: post
title: "Ugh...lookup relationships..."
description: "They aren't that bad!"
category: 
tags: [process-builder]
---
{% include JB/setup %}

Lookup relationships are frustrating. Okay, that's a bit of an odd statement, but if you've been around the SFDC as an admin for even a few months, you've already run into a BIG limitation.

## You can't rollup from a lookup relationship.

Maybe there's a serious technical reason for this, something deep in the SObjects, the Apex, the Governor, whatever. It's annoying. And to add insult to injury, you can't make a standard object the Detail side of a Master-Detail relationship. Admittedly, that one makes a LOT of sense (remember, M-D relationship have the basic premise that there's no Detail record without the Master), but even though it makes a lot of sense, it causes a lot of pain! 

What's an admin to do?

Yes, you can always commission a developer (in the arts world we say commission, I bet you consultants would say "hire"), but if you haven't caught on to the DIY-nature of my blog yet, let me reveal a hidden secret. My organization does NOT invest in it's CRM. So I learn instead. That's the **#SmallButMighty** lifestyle.

I wanted to rollup Opportunity amount to my "Pledge" object to see how much had been paid on the pledge. I'm also looking at rolling up Opportunity amounts for a Group Sales system.

So, without writing custom code, we look to useful tools on the AppExchange. In this case, enter the...

# [Declarative Lookup Rollup Summary Tool](https://github.com/afawcett/declarative-lookup-rollup-summaries)

[Andrew Fawcett](https://github.com/afawcett) of [Andy in the Cloud](http://andyinthecloud.com/) fame wrote together a brilliant tool. He's written some crazy flexible code that let's you define rollup summaries in a point-and-click fashion through the standard Salesforce UI.

The UI looks like this:

<img src="https://s3.amazonaws.com/dfc-wiki/en/images/9/95/Figure4_rollup.png" width="600" />

And you can rollup any field on the "many" end of a lookup relationship to your object on the "one" end. You can specify criteria to only rollup certain records or `RecordTypes`.

Up until recently, this tool relied on a little bit of voodoo known as the Metadata API to actually deploy an auto-generated trigger to your Production environment. If you know enough to be dangerous about Apex, you know that you can't create new Apex in a production org, you have to make it in a Sandbox. This tool used the API to "circumvent" this (though in a totally allowed way!)

But with the magical Spring '15, it got easier. 

## My best friend, Process Builder, comes to the rescue.

Now, instead of worrying about that trigger (and being certain that all your tests, even the ones you didn't write, pass), you can just define your rollup to be triggered by the Lighting Process Builder

<img src="https://andrewfawcett.files.wordpress.com/2015/02/processbuilder2.png?w=820&h=559" width="600"/>

It's that easy. Within minutes, you can rollup any field to any object. An admin's life made that much simpler. I spent today buying new socks and towels (think about it, what doesn't make someone feel cleaner and happier than fresh socks and towels) and then I sat down to work and was greeted by a VERY pleasant experience that had immediate (and desired) results. Thanks Andy!
