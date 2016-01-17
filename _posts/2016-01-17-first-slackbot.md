---
layout: post
title: "Your First Slackbot, in Apex!"
description: "Using SlackSFDCLib to build a Slack slash command"
category: developer
tags: [slack]
---
{% include JB/setup %}

Does your team use Slack for team communication? Ours sure does, and it's probably the most loved tool in the office. Without a doubt, one of the best parts of Slack is how easily it integrates with other tools. We've got it connected to Trello, Github, Google Hangouts, and more. And I finally got to thinking, maybe we can integrate it with our internal Salesforce based time tracking tool! What other Salesforce data could we integrate with Slack? 

So, I got to writing a framework to write a simple Slackbot! In this post, we'll look at a command that gives you statistics about recent donations. You can use this code as a jumping off point to write slash-commands for any sort of data in Salesforce, from updating case statuses to logging project updates.

Slack offers a bunch of different ways to integrate your tools, but we're going to look at the `slash-command` method. In this integration, you define a command that starts with a slash, like `/donation-stats` and give Slack a URL to call when that command is used, with a bunch of metadata like who said it and in what channel. We'll use Pat Patterson's technique for a [public Apex REST API](https://developer.salesforce.com/blogs/developer-relations/2012/02/quick-tip-public-restful-web-services-on-force-com-sites.html) to provide an endpoint for our Slack integration to hit.

I've created a framework you can use to start building your integration, [SlackSFDClib](https://github.com/cdcarter/SlackSFDCLib), which will provide us with some simple wrappers over the Slack request and responses to make it easier to assemble things. Go ahead and install the unmanaged package (or just paste the `SlackContext` and `SlackContext_TEST` classes into your org) and then let's get started.

We're going to create a class called `SlackBot` to handle our Slack requests. SlackSFDCLib comes with a basic class to help us out:

<pre>
@restResource(urlMapping='/slackbot/*')
global class SlackBot {
	static final string SLACK_TOKEN='PASTE_TOKEN_HERE';

	@httpPost
	global static void doPost() {
		SlackContext ctx = new SlackContext(RestContext.request);				
		ctx.checkToken(SLACK_TOKEN);
		ctx.response.response_type='ephemeral';

		// DO WORK HERE
		ctx.response.text = 'Thanks for calling Salesforce!';

		ctx.setResponse(RestContext.response);
	}
}
</pre>

Now, if you're a non-profit, you may want to be able to ask Slack for updates on your current donations. 

<img src="http://i.imgur.com/bCQBxBc.png" width="70%"/>

In that case, we'd replace the "DO WORK HERE" block with:

<pre>
String count = String.valueOf(([SELECT Count(Id) FROM Opportunity WHERE CreatedDate = TODAY][0]).get('expr0'));
String base = '{0} new donations today!';
ctx.response.text = String.format(base, new List&lt;String&gt; {count});
</pre>

Once your class is in working shape, follow Pat's instructions on adding the class to your Force.com site (and make sure the site guest user has permissions on all the objects and fields you are using!)

Now, we need to head over to Slack to set up an integration. We're going to add the "Slash Command" integration, and configure a new command called `/donations`.

<img src="http://i.imgur.com/rS7UxoD.png" width="70%"/>

To configure it, simply insert the URL to your public site's REST API and give your bot a name. You can also add help text and upload a bot icon here!

<img src="http://i.imgur.com/IJazdw6.png" width="70%"/>

Next, grab that token and paste it in to your `SlackBot` class in the `SLACK_TOKEN` definition.

Head on over to Slack and give your new command a try!

`SlackSFDClib` also provides wrappers around the Slack Attachment object. With that, it becomes trivial to create beautiful responses.

<img src="http://i.imgur.com/ABlST7H.png" width="70%"/>

Here, we created an attachment with a `title`, some `text`, a `color` and four `SlackFields`!

To make things even cooler, your `SlackContext` object contains a `SlackRequest` that you can use to find out what user or channel the command was called in.

In my test org, I created a new external ID field named `Slack_Channel__c` on the Campaign SObject. When our slash command gets called, it can search for the Campaign that is related to the channel the command was invoked in:

<pre>
Campaign c = [SELECT Id,Name,NumberOfContacts,NumberOfWonOpportunities,AmountWonOpportunities,Amount_Remaining__c 
              FROM Campaign WHERE Slack_Channel__c =: ctx.request.channel_name];
</pre>

Here's our final class, all together:

<pre>
global class SlackBot {
    static final string SLACK_TOKEN='PASTE_TOKEN_HERE';
    
    @httpPost
    global static void doPost() {
        SlackContext ctx = new SlackContext(RestContext.request);
        ctx.checkToken(SLACK_TOKEN);
        ctx.response.response_type='ephemeral';
        
        Campaign c = [SELECT Id,Name,NumberOfContacts,NumberOfWonOpportunities,AmountWonOpportunities,Amount_Remaining__c 
                      FROM Campaign WHERE Slack_Channel__c =: ctx.request.channel_name];
        
        SlackContext.SlackAttachment a = new SlackContext.SlackAttachment();
        a.title = 'Annual Campaign Statistics';
        a.text = 'Here\'s how the campaign looks, boss!';
        a.color = '#439FE0';
        a.fields.add(new SlackContext.SlackField('Donors',String.valueOf(c.NumberOfContacts)));
        a.fields.add(new SlackContext.SlackField('Donations',String.valueOf(c.NumberOfWonOpportunities)));
        a.fields.add(new SlackContext.SlackField('Amount Raised',String.valueOf(c.AmountWonOpportunities)));
        a.fields.add(new SlackContext.SlackField('Amount Remaining',String.valueOf(c.Amount_Remaining__c)));
        
        ctx.resp.attachments.add(a);
        
        ctx.setResponse(RestContext.response);
    }
}
</pre>

It's that easy to get up and running with slash commands for your team, so what will you build?
