---
layout: page
title: Christian Carter
tagline: Non-Profit Operations and Databases
---
{% include JB/setup %}

## Who am I?
I am a certified Salesforce Administrator, App Builder, and Platform Developer.

I now work at [Bigger Boat Consulting](http://biggerboatconsulting.com/) as a Salesforce Consultant and Developer for non-profits. I am very active in both the Power of Us Hub and the PatronManager Client Community. I was a speaker at the 2015 PatronManager Commuity Meeting talking about building Lightning Apps for mobile ticket management, and I spoke at Dreamforce 2015 in the Foundation Theatre, presenting on using and optimizing the Salesforce1 Mobile App.

You can see my open-source contributions on my [projects page](/projects/).
    
## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

