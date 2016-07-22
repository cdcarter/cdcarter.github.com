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

A simple `Story__c` custom object holds the story, and it has a lookup relationship to the contact that submitted it. The lookup field has the API name `Contact__c`, which means there is a **named parent relationship** called `Contact__r`. Wayfinder uses the built in mail merge button to export the stories to their CMS for publishing, which means the Wayfinder admin had to build a formula on the Story object to pull over the Contact’s name for publication. That field looks like this:

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
<div class='quiz-container'></div><script type='text/javascript'>window.jQuery || document.write("<script src='//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js'><\/script>");</script><script type='text/javascript'>var input = [{"description":"Donec mauris erat, iaculis faucibus molestie eu, convallis non nibh. Etiam non dui quis massa auctor dapibus in vel libero. Etiam fermentum eros ante, ac dapibus erat ultrices eget. Sed dictum arcu ac risus semper, ac iaculis sem dignissim. Suspendisse eget tortor sapien. Suspendisse potenti. Ut ornare lectus nisi, at ornare eros accumsan et. Cras sed nunc congue, bibendum massa sit amet, varius dui. Proin dolor tortor, elementum quis mauris vel, vehicula tempor augue.","question":"Which country won their first World Cup game in 2014?","a":"Slovakia","b":"Ghana","c":"Bosnia and Herzegovina","d":"Cote d'Ivoire","answer":"Bosnia and Herzegovina","incorrect":"0053006c006f00760061006b0069006100200066006900720073007400200070006c006100790065006400200069006e002000740068006500200057006f0072006c0064002000430075007000200069006e00200032003000310030002c0020004700680061006e006100200061006e006400200043006f0074006500200064002700490076006f00690072006500200069006e00200032003000300036002e","correct":"0042006f0073006e0069006100200061006e00640020004800650072007a00650067006f00760069006e00610020003c006100200068007200650066003d00220068007400740070003a002f002f007700770077002e0074006800650067007500610072006400690061006e002e0063006f006d002f0066006f006f007400620061006c006c002f0032003000310034002f006a0075006e002f00320035002f0062006f0073006e00690061002d006800650072007a00650067006f00760069006e0061002d006900720061006e002d0077006f0072006c0064002d006300750070002d00670072006f00750070002d0066002d006d0061007400630068002d007200650070006f0072007400220020007400610072006700650074003d0022005f0062006c0061006e006b0022003e0077006f006e00200074006800650069007200200066006900720073007400200057006f0072006c006400200043007500700020006d0061007400630068003c002f0061003e00200061006700610069006e007300740020004900720061006e00200033002d0031002e","hint":"There is only one country that is a first-timer in the 2014 World Cup.","rowNumber":1},{"description":"","question":"How many editions of the World Cup have been won by the host team?","a":"6","b":"13","c":"8","d":"5","answer":"6","incorrect":"0055007200750067007500610079002c0020004900740061006c0079002c00200045006e0067006c0061006e0064002c0020004700650072006d0061006e0079002c00200041007200670065006e00740069006e006100200061006e00640020004600720061006e0063006500200061007200650020007400680065002000730069007800200068006f007300740020007400650061006d007300200074006f0020006800610076006500200077006f006e002000740068006500200057006f0072006c00640020004300750070002e","correct":"0055007200750067007500610079002c0020004900740061006c0079002c00200045006e0067006c0061006e0064002c0020004700650072006d0061006e0079002c00200041007200670065006e00740069006e006100200061006e00640020004600720061006e0063006500200061007200650020007400680065002000730069007800200068006f007300740020007400650061006d007300200074006f0020006800610076006500200077006f006e002000740068006500200057006f0072006c00640020004300750070002e","hint":"19 editions of the World Cup have been hosted in total.","rowNumber":2},{"description":"Suspendisse potenti. Ut ornare lectus nisi, at ornare eros accumsan et. Cras sed nunc congue, bibendum massa sit amet, varius dui. Proin dolor tortor, elementum quis mauris vel, vehicula tempor augue.","question":"Who is the World Cup's all-time leading scorer?","a":"Klose","b":"Kocsis","c":"Fontaine","d":"Ronaldo","answer":"Klose","incorrect":"0046006f006e007400610069006e006500200068006f006c0064007300200074006800650020007200650063006f0072006400200066006f00720020006d006f0073007400200067006f0061006c007300200069006e00200061002000730069006e0067006c006500200074006f00750072006e0061006d0065006e0074002e0020004b006f00630073006900730020006800610073002000740068006500200074006f00750072006e0061006d0065006e00740027007300200062006500730074002000730063006f00720069006e006700200061007600650072006100670065002e0020004b006c006f007300650020006a0075007300740020006200650061007400200052006f006e0061006c0064006f002700730020007200650063006f0072006400200069006e0020007400680065002000730065006d006900660069006e0061006c002000670061006d006500200061006700610069006e007300740020004200720061007a0069006c0020006f006e0020004a0075006c007900200038002c00200032003000310034002e","correct":"0052006f006e0061006c0064006f002000770061007300200074006800650020006c0065006100640069006e0067002000730063006f0072006500720020007700690074006800200031003500200067006f0061006c007300200075006e00740069006c0020004b006c006f007300650020006200650061007400200068006900730020007200650063006f0072006400200069006e0020004200720061007a0069006c00200032003000310034002e","hint":"Brazil fans are very upset.","rowNumber":3}]; var pubStylesheet = 'http://assets.sbnation.com.s3.amazonaws.com/features/quiz-generator/quiz-vox.css'; var pub = 'vox'; </script><script src='http://assets.sbnation.com.s3.amazonaws.com/features/quiz-generator/quiz.js'></script>

### About the Author
[Christian Carter](https://twitter.com/cdcarter), a 10X Certified Salesforce MVP and consultant at Bigger Boat Consulting, works exclusively with non-profit customers. Working on implementations for affordable housing, supportive services, and other world-changing non-profits, Christian has developed solutions starring point-and-click sensibility, outcomes focused data architecture, and human centric design at the heart of it all. He's a leading community contributor to the Nonprofit Starter Pack and has also developed his own open source managed packages on the Salesforce platform. With Beth Breisnes, he runs [Big Thinks](https://bigthinks.cloud), a Salesforce idea incubator. 

Shouts out to [Beth Breisnes](https://twitter.com/bethbrains), [Chris Peterson](https://twitter.com/ca_peterson), [Shonnah Hughes](https://twitter.com/saasy_sistah), and [@Gregory Brown](https://twitter.com/practicingdev) who all gave fantastic feedback on this article.
