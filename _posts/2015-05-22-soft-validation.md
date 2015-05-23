---
layout: post
title: "Soft Validation Rules"
description: ""
category: admin 
tags: [admin]
---
{% include JB/setup %}

Sometimes you want a &quot;soft&quot; validation rule. That is -- if a field doesn't match certain conditions, you want to just issue a warning, but still allow the record to be saved and the user to move on with their life. 

Remember the Campsite app from <a href="http://developer.salesforce.com/trailhead" target="">Trailhead</a>? Let's say that we also want to record the size of each campsite, so that potential campers can determine if the site is in line with what they have. While the rangers are out in the field, measuring each campsite to see which size they are, we want our admins back at the lodge to not be hindered by a new required field. 

## A Warning Field!

To solve this, we'll use a standard custom field. We'll just use it in an interesting way. Here's what our page layout will look like for our administrators in the lodge. When that square foor field isn't filled, we show a simple warning on the page.

![Warning Field](http://i.imgur.com/4GpDupF.png)

## Our schema

We'll assume you've already created a custom object called Campsite__c with a long text area for Description. First up, add a Number field called Square_feet__c to contain the size of the site! Make this field visible to all profiles, and add it to the Information section of your page layout.

Now, create a formula field of type Text called Size_warning__c. Here's the formula we're using:

<pre><code>IF( 
 &nbsp;OR(ISNULL( Square_Feet__c ),(Square_Feet__c == 0)), 
 &nbsp;IMAGE(&quot;/img/samples/flag_yellow.gif&quot; , 'Yellow' ,12,12) &amp; ' It is very helpful to other campers to list the size of the campsite.' &amp; BR(), 
 &nbsp;NULL 
)</code></pre>

This way, if Square_Feet__c is null or zeroed, it will display a standard Yellow Flag image, with our message about how useful it is to fill in the size when we know it!

## Field Level Security

On this field, we'll want to make it ONLY visible to administrators, that way your users aren't bothered by it. You might have some profiles that you want to see the warning, and some who can safely live without it.

![FLS](http://i.imgur.com/7oPtJGg.png)

## Edit the page layout

Our last step to making this error useable is to edit the page layout. First, create a new section ABOVE &quot;Information&quot; called warnings. We want to set this section to never show it's headers. This way, it will appear at the very top of the record, and if the profile can't see the warning message, they can't see the section at all.</p>

Drag your Size Warning field into that section, save your layout, and you're all set!

![Page Layout](http://i.imgur.com/x9PA71r.png)

## It just works...

The field relies on the first section of our formula to decide whether or not to show the warning flag. You can set that condition to anything a formula might be able to do. For our administrators, the yellow flag will fly when the conditions are met, and users who haven't been shown the field (like our campers) won't know any other interface!

![Normal View](http://i.imgur.com/t4luhaz.png)

We can use soft validation rules for these "sort of" required fields, to show users discrepancies between related objects, to tip off an admin about possibly untrustworthy data, and more.

