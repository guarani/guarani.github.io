---
layout: page
title: Projects
permalink: /projects/
---

# My iOS Projects

Here are some of the key projects I've worked on throughout my career as a Senior iOS Engineer.

<div class="projects-grid">
  {% for project in site.data.projects %}
    <div class="project-card">
             <div class="project-header">
         {% if project.icon %}
           <img src="{{ project.icon }}" alt="{{ project.title }}" class="project-icon">
         {% endif %}
         <div class="project-title-area">
           <h3><a href="/projects/{{ project.id }}/">{{ project.title }}</a></h3>
           <p class="project-subtitle">{{ project.subtitle }}</p>
         </div>
       </div>
      
      <p class="project-description">{{ project.description }}</p>
      
      <div class="project-details">
        <p><strong>Role:</strong> {{ project.role }}</p>
        {% if project.impact %}
          <p><strong>Impact:</strong> {{ project.impact }}</p>
        {% endif %}
      </div>
      
      <div class="project-technologies">
        {% for tech in project.technologies %}
          <span class="tech-tag">{{ tech }}</span>
        {% endfor %}
      </div>
      
      <ul class="project-highlights">
        {% for highlight in project.highlights %}
          <li>{{ highlight }}</li>
        {% endfor %}
      </ul>
      
      <div class="project-links">
        {% if project.app_store_url != "" %}
          <a href="{{ project.app_store_url }}" class="btn btn-primary" target="_blank">App Store</a>
        {% endif %}
        {% if project.github_url != "" %}
          <a href="{{ project.github_url }}" class="btn btn-secondary" target="_blank">GitHub</a>
        {% endif %}
      </div>
    </div>
  {% endfor %}
</div>

---

## Interested in Working Together?

I'm always open to discussing new iOS development opportunities. Whether you need help with:

- **SwiftUI/UIKit Development**
- **App Store Optimization**
- **StoreKit Integration**
- **Mobile Architecture**
- **Team Leadership**

Feel free to [get in touch](mailto:{{ site.email }}) to discuss your project needs. 