---
layout: post
title: "A Lightning App Pattern Language"
description: "Why we need patterns"
category: patterns
tags: [lightning]
---
{% include JB/setup %}

In 1977, Christopher Alexander and the Center for Environmental Structure in Berkeley, California published _A Pattern Language_. The premise of the book required a radical transformation of the architectural profession, and presented a language of 253 patterns that everyday people could use to describe, design, and construct the homes and buildings in which they wished to dwell. Each pattern is an answer to a design problem, a solution that the team at the Center collected in their eight years of study. How does the book work? In Alexander's own words

> All 253 patterns together form a language. They create a coherent picture of an entire region, with the power to generate such regions in a million forms, with infinite variety in all the details. 
> It is also true that any small sequence of patterns from this language is itself a language for a smaller part of the environment; and this small list of patterns is then capable of generating a million parks, paths, houses, workshops, or gardens.

The patterns are thus both prescriptive (they offer one solution to a specific problem) but also composable. One of the members of the Center created a small porch at the front of the house. 

* PRIVATE TERRACE ON THE STREET (140)
* SUNNY PLACE (161)
* OUTDOOR ROOM (163)
* SIX FOOT BALCONY (167)
* COLUMNS AT THE CORNERS (212)
* FRONT DOOR BENCH (242)
* RAISED FLOWERS (245)

The designer selected a set of patterns that resonated with him, and working through from more creational/structural to more embellishments. It's almost a beautiful little MVP and iterations ;).

Over time, the professional software development community started to explore a concept of design patterns, inspired by Alexander's work. However, in practice, they are mostly patterns without a language. They are implementation focused. They are often about how the code will do something, or adding a layer of abstraction for consistency. Patterns like factories and builders are what come to mind. This work is valuable, but a designer couldn't assemble a list of them into a sequence in such a way that a builder could create something out of them. Yes, some of Alexander's patterns explain in detail *how* to do something. But they're still strung together with other larger picture ideas about what you're trying to make and how you're going to get there.

We need a pattern language not just for the architecture of the code or the servers that make it possible, but also for the actual design of end user applications. The Lightning Platform empowered business analysts and citizen developers to build applications for their organizations. Many of these are powerful, and many of them would benefit from using peer vetted patterns that can allow them to more quickly and easily create an impactful application experience.

By assembling (well thought out and documented) patterns from sections such as:

* data model (custom objects and fields)
* automation and business logic
* application experience and navigation
* layout and action

and using the rapid application development platform that is the Lightning Platform, a business user and a cictizen developer should be able to work together in tandem to prototype, validate, and quickly implement an application.

Over the years, I've been writing about patterns for [frosting][1], [reporting][2], [data modeling][4], and even for [what parts of Salesforce to build your app on, and what to avoid][3]. As I see more and more admins and developers attempt to transition their orgs and business processes to the Lightning Experience, I see a need for a more holistic pattern language for Lightning Application design. A year ago, at Tahoe Dreamin' I presented on a number of patterns that were available then, about two years into the Lightning Experience. With another year's worth of building, learning, and improvements to the platform, I think its time to start collecting those patterns into a language.

[1]: http://cdcarter.github.io/admin/2015/11/12/frosting
[2]: https://biggerboatconsulting.com/salesforce-exception-reporting-admins-best-friend/
[3]: https://biggerboatconsulting.com/service-cloud-and-cases-for-nonprofits/
[4]: https://twitter.com/bigthinkscloud
