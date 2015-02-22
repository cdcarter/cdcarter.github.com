---
layout: page
title: Salesforce for Humanists
tagline: Computers were made by humans to make humanity better
---
{% include JB/setup %}

Salesforce for Humanists provides resources and information for non-profit Salesforce administrators and developers.
    
## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

