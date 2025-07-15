---
layout: home
description: "Senior iOS Engineer with 10+ years experience building apps for millions. Previously at Automattic (Day One) & WordPress Mobile. Swift, SwiftUI expert available for US remote work."
keywords: "Senior iOS Engineer, Remote iOS Developer, Swift Developer, SwiftUI Expert, Automattic, Day One, WordPress Mobile"
---

<img src="{{ site.profile_image }}" alt="{{ site.author }}" style="border-radius: 50%; width: 150px; height: 150px; margin: 20px auto; display: block;">

# {{ site.author }}
## Senior iOS Engineer

**Swift & SwiftUI specialist with 10+ years shipping iOS apps**

Previously: **Day One at Automattic** & **WordPress Mobile**

<div class="cta-buttons">
  <a href="https://calendly.com/pschrottky" target="_blank">Schedule Call</a>
  <a href="/assets/Paul%20Von%20Schrottky%20-%20Resume.pdf" target="_blank">Download Resume</a>
  <a href="mailto:{{ site.email }}">Email Me</a>
</div>

---

## Featured Work

{% for project in site.projects %}
{% if project.featured %}
<div class="featured-project">
  {% if project.icon %}<img src="{{ project.icon }}" alt="{{ project.title }}">{% endif %}
  <div>
    <strong><a href="{{ project.url }}">{{ project.title }}</a></strong> - {{ project.subtitle }}<br>
    {{ project.role }} | {{ project.technologies[0] }}, {{ project.technologies[1] }}{% if project.technologies[2] %}, {{ project.technologies[2] }}{% endif %}
  </div>
</div>
{% endif %}
{% endfor %}

[View All Projects](/projects)

---

## Core Technologies

**iOS:** Swift, SwiftUI, UIKit, Combine, StoreKit  
**Architecture:** MVVM, Clean Architecture, Unit Testing  
**Tools:** Xcode, Git, Fastlane, CI/CD

---

## Available for Remote Work

US-based LLC • No visa sponsorship needed • 10+ years distributed team experience

**Ready to discuss your iOS project?**

<div class="cta-buttons">
  <a href="https://calendly.com/pschrottky" target="_blank">Schedule Call</a>
  <a href="mailto:{{ site.email }}">Email Me</a>
</div>
