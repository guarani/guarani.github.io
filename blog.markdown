---
layout: page
title: Blog
permalink: /blog/
description: "iOS development blog by Paul Von Schrottky - Technical insights, Swift tutorials, remote work experiences, and mobile development best practices from 12+ years of iOS engineering."
keywords: "iOS Development Blog, Swift Tutorials, URLProtocol iOS, SwiftUI Best Practices, Remote iOS Development, Mobile Engineering Insights, iOS Technical Writing"
---

## Writing on iOS Engineering

<div class="grid grid--cards">
  {% for post in site.posts %}
    <article class="card post-card">
      <a class="card__body" href="{{ post.url | relative_url }}">
        <h3 class="card__title">{{ post.title | escape }}</h3>
        <p class="card__meta">{{ post.date | date: "%B %d, %Y" }}</p>
        {% if post.excerpt %}
          <p class="card__desc">{{ post.excerpt | strip_html | truncatewords: 36 }}</p>
        {% endif %}
      </a>
    </article>
  {% endfor %}
</div>

{% if site.posts.size == 0 %}
  <p>No posts yet. Follow on <a href="https://twitter.com/{{ site.twitter_username }}">Twitter</a> for updates.</p>
{% endif %}