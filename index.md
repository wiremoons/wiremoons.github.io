---
title : "Wiremoons Web Site"
description : "Home page for www.wiremoons.com"
markup : "md"
---
# Wiremoons Web Site

### Welcome to my website!

This is my website containing a general blog, and providing a jump point for other wiremoons content on the Internet. Please see the links in the section below for other sites I post content too as well.

In case you were wondering - '*wiremoons*' is an anagram for Simon Rowe - which is my name.

## Blog Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

## About the Site

My web site is hosted on GitHub and has a redirect from my main domain name [www.wiremoons.com](https://www.wiremoons.com/). The site content is managed using <a href="https://gohugo.io/">Hugo</a>, with version control through [Git](http://git-scm.com/)

## My Other Internet Sites

<table width="90%">
<tr><td><a href="https://www.wiremoons.com/">www.wiremoons.com</a></td><td>This site for general content</td></tr>
<tr><td><a href="https://www.flickr.com/photos/wiremoons/">Flickr</a></td><td>My Flickr account for photographs</td></tr>
<tr><td><a href="http://wiremoons.tumblr.com/">Tumblr</a></td><td>Other blog and more casual photos (not used much!)</td></tr>
<tr><td><a href="https://github.com/wiremoons">GitHub</a></td><td>My projects and development code on GitHub</td></tr>
<tr><td><a href="https://twitter.com/wiremoons">Twitter</a></td><td>My Twitter page @wiremoons</td></tr>
</table>
