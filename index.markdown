---
layout: home
---

<div class="hero-section">
  <div class="hero-content">
    <img src="{{ site.profile_image }}" alt="{{ site.author }}" class="profile-image">
    
    <h1><strong>{{ site.author }}</strong></h1>
    <h2 class="hero-subtitle">Senior iOS Engineer</h2>
    <p class="hero-tagline"><strong>Swift, SwiftUI | Remote-First | Available via US-based LLC</strong></p>
    
    <p class="hero-description">
      Previously: <strong>Day One at Automattic</strong> & <strong>WordPress Mobile</strong><br>
      10+ years building iOS apps for millions of users
    </p>
    
    <div class="hero-ctas">
      <a href="mailto:{{ site.email }}" class="btn btn-primary">Contact Me</a>
      <a href="/about" class="btn btn-secondary">Learn More</a>
    </div>
    
    <div class="social-links">
      <a href="https://github.com/{{ site.github_username }}" title="GitHub">GitHub</a>
      <a href="https://twitter.com/{{ site.twitter_username }}" title="Twitter">Twitter</a>
      <a href="https://linkedin.com/in/{{ site.linkedin_username }}" title="LinkedIn">LinkedIn</a>
      <a href="https://stackoverflow.com/{{ site.stackoverflow }}" title="Stack Overflow">Stack Overflow</a>
    </div>
  </div>
</div>

## Featured Projects

<div class="projects-grid">
  {% for project in site.data.projects %}
    {% if project.featured %}
    <div class="project-card">
      <div class="project-header">
        {% if project.icon %}
          <img src="{{ project.icon }}" alt="{{ project.title }}" class="project-icon">
        {% endif %}
        <div class="project-title-area">
          <h3>{{ project.title }}</h3>
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
    {% endif %}
  {% endfor %}
</div>

## Latest Blog Posts
