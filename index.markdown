---
layout: home
description: "Senior iOS Engineer with 10+ years experience building apps for millions. Previously at Automattic (Day One) & WordPress Mobile. Swift, SwiftUI expert available for US remote work. No visa sponsorship needed."
keywords: "Senior iOS Engineer, Remote iOS Developer, Swift Developer, SwiftUI Expert, Automattic, Day One, WordPress Mobile, Paraguay iOS Engineer, US Remote Work"
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
      <a href="https://calendly.com/pschrottky" class="btn btn-primary" target="_blank">Schedule Call</a>
      <a href="/assets/Paul%20Von%20Schrottky%20-%20Resume.pdf" class="btn btn-secondary" target="_blank">Download Resume</a>
      <a href="mailto:{{ site.email }}" class="btn btn-tertiary">Email Me</a>
    </div>

  </div>
</div>

## Featured Projects

<div class="projects-grid">
  {% for project in site.projects %}
    {% if project.featured %}
    <div class="project-card">
      <div class="project-header">
        {% if project.icon %}
          <img src="{{ project.icon }}" alt="{{ project.title }}" class="project-icon">
        {% endif %}
        <div class="project-title-area">
          <h3><a href="{{ project.url }}">{{ project.title }}</a></h3>
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

## Technical Expertise

<div class="skills-section">
  <div class="skills-grid">
    <div class="skill-category">
      <h3>🍎 iOS Development</h3>
      <div class="skills-list">
        <div class="skill-item">
          <span class="skill-name">Swift</span>
          <span class="skill-years">8+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">SwiftUI</span>
          <span class="skill-years">4+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">UIKit</span>
          <span class="skill-years">10+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">Combine</span>
          <span class="skill-years">3+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">StoreKit</span>
          <span class="skill-years">5+ years</span>
        </div>
      </div>
    </div>
    
    <div class="skill-category">
      <h3>🏗️ Architecture & Patterns</h3>
      <div class="skills-list">
        <div class="skill-item">
          <span class="skill-name">MVVM</span>
          <span class="skill-years">6+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">Clean Architecture</span>
          <span class="skill-years">4+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">Dependency Injection</span>
          <span class="skill-years">5+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">Unit Testing</span>
          <span class="skill-years">8+ years</span>
        </div>
      </div>
    </div>
    
    <div class="skill-category">
      <h3>🔧 Tools & Platforms</h3>
      <div class="skills-list">
        <div class="skill-item">
          <span class="skill-name">Xcode & Instruments</span>
          <span class="skill-years">10+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">Git & GitHub</span>
          <span class="skill-years">10+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">Sentry & Crashlytics</span>
          <span class="skill-years">6+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">Fastlane & CI/CD</span>
          <span class="skill-years">5+ years</span>
        </div>
      </div>
    </div>
    
    <div class="skill-category">
      <h3>🌐 Backend & APIs</h3>
      <div class="skills-list">
        <div class="skill-item">
          <span class="skill-name">RESTful APIs</span>
          <span class="skill-years">10+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">GraphQL</span>
          <span class="skill-years">3+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">WebSocket</span>
          <span class="skill-years">4+ years</span>
        </div>
        <div class="skill-item">
          <span class="skill-name">OAuth & Security</span>
          <span class="skill-years">6+ years</span>
        </div>
      </div>
    </div>
  </div>
</div>

## Community Recognition

<div class="community-section">
  <div class="community-grid">
    <div class="community-card">
      <h3>🏆 Stack Overflow Developer</h3>
      <p>Long-time contributor to the programming community with expertise in iOS development, Swift, and mobile architecture.</p>
      <div class="flair-container">
        <a href="https://stackoverflow.com/users/1305067/paulvs" target="_blank" rel="noopener">
          <img src="https://stackoverflow.com/users/flair/1305067.png" width="208" height="58" 
               alt="profile for paulvs at Stack Overflow, Q&amp;A for professional and enthusiast programmers" 
               title="profile for paulvs at Stack Overflow, Q&amp;A for professional and enthusiast programmers">
        </a>
      </div>
      <p class="contribution-description">Helping fellow developers solve complex iOS and Swift challenges</p>
    </div>
    
    <div class="community-card">
      <h3>📱 WordPress Mobile Leadership</h3>
      <p>Led strategic initiatives and community engagement for the WordPress Mobile platform serving millions of users worldwide.</p>
      <div class="wordpress-contributions">
        <div class="contribution-item">
          <h4><a href="https://make.wordpress.org/mobile/2023/04/11/contribute-to-the-future-of-the-wordpress-app/" target="_blank" rel="noopener">Contribute to the Future of the WordPress App</a></h4>
          <p>Authored comprehensive guide for community contributors after successfully refocusing the WordPress app on core features, enabling broader open source participation.</p>
        </div>
        <div class="contribution-item">
          <h4><a href="https://make.wordpress.org/mobile/2022/07/27/refocusing-the-wordpress-app-on-core-features/" target="_blank" rel="noopener">Refocusing the WordPress App on Core Features</a></h4>
          <p>Led strategic pivot removing Jetpack dependencies to streamline the WordPress mobile experience and increase community involvement.</p>
        </div>
      </div>
      <p class="contribution-description">Strategic leadership in mobile platform architecture and community building</p>
    </div>
    
    <div class="community-card">
      <h3>🌟 WordPress Core Contributor</h3>
      <p>Recognized contributor to WordPress Core across multiple major releases from version 5.5 to 6.3, helping power 40% of the web.</p>
      <div class="core-contribution">
        <div class="contribution-highlight">
          <span class="versions">WordPress 5.5 → 6.3</span>
          <span class="contribution-type">Core Contributor</span>
        </div>
        <p><a href="https://wordpress.org/news/2023/08/lionel/" target="_blank" rel="noopener">Official WordPress Release Credits</a></p>
      </div>
      <p class="contribution-description">Sustained contributions to the world's most popular CMS platform</p>
    </div>
  </div>
</div>

## What Colleagues Say

<div class="testimonials-section">
  <div class="testimonials-grid">
    {% for testimonial in site.data.testimonials %}
      {% if testimonial.featured %}
      <div class="testimonial-card">
        <div class="testimonial-content">
          <p>"{{ testimonial.quote }}"</p>
        </div>
        <div class="testimonial-author">
          <div class="author-info">
            <h4>{{ testimonial.name }}</h4>
            <p>{{ testimonial.title }}</p>
            <p class="company">{{ testimonial.company }}</p>
          </div>
        </div>
      </div>
      {% endif %}
    {% endfor %}
  </div>
</div>

## Work With Me

<div class="work-with-me-section">
  <div class="work-advantages">
    <div class="advantage-card">
      <h3>🌎 Remote-First Experience</h3>
      <p><strong>5+ years at Automattic</strong> - the company that pioneered distributed work. I've shipped features to millions of users while working across timezones with teams in San Francisco, London, and beyond.</p>
    </div>
    
    <div class="advantage-card">
      <h3>🇺🇸 US Business Ready</h3>
      <p><strong>No visa sponsorship needed.</strong> I operate via US-based LLC for seamless contracting, or through platforms like Deel. Clean, simple business setup that works just like hiring any US contractor.</p>
    </div>
    
    <div class="advantage-card">
      <h3>⏰ Timezone Flexibility</h3>
      <p><strong>Paraguay (UTC-3)</strong> offers excellent overlap with US timezones. I regularly work US hours and have extensive experience with async communication and distributed team collaboration.</p>
    </div>
    
    <div class="advantage-card">
      <h3>📱 Production iOS Expertise</h3>
      <p><strong>10+ years shipping iOS apps</strong> including subscription systems at scale, StoreKit implementation, and leading mobile teams. From startup MVPs to enterprise security - I've built it all.</p>
    </div>
  </div>
  
  <div class="work-cta">
    <p><strong>Ready to discuss your iOS project?</strong></p>
    <div class="work-cta-buttons">
      <a href="https://calendly.com/pschrottky" class="btn btn-primary" target="_blank">Schedule Call</a>
      <a href="/assets/Paul%20Von%20Schrottky%20-%20Resume.pdf" class="btn btn-secondary" target="_blank">Download Resume</a>
      <a href="mailto:{{ site.email }}" class="btn btn-tertiary">Email Me</a>
    </div>
    <p class="work-note">Available US hours • Typically respond within 24 hours</p>
  </div>
</div>

## Latest Blog Posts
