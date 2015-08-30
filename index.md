---
layout: page
title: Salesforce for Humanists
tagline: Computers were made by humans to make humanity better
---
{% include JB/setup %}

Salesforce for Humanists provides resources and information for non-profit Salesforce administrators and developers.

## Who am I?
Christian Carter is a certified Salesforce Administrator, App Builder, and Platform Developer who has deep domain knowledge in technology implementations for arts and culture organizations. From donor data to ticketing systems to custom apps for your organizational success, we can grow together to make Salesforce work for you and for you to learn from your data. There's incredible power in having a single database to track a 360 degree view of your patrons, and cloud based databases can provide that for you.

With a background as a ticketing and front-of-house professional, I've worked at many organizations with a single-patron model. I see the value in collecting and reporting on accurate data on all facet of your patron base, and I also have a strong operational brain, helping design a user experience for both your patrons and your staff to get the most benefits from the data you have.

I now work at [Bigger Boat Consulting](http://biggerboatconsulting.com/) as a Salesforce Consultant and Developer for non-profits. I am very active in both the Power of Us Hub and the Patron Manager Client Community. I'm a speaker at the 2015 Patron Manager Commuity Meeting and Dreamforce 2015 in the Foundation Theatre about using and optimizing the Salesforce1 Mobile App.

You can see my open-source contributions on my [projects page](/projects/).
    
## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

