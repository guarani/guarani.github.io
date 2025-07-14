---
layout: page
title: Blog
permalink: /blog/
description: "iOS development blog by Paul Von Schrottky - Technical insights, Swift tutorials, remote work experiences, and mobile development best practices from 10+ years of iOS engineering."
keywords: "iOS Development Blog, Swift Tutorials, URLProtocol iOS, SwiftUI Best Practices, Remote iOS Development, Mobile Engineering Insights, iOS Technical Writing"
---

# iOS Development Blog

Thoughts, insights, and experiences from 10+ years of iOS development, remote work, and building apps for millions of users.

<div class="blog-posts">
  {% for post in site.posts %}
    <article class="post-preview">
      <h2><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></h2>
      <p class="post-meta">{{ post.date | date: "%B %d, %Y" }}</p>
      
      {% if post.excerpt %}
        <p class="post-excerpt">{{ post.excerpt | strip_html | truncatewords: 50 }}</p>
      {% endif %}
      
      <a href="{{ post.url | relative_url }}" class="read-more">Read more →</a>
    </article>
  {% endfor %}
</div>

{% if site.posts.size == 0 %}
<div class="no-posts">
  <p>Blog posts coming soon! I'll be sharing insights about:</p>
  <ul>
    <li>SwiftUI best practices from production apps</li>
    <li>Remote iOS development at scale</li>
    <li>StoreKit and subscription implementation</li>
    <li>Leading distributed mobile teams</li>
    <li>iOS security and performance optimization</li>
  </ul>
  <p>Follow me on <a href="https://twitter.com/{{ site.twitter_username }}">Twitter</a> for updates!</p>
</div>
{% endif %} 