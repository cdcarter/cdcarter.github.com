---
layout: post
category: appbuilder
title: 'Seeing value with Portion Control'
tags:
  - admin
  - frosting
  - formula
published: true
---

<meta name="twitter:card" content="summary" />
<meta name="twitter:site" content="@cdcarter" />
<meta name="twitter:title" content="Seeing value with Portion Control" />
<meta name="twitter:description" content="Because you can never have enough image tricks..." />
<meta name="twitter:image" content="s://portioncontrol.herokuapp.com/pie.png?size=250x250&performance=65&readabilty=15&scalability=20" />

{% include JB/setup %}

[Portion Control](https://github.com/salesforce-ux/portion-control) is a free open-source app from the Salesforce UX team. They describe it as a way to "quickly communicate project value", but **it** has more value than that!

<img src="https://portioncontrol.herokuapp.com/pie.png?size=250x250&performance=65&readabilty=15&scalability=20"/>

That's what a Portion Control chart looks like, and it's not hard to make:

    <img src="https://portioncontrol.herokuapp.com/pie.png?size=250x250&performance=65&readabilty=15&scalability=20"/>
    
Or, in a Salesforce formula field:

    IMAGE("https://portioncontrol.herokuapp.com/pie.png?size=250x250&performance=" & Performance__c & "&readability=" & Readibility__c & "&scalability=" & Scalability__c)
    
I've gone on record being excited about [Google Charts](https://cdcarter.github.io/admin/2015/11/12/frosting) and [Progress Bar](https://cdcarter.github.io/admin/2016/02/15/progress-bar) formula fields that help communicate data to your end users on the record home or list view. Here's one more tool in your toolbelt, thanks to Salesforce UX!
