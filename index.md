---
layout: page
---

<!-- Intro Section -->
<div class="mb-4">
  <h2 class="h4 fw-bold mb-2">Hi, I'm Mark 👋</h2>
  <p class="text-muted mb-3">
    I'm a <strong>Lead Digital API Specialist at FactSet</strong> and a <strong>Mathematics graduate from UP Diliman</strong>. 
    I bridge the gap between rigorous mathematical theory and practical, automated software solutions—building quantitative models, high-performance Python APIs, and scalable data workflows.<br><br>
     
    As a <strong>Lead Digital API Specialist</strong>, I acted as a subject matter expert for FactSet's Digital API products. During my first 18 months, I led an automation initiative using the <strong>Power Platform</strong> that overhauled internal queuing workflows—slashing ticket acknowledgment turnaround by <strong>50%</strong> and earning an early promotion.<br><br>
    <strong>Current Direction:</strong> I am expanding further into <strong>Quantitative Development</strong> and <strong>Machine Learning Engineering</strong>. From my undergraduate research applying Support Vector Regression to equity markets, to building econometric REST APIs and earning credentials in Snowflake and Advanced Analytics, I build resilient systems that turn complex data into actionable operational signal.
  </p>
  <div class="d-flex flex-wrap mb-3">
    <a href="mailto:malabaclado@gmail.com?subject=Resume%20Request" class="btn btn-outline-primary btn-sm rounded-pill me-2 mb-2">
      <i class="fas fa-file-alt me-1"></i> Resume
    </a>
    <a href="{{ '/projects/' | relative_url }}" class="btn btn-outline-primary btn-sm rounded-pill me-2 mb-2">
      <i class="fas fa-laptop-code me-1"></i> Projects
    </a>
    <a href="https://www.linkedin.com/in/mark-jayson-labaclado-14603b204/" target="_blank" rel="noopener noreferrer" class="btn btn-outline-primary btn-sm rounded-pill me-2 mb-2">
      <i class="fab fa-linkedin me-1"></i> LinkedIn
    </a>
    <a href="https://github.com/malabaclado" target="_blank" rel="noopener noreferrer" class="btn btn-outline-primary btn-sm rounded-pill me-2 mb-2">
      <i class="fab fa-github me-1"></i> GitHub
    </a>
    <a href="mailto:malabaclado@gmail.com" class="btn btn-outline-primary btn-sm rounded-pill mb-2">
      <i class="fas fa-envelope me-1"></i> Email
    </a>
  </div>
</div>

<!-- Technical Competencies -->
<h2 class="h5 fw-bold mb-3">Technical Competencies</h2>

<div class="row mb-4">
  <div class="col-12 col-md-4 mb-3">
    <div class="border h-100" style="border-radius: 8px; padding: 1.25rem !important; background: var(--card-bg, inherit);">
      <div class="fw-bold small mb-2 text-uppercase text-muted" style="letter-spacing: 0.5px;">Languages &amp; Core</div>
      <div class="d-flex flex-wrap">
        <span class="post-tag me-1 mb-1">Python</span>
        <span class="post-tag me-1 mb-1">SQL</span>
        <span class="post-tag me-1 mb-1">VBA</span>
        <span class="post-tag me-1 mb-1">Git</span>
        <span class="post-tag me-1 mb-1">Bash</span>
      </div>
    </div>
  </div>
  <div class="col-12 col-md-4 mb-3">
    <div class="border h-100" style="border-radius: 8px; padding: 1.25rem !important; background: var(--card-bg, inherit);">
      <div class="fw-bold small mb-2 text-uppercase text-muted" style="letter-spacing: 0.5px;">Data &amp; Platforms</div>
      <div class="d-flex flex-wrap">
        <span class="post-tag me-1 mb-1">Snowflake</span>
        <span class="post-tag me-1 mb-1">FastAPI</span>
        <span class="post-tag me-1 mb-1">SQLite</span>
        <span class="post-tag me-1 mb-1">REST APIs</span>
        <span class="post-tag me-1 mb-1">Power Platform</span>
      </div>
    </div>
  </div>
  <div class="col-12 col-md-4 mb-3">
    <div class="border h-100" style="border-radius: 8px; padding: 1.25rem !important; background: var(--card-bg, inherit);">
      <div class="fw-bold small mb-2 text-uppercase text-muted" style="letter-spacing: 0.5px;">Quantitative &amp; Modeling</div>
      <div class="d-flex flex-wrap">
        <span class="post-tag me-1 mb-1">GARCH</span>
        <span class="post-tag me-1 mb-1">ARIMA</span>
        <span class="post-tag me-1 mb-1">Value-at-Risk</span>
        <span class="post-tag me-1 mb-1">Monte Carlo</span>
        <span class="post-tag me-1 mb-1">NumPy / Pandas</span>
      </div>
    </div>
  </div>
</div>

<!-- Featured Projects -->
<h2 class="h5 fw-bold mb-3">Featured Projects</h2>

<div id="project-list" class="mb-3">
  {% for post in site.categories.Projects limit: 2 %}
    <article class="border mb-3" style="border-radius: 8px; padding: 1.25rem !important; background: var(--card-bg, inherit);">
      <div class="d-flex justify-content-between align-items-baseline flex-wrap mb-2">
        <h3 class="h6 mb-1 fw-bold me-2">
          <a href="{{ post.url | relative_url }}" class="text-decoration-none">{{ post.title }}</a>
        </h3>
        <span class="small text-muted mb-1">
          <i class="far fa-calendar fa-fw"></i> {{ post.date | date: "%b %d, %Y" }}
        </span>
      </div>
      {% if post.tags.size > 0 %}
        <div class="mb-2 d-flex flex-wrap">
          {% for tag in post.tags %}
            <span class="post-tag me-1 mb-1">{{ tag }}</span>
          {% endfor %}
        </div>
      {% endif %}
      <p class="small text-muted mb-2">{{ post.excerpt | strip_html | truncate: 200 }}</p>
      <div>
        <a href="{{ post.url | relative_url }}" class="small text-primary text-decoration-none fw-bold">Read Case Study &rarr;</a>
      </div>
    </article>
  {% endfor %}

  {% if site.categories.Projects.size < 2 %}
    <article class="border mb-3" style="border-radius: 8px; padding: 1.25rem !important; background: var(--card-bg, inherit);">
      <div class="d-flex justify-content-between align-items-baseline flex-wrap mb-2">
        <h3 class="h6 mb-1 fw-bold me-2">Enterprise Incident Queuing &amp; Workflow Automation</h3>
        <span class="small text-muted mb-1">
          <i class="fas fa-briefcase fa-fw"></i> FactSet Research Systems
        </span>
      </div>
      <div class="mb-2 d-flex flex-wrap">
        <span class="post-tag me-1 mb-1">power-platform</span>
        <span class="post-tag me-1 mb-1">automation</span>
        <span class="post-tag me-1 mb-1">workflow-optimization</span>
      </div>
      <p class="small text-muted mb-0">Architected an automated ticketing and incident dispatch solution that streamlined cross-team monitoring, standardized issue intake, and reduced acknowledgment times by 50%.</p>
    </article>
  {% endif %}
</div>

<div class="mb-4">
  <a href="{{ '/projects/' | relative_url }}" class="btn btn-outline-primary btn-sm rounded-pill">
    Explore All Projects &rarr;
  </a>
</div>

<!-- Certifications & Credentials -->
<h2 class="h5 fw-bold mb-3">Certifications &amp; Credentials</h2>

<div class="row mb-4">
  <div class="col-12 col-md-6 col-lg-3 mb-3">
    <div class="border h-100 d-flex flex-column justify-content-between" style="border-radius: 8px; padding: 1.25rem !important; background: var(--card-bg, inherit);">
      <div>
        <div class="d-flex align-items-center justify-content-between mb-1">
          <span class="text-uppercase text-muted" style="font-size: 0.7rem; letter-spacing: 0.5px;">Snowflake</span>
          <i class="fas fa-award text-primary fa-xs"></i>
        </div>
        <div class="fw-bold small mb-1">SnowPro Core</div>
        <div class="small text-muted mb-2" style="font-size: 0.8rem;">Cloud Data Platform</div>
      </div>
      <span class="small text-muted" style="font-size: 0.75rem;">Verified Credential</span>
    </div>
  </div>
  <div class="col-12 col-md-6 col-lg-3 mb-3">
    <div class="border h-100 d-flex flex-column justify-content-between" style="border-radius: 8px; padding: 1.25rem !important; background: var(--card-bg, inherit);">
      <div>
        <div class="d-flex align-items-center justify-content-between mb-1">
          <span class="text-uppercase text-muted" style="font-size: 0.7rem; letter-spacing: 0.5px;">WorldQuant</span>
          <i class="fas fa-award text-primary fa-xs"></i>
        </div>
        <div class="fw-bold small mb-1">Applied Data Science Lab</div>
        <div class="small text-muted mb-2" style="font-size: 0.8rem;">Statistical ML</div>
      </div>
      <a href="https://www.credly.com/badges/0e700622-a287-4190-ba0e-d41961d8fdc6" target="_blank" rel="noopener noreferrer" class="small text-decoration-none" style="font-size: 0.75rem;">
        Verify Badge <i class="fas fa-external-link-alt fa-xs ms-1"></i>
      </a>
    </div>
  </div>
  <div class="col-12 col-md-6 col-lg-3 mb-3">
    <div class="border h-100 d-flex flex-column justify-content-between" style="border-radius: 8px; padding: 1.25rem !important; background: var(--card-bg, inherit);">
      <div>
        <div class="d-flex align-items-center justify-content-between mb-1">
          <span class="text-uppercase text-muted" style="font-size: 0.7rem; letter-spacing: 0.5px;">Google</span>
          <i class="fas fa-award text-primary fa-xs"></i>
        </div>
        <div class="fw-bold small mb-1">Advanced Data Analytics</div>
        <div class="small text-muted mb-2" style="font-size: 0.8rem;">Professional Certificate</div>
      </div>
      <a href="https://www.credly.com/badges/2efdae6f-51d6-43ab-b7eb-f1405d19ef1b" target="_blank" rel="noopener noreferrer" class="small text-decoration-none" style="font-size: 0.75rem;">
        Verify Badge <i class="fas fa-external-link-alt fa-xs ms-1"></i>
      </a>
    </div>
  </div>
  <div class="col-12 col-md-6 col-lg-3 mb-3">
    <div class="border h-100 d-flex flex-column justify-content-between" style="border-radius: 8px; padding: 1.25rem !important; background: var(--card-bg, inherit);">
      <div>
        <div class="d-flex align-items-center justify-content-between mb-1">
          <span class="text-uppercase text-muted" style="font-size: 0.7rem; letter-spacing: 0.5px;">Google</span>
          <i class="fas fa-award text-primary fa-xs"></i>
        </div>
        <div class="fw-bold small mb-1">Data Analytics</div>
        <div class="small text-muted mb-2" style="font-size: 0.8rem;">Professional Certificate</div>
      </div>
      <a href="https://www.credly.com/badges/1e2d3169-1977-44dc-b1e0-1d3ef03b88f5" target="_blank" rel="noopener noreferrer" class="small text-decoration-none" style="font-size: 0.75rem;">
        Verify Badge <i class="fas fa-external-link-alt fa-xs ms-1"></i>
      </a>
    </div>
  </div>
</div>
