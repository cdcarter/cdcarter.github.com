---
layout: post
title: "Black Beans and FOR VIEW"
description: ""
category: dev
tags: [soql, cooking]
---
{% include JB/setup %}

I don't eat, sleep, and breathe Salesforce. In fact, I'd much rather be cooking than coding most of the time, problem is, I'm not very good at it. I spent a good portion of today practicing my knife skills, watching yooutube videos and decimating a bag of potatoes. 

I have a pot of black beans on, which is a LONG process at low temperature. I use [this recipe](http://www.seriouseats.com/recipes/2014/09/the-lazy-cooks-black-beans-easy-recipe.html) from Serious Eats, because it's easy, you don't have to soak the beans over night, and it gives me a bunch of time to try other things. Like chopping potatoes.

![Beans!](http://www.seriouseats.com/recipes/assets_c/2014/08/20140827-black-beans-vicky-wasik-4-thumb-625xauto-410002.jpg)

But I wasn't just chopping potatoes today, I was also thinking about Salesforce, thinking about SOQL. [Kieren Jameson](http://womencodeheroes.com/), who I had the total honor of being recognized alongside for [July Hub Heroes](http://www.salesforcefoundation.org/hub-heroes-july-2015/), wrote an absolutely _stunning_ introduction to SOQL a few months ago called [Cooking with Code](http://womencodeheroes.com/2015/04/cooking-with-code-a-sweet-intro-to-soql-part-one/) and I highly recommend it.

As I was doing my own cooking and coding, I remembered a rarely mentioned but super great feature of SOQL that I wish more people would talk about, so I'm going to share it with you. 

# Recent Items?

You know that Recent Items sidebar...

![Recent Items](https://dl.dropboxusercontent.com/spa/q8pc7mthv83x9i1/2015-07-19-15h10m/images/docs/untitled/contacts-~-salesforcecom---developer-edition.png)

And the autocompletes in global search...

![Global Search](https://dl.dropboxusercontent.com/spa/q8pc7mthv83x9i1/2015-07-19-15h10m/images/docs/untitled/contacts-~-salesforcecom---developer-edition-1.png)

Ever wondered how items get there? Ever wondered how YOUR objects can go there?

If your object has a Tab created with the Tab wizard, and you're using the standard detail page, you'll notice that your object is automatically showing up in both places when you look at a record. Even if you've overrode the detail page with a Visualforce page of your own choosing, because you're using the standard controller, your object is still going to show up there.

But what about when you are using a custom controller? Deep in the throes of custom UI, or just building a lightning component (we'll get a standard lightning controller someday, I'm sure of it!) you have resorted to a custom controller and now your object isn't showing up in the sidebar anymore. You can try it yourself:

#### ContactViewer.vfp

	<apex:page Controller="ContactViewerController">
    	<h1>
        	This is {!c.Name}
    	</h1>
	</apex:page>

#### ContactViewerController.apxc

	public class ContactViewerController {
	    public Contact c {get;set;}

	    public ContactLookerController() {
	        c = [SELECT Name,Id 
	        	 FROM Contact 
	        	 WHERE Id =: ApexPages.currentPage().getParameters().get('id')];
	    }
	}

Now got to `http://{your-instance}.salesforce.com/apex/ContactViewer?id={contactId}`. Is your item in the sidebar? NOPE. Ugh.

That's okay.

# Let's use `FOR VIEW`

Let's make a slight change to our controller. After the WHERE clause in our query, let's add `FOR VIEW`, like this:

#### ContactViewerController.apxc

	public class ContactViewerController {
	    public Contact c {get;set;}

	    public ContactLookerController() {
	        c = [SELECT Name,Id 
	        	 FROM Contact 
	        	 WHERE Id =: ApexPages.currentPage().getParameters().get('id')
	        	 FOR VIEW];
	    }
	}

Now give your page a try...

BLAM, the contact shows up in the Recently Viewed Items sidebar! How sweet was that?

Learn more about `FOR VIEW` in the [SOQL Docs](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_select_for_view.htm) and don't lose this opportunity to check out it's best friend, [`FOR REFERENCE`](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_select_for_reference.htm), to be used when creating custom related lists.