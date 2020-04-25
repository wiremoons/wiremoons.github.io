---
title : "Wiremoons Web Site"
description : "Home page for www.wiremoons.com"
markup : "md"
---
# Wiremoons Web Site

### Welcome to my website!

This is my website containing a general blog, and providing a jump point for other wiremoons content on the Internet. Please see the [About](/about.html) page for other sites I sometime post content too as well.


## Blog Posts
Below is a selection of blog posts I have made on various topics which maybe of interest:

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
