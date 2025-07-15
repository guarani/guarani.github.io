---
layout: page
title: Projects
permalink: /projects/
description: "iOS projects portfolio by Paul Von Schrottky - Day One at Automattic, WordPress Mobile, Banking apps, and more. SwiftUI and iOS development expertise."
---

# My iOS Projects

Key projects I've worked on:

{% for project in site.projects %}
<div class="featured-project">
  {% if project.icon %}<img src="{{ project.icon }}" alt="{{ project.title }}">{% endif %}
  <div>
    <strong><a href="{{ project.url }}">{{ project.title }}</a></strong> - {{ project.subtitle }}<br>
    {{ project.role }} | {{ project.technologies | join: ", " }}
  </div>
</div>
{% endfor %}

---

**Interested in working together?** I'm available for iOS development projects involving SwiftUI, StoreKit, mobile architecture, and team leadership.

[Get in touch](mailto:{{ site.email }}) to discuss your project needs. 