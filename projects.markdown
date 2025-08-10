---
layout: page
title: Projects
permalink: /projects/
description: "iOS projects portfolio by Paul Von Schrottky - Day One at Automattic, WordPress Mobile, Banking apps, and more. SwiftUI and iOS development expertise."
---

## Selected iOS Projects

<div class="grid grid--cards">
{% for project in site.projects %}
  <article class="card project-card">
    <a href="{{ project.url }}" class="card__body">
      {% if project.icon %}
        <img class="project-card__icon" src="{{ project.icon }}" alt="{{ project.title }}">
      {% endif %}
      <h3 class="card__title">{{ project.title }}</h3>
      <p class="card__meta">{{ project.subtitle }}</p>
      {% if project.description %}
        <p class="card__desc">{{ project.description }}</p>
      {% endif %}
      {% if project.technologies %}
        <p class="card__tags">{{ project.technologies | join: " • " }}</p>
      {% endif %}
    </a>
  </article>
{% endfor %}
</div>

---

Interested in working together? I take on Swift/SwiftUI, StoreKit, and mobile architecture work.

<div class="cta-buttons">
  <a class="btn btn--primary" href="https://calendly.com/pschrottky" target="_blank" rel="noopener">Schedule Call</a>
  <a class="btn" href="mailto:{{ site.email }}">Email Me</a>
  <a class="btn btn--secondary" href="/assets/Paul%20Von%20Schrottky%20-%20Resume.pdf" target="_blank" rel="noopener">Resume</a>
</div>