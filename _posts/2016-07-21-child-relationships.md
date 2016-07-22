---
layout: post
title: "The F*** Is the Child Relationship"
description: ""
category: appbuilder
tags: [wayfinder, process-builder, visualforce, frosting]
---
{% include JB/setup %}

Let’s look at how the Wayfinder Foundation’s communications team uses Salesforce to track stories submitted by their constituents for the weekly e-newsletter. As clients work through their programs, they are encouraged to submit stories about what they’re working on, what’s going on in their neighborhood, or how Wayfinder impacted their life. This newsletter is send to all current and former participants - Wayfinder serves as a community for the shared experience of being a refugee in America, recent or established. 

Since Wayfinder clients are just Contact records in Salesforce, the data model here is pretty simple: 

<img src="http://i.imgur.com/j0mRBX3.png" width="70%"/>

### The Parent Relationship

As an App Builder, there’s no doubt that you’re familiar with the named parent relationship. No? I bet you are!

A simple `Story__c` custom object holds the story, and it has a lookup relationship to the contact that submitted it. The lookup field has the API name `Contact__c`, which means there is a **named parent relationship **called `Contact__r`. Wayfinder uses the built in mail merge button to export the stories to their CMS for publishing, which means the Wayfinder admin had to build a formula on the Story object to pull over the Contact’s name for publication. That field looks like this:

```
Contact__r.FirstName & " “ & Contact__r.LastName
```

Yep, that’s the named parent relationship, you use it every time you pull a field from a parent object into a field update, a custom button, a process builder criteria, I could go on and on…

### So what’s the Child Relationship?

Well, when the Wayfinder App Builder was creating the lookup relationship, she had to set a child relationship name, so maybe that gives us a hint:

<img src="http://i.imgur.com/ZZpOlqD.png" width="70%"/>

The child relationship name is, by default, the plural name for the object. So, Story has a parent relationship named `Contact__r`, and Contact has a child relationship named `Stories__r`. You can find all of an object’s child relationships by describing it in [Workbench](https://workbench.developerforce.com).

<img src="http://i.imgur.com/3NKRMJC.png"/>

## So what do I do with it?

The parent relationship gets used **everywhere**, but I’m writing a post about the child relationship. That must means it gets used **somewhere**, right? Let’s dig into how an App Builder uses the child relationship.

### Process Builder

For a long time, the child relationship was relegated to users of SOQL and Apex doing complicated sub-selects. This meant it didn’t have a lot of visibility, and there are orgs out there that ended up with child relationships named `Contacts_4` instead of anything meaningful. But with the advent of the Lightning Process Builder, admins have access to the child relationship!

At Wayfinder, the communications department made a normal request: when a contact leaves the program and is flagged "no re-entry" by the programs staff, any stories that haven’t been published should be pulled from the publication pool. The admin built a quick process to manage this. When you create an “Update Records” action, you get asked to select the record to update, and then, finally, a list of child relationships!

<img src="http://i.imgur.com/SDP6Ter.png"/>

The process then updates the status on all unpublished stories to "Not for publication" - and the communications director at Wayfinder loves Salesforce even a little bit more now.

### Visualforce

As an App Builder, it can be helpful to know a little bit of HTML and a little bit of Visualforce. No Apex needed, you can make pretty nice looking page for a Force.com Public Site to allow clients to submit and review their stories. I like to call this public flow a Flowsite. Shouts out to [Beth Breisnes](http://twitter.com/bethbrains) who designed the original Flowsite that inspired this post!

Using a flow, it’s easy for clients to submit new stories, and you even built it to allow the client to edit a story if a story ID is passed in. This works well, but there’s no good way to show clients a list of their submitted stories and their status, so that they can select the story they want to edit! 

[Here’s the code for the page now](https://gist.github.com/cdcarter/e318fa62a5057cd61e731f75838cf748/3bac88246d43916c55549cd2cf6cf6d7efd3975b), and here’s what it looks like:

<img src="http://i.imgur.com/dcz9nn8.png" width="70%"/>

The communications department wants another section on this page, where a client can see a grid of their stories, the status, and an edit link if the story is in the "Submitted" stage. We could do something like this in a flow with dynamic choices, but it’s not particularly usable… Instead, the admin at Wayfinder decided to flex her Visualforce muscles!

When using the standard controller, Visualforce allows us to access child relationships with almost no effort.

The full code is [available here](https://gist.github.com/cdcarter/e318fa62a5057cd61e731f75838cf748/d7de501d1d40bbdeffb7c3d8702adfd7fb42abf0), but the key section is here:

```
<apex:dataTable styleClass="table" value="{!Contact.Stories__r}" var="story">
  <apex:column value="{!story.Name}" headerValue="Submission Number"/>
  <apex:column value="{!story.Title__c}" headerValue="Title"/>
  <apex:column value="{!story.Status__c}" headerValue="Status"/>
  <apex:column rendered="{!NOT(story.Status__c == 'Published')}" >

    <a href="{!URLFOR(...)}">Edit</a>

  </apex:column>
</apex:dataTable>
```

That’s right, we just declare that we want a table made from `Stories__r`, and then declare what each of the columns should look like. The syntax around `URLFOR()` is a little...obtuse (so I excluded it from the listing), but the rest of this should make some sense!

When it’s done, what do you got?

<img src="http://i.imgur.com/Q5uXT3S.png" width="70%"/>

That’s right. Throw that in a Force.com Public Site and Wayfinder’s clients can be given their individual links to submit and manage their stories!

### The API

Isn’t that enough? One quick last thing! When Wayfinder was working with a web developer to build out their new content site, they wanted to build client profiles pages for certain clients who are very involved with Wayfinder. 

The web developer didn’t know much about the Salesforce API, so the Wayfinder team pointed him to a clever trick. You can get at child relationships through [friendly URLs](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_relationship_traversal.htm), which made it easier than ever for the dev to get a list of published stories to present on the website too!

## Whew.

There’s a ton you can do with child relationships, and a lot of reasons for you to know about them. When you’re creating your lookup fields, don’t overlook the importance of a meaningful child relationship name.

Want to look through ALL the code in this blog post? You can install the Story object and our Visualforce and Flow [with this unmanaged package.](https://login.salesforce.com/packaging/installPackage.apexp?p0=04t36000000rdkP) Try it out in a sandbox or developer org, and have fun!

### Three Second Quiz


### About the Author
[Christian Carter](https://twitter.com/cdcarter), a 10X Certified Salesforce MVP and consultant at Bigger Boat Consulting, works exclusively with non-profit customers. Working on implementations for affordable housing, supportive services, and other world-changing non-profits, Christian has developed solutions starring point-and-click sensibility, outcomes focused data architecture, and human centric design at the heart of it all. He's a leading community contributor to the Nonprofit Starter Pack and has also developed his own open source managed packages on the Salesforce platform. With Beth Breisnes, he runs [Big Thinks](https://bigthinks.cloud), a Salesforce idea incubator. 

Shouts out to [Beth Breisnes](https://twitter.com/bethbrains), [Chris Peterson](https://twitter.com/ca_peterson), [Shonnah Hughes](https://twitter.com/saasy_sistah), and [@Gregory Brown](https://twitter.com/practicingdev) who all gave fantastic feedback on this article.
