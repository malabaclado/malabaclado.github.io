---
layout: page
title: About
---

<!-- Hero Section -->
<div class="hero-section mb-4 pb-2">
  <div class="d-flex align-items-center gap-2 mb-2">
    <span class="badge border text-success bg-transparent small px-2 py-1">
      <i class="fas fa-circle text-success fa-2xs me-1"></i> Data Science &amp; Quantitative Engineering
    </span>
  </div>
  <h1 class="display-6 fw-bold mb-2">Bridging Mathematical Theory &amp; Scalable Data Systems</h1>
  <p class="lead text-muted mb-3">
    Lead Digital API Specialist at <strong>FactSet</strong> &bull; BS Mathematics, <strong>University of the Philippines Diliman</strong>.<br>
    Specializing in financial econometrics, high-performance Python APIs, time-series forecasting, and enterprise data automation.
  </p>
  <div class="d-flex flex-wrap gap-2 mb-3">
    <a href="mailto:malabaclado@gmail.com?subject=Resume%20Request" class="btn btn-outline-primary btn-sm">
      <i class="fas fa-file-alt me-1"></i> Request Resume
    </a>
    <a href="{{ '/projects/' | relative_url }}" class="btn btn-outline-primary btn-sm">
      <i class="fas fa-laptop-code me-1"></i> View Projects
    </a>
    <a href="https://www.linkedin.com/in/mark-jayson-labaclado-14603b204/" target="_blank" rel="noopener noreferrer" class="btn btn-outline-primary btn-sm">
      <i class="fab fa-linkedin me-1"></i> LinkedIn
    </a>
    <a href="https://github.com/malabaclado" target="_blank" rel="noopener noreferrer" class="btn btn-outline-primary btn-sm">
      <i class="fab fa-github me-1"></i> GitHub
    </a>
    <a href="mailto:malabaclado@gmail.com" class="btn btn-outline-primary btn-sm">
      <i class="fas fa-envelope me-1"></i> Email
    </a>
  </div>
</div>

<!-- Impact Metrics Grid -->
<div class="row g-3 mb-5">
  <div class="col-6 col-md-3">
    <div class="p-3 border rounded-3 h-100 text-center">
      <div class="h4 fw-bold text-primary mb-1">50%</div>
      <div class="small fw-semibold mb-1">Queue Turnaround</div>
      <div class="small text-muted">Cut incident acknowledgement times by 50% via automated routing at FactSet.</div>
    </div>
  </div>
  <div class="col-6 col-md-3">
    <div class="p-3 border rounded-3 h-100 text-center">
      <div class="h4 fw-bold text-primary mb-1">GARCH</div>
      <div class="small fw-semibold mb-1">Risk Forecasting</div>
      <div class="small text-muted">Engineered REST API for multi-horizon volatility &amp; VaR modeling in Python.</div>
    </div>
  </div>
  <div class="col-6 col-md-3">
    <div class="p-3 border rounded-3 h-100 text-center">
      <div class="h4 fw-bold text-primary mb-1">SnowPro</div>
      <div class="small fw-semibold mb-1">Core Certified</div>
      <div class="small text-muted">Demonstrated proficiency in Snowflake cloud data warehouse architectures.</div>
    </div>
  </div>
  <div class="col-6 col-md-3">
    <div class="p-3 border rounded-3 h-100 text-center">
      <div class="h4 fw-bold text-primary mb-1">BS Math</div>
      <div class="small fw-semibold mb-1">UP Diliman</div>
      <div class="small text-muted">Rigorous foundation in probability theory, linear algebra, and statistical inference.</div>
    </div>
  </div>
</div>

<!-- Featured Projects Section -->
<h3 class="mb-3"><i class="fas fa-layer-group text-primary me-2"></i>Featured Projects</h3>

<div id="project-list" class="mb-4">
  {% for post in site.categories.Projects limit: 2 %}
    <article class="p-3 border rounded-3 mb-3">
      <div class="d-flex justify-content-between align-items-start flex-wrap gap-2 mb-2">
        <h4 class="h5 mb-0">
          <a href="{{ post.url | relative_url }}" class="text-decoration-none">{{ post.title }}</a>
        </h4>
        <span class="small text-muted">
          <i class="far fa-calendar fa-fw"></i> {{ post.date | date: "%b %d, %Y" }}
        </span>
      </div>
      {% if post.tags.size > 0 %}
        <div class="mb-2">
          {% for tag in post.tags %}
            <span class="badge border text-muted me-1 mb-1">{{ tag }}</span>
          {% endfor %}
        </div>
      {% endif %}
      <p class="small text-muted mb-3">{{ post.excerpt | strip_html | truncate: 220 }}</p>
      <div class="d-flex gap-2">
        <a href="{{ post.url | relative_url }}" class="btn btn-outline-primary btn-sm">Read Case Study &rarr;</a>
      </div>
    </article>
  {% endfor %}

  {% if site.categories.Projects.size < 2 %}
    <article class="p-3 border rounded-3 mb-3">
      <div class="d-flex justify-content-between align-items-start flex-wrap gap-2 mb-2">
        <h4 class="h5 mb-0">Enterprise Incident Queuing &amp; Workflow Automation</h4>
        <span class="small text-muted">
          <i class="fas fa-briefcase fa-fw"></i> FactSet Research Systems
        </span>
      </div>
      <div class="mb-2">
        <span class="badge border text-muted me-1 mb-1">power-platform</span>
        <span class="badge border text-muted me-1 mb-1">automation</span>
        <span class="badge border text-muted me-1 mb-1">workflow-optimization</span>
      </div>
      <p class="small text-muted mb-2">Architected an automated ticketing and incident dispatch solution that streamlined cross-team monitoring, standardized issue intake, and reduced acknowledgment times by 50%.</p>
    </article>
  {% endif %}
</div>

<div class="mb-5">
  <a href="{{ '/projects/' | relative_url }}" class="btn btn-outline-primary btn-sm">
    <i class="fas fa-arrow-right me-1"></i> Explore All Projects
  </a>
</div>

<!-- Technical Competencies -->
<h3 class="mb-3"><i class="fas fa-code text-primary me-2"></i>Technical Competencies</h3>

<div class="row g-3 mb-5">
  <div class="col-md-4">
    <div class="p-3 border rounded-3 h-100">
      <h6 class="fw-bold mb-2"><i class="fas fa-terminal me-2 text-primary"></i>Languages &amp; Core</h6>
      <div class="d-flex flex-wrap gap-1">
        <span class="badge border text-muted">Python</span>
        <span class="badge border text-muted">SQL</span>
        <span class="badge border text-muted">VBA</span>
        <span class="badge border text-muted">Git</span>
        <span class="badge border text-muted">Bash</span>
      </div>
    </div>
  </div>
  <div class="col-md-4">
    <div class="p-3 border rounded-3 h-100">
      <h6 class="fw-bold mb-2"><i class="fas fa-database me-2 text-primary"></i>Data &amp; Cloud Systems</h6>
      <div class="d-flex flex-wrap gap-1">
        <span class="badge border text-muted">Snowflake</span>
        <span class="badge border text-muted">SQLite</span>
        <span class="badge border text-muted">RESTful APIs</span>
        <span class="badge border text-muted">FastAPI</span>
        <span class="badge border text-muted">Power Platform</span>
      </div>
    </div>
  </div>
  <div class="col-md-4">
    <div class="p-3 border rounded-3 h-100">
      <h6 class="fw-bold mb-2"><i class="fas fa-chart-line me-2 text-primary"></i>Quantitative &amp; ML</h6>
      <div class="d-flex flex-wrap gap-1">
        <span class="badge border text-muted">Time Series (GARCH/ARIMA)</span>
        <span class="badge border text-muted">Value-at-Risk (VaR)</span>
        <span class="badge border text-muted">Monte Carlo Simulation</span>
        <span class="badge border text-muted">Hypothesis Testing</span>
        <span class="badge border text-muted">Pandas / NumPy / SciPy</span>
      </div>
    </div>
  </div>
</div>

<!-- Background & Experience -->
<h3 class="mb-3"><i class="fas fa-user text-primary me-2"></i>Background &amp; Focus</h3>

<div class="mb-5">
  <p>
    I am a <strong>Mathematics graduate</strong> and <strong>Data Professional</strong> based in the Philippines, focused on bridging the gap between rigorous mathematical theory and scalable automated software solutions.
  </p>
  <p>
    Currently, I serve as a <strong>Lead Specialist at FactSet</strong>, acting as a subject matter expert for Digital API products. During my first 18 months, I led an automation initiative using the <strong>Power Platform</strong> that overhauled internal queuing workflows—slashing ticket acknowledgment turnaround by <strong>50%</strong> and earning an early promotion.
  </p>
  <p>
    <strong>Current Direction:</strong> I am expanding further into <strong>Quantitative Development</strong> and <strong>Machine Learning Engineering</strong>. From my undergraduate research applying Support Vector Regression to equity markets, to building econometric REST APIs and earning credentials in Snowflake and Advanced Analytics, I build resilient systems that turn complex data into actionable operational signal.
  </p>
</div>

<!-- Certifications & Credentials -->
<h3 class="mb-3"><i class="fas fa-certificate text-primary me-2"></i>Certifications &amp; Credentials</h3>

<div class="row g-3 mb-4">
  <div class="col-md-6 col-lg-3">
    <div class="p-3 border rounded-3 h-100 d-flex flex-column justify-content-between">
      <div>
        <div class="d-flex align-items-center justify-content-between mb-2">
          <span class="badge border text-muted small">Snowflake</span>
          <i class="fas fa-award text-primary"></i>
        </div>
        <h6 class="fw-bold mb-1">SnowPro Core Certification</h6>
        <p class="small text-muted mb-2">Snowflake Cloud Data Platform</p>
      </div>
      <div class="small text-muted">Verified Credential</div>
    </div>
  </div>
  <div class="col-md-6 col-lg-3">
    <div class="p-3 border rounded-3 h-100 d-flex flex-column justify-content-between">
      <div>
        <div class="d-flex align-items-center justify-content-between mb-2">
          <span class="badge border text-muted small">WorldQuant University</span>
          <i class="fas fa-award text-primary"></i>
        </div>
        <h6 class="fw-bold mb-1">Applied Data Science Lab</h6>
        <p class="small text-muted mb-2">Statistical Modeling &amp; Machine Learning</p>
      </div>
      <a href="https://www.credly.com/badges/0e700622-a287-4190-ba0e-d41961d8fdc6" target="_blank" rel="noopener noreferrer" class="small text-decoration-none">
        Verify Badge <i class="fas fa-external-link-alt fa-xs ms-1"></i>
      </a>
    </div>
  </div>
  <div class="col-md-6 col-lg-3">
    <div class="p-3 border rounded-3 h-100 d-flex flex-column justify-content-between">
      <div>
        <div class="d-flex align-items-center justify-content-between mb-2">
          <span class="badge border text-muted small">Google</span>
          <i class="fas fa-award text-primary"></i>
        </div>
        <h6 class="fw-bold mb-1">Advanced Data Analytics</h6>
        <p class="small text-muted mb-2">Professional Certificate</p>
      </div>
      <a href="https://www.credly.com/badges/2efdae6f-51d6-43ab-b7eb-f1405d19ef1b" target="_blank" rel="noopener noreferrer" class="small text-decoration-none">
        Verify Badge <i class="fas fa-external-link-alt fa-xs ms-1"></i>
      </a>
    </div>
  </div>
  <div class="col-md-6 col-lg-3">
    <div class="p-3 border rounded-3 h-100 d-flex flex-column justify-content-between">
      <div>
        <div class="d-flex align-items-center justify-content-between mb-2">
          <span class="badge border text-muted small">Google</span>
          <i class="fas fa-award text-primary"></i>
        </div>
        <h6 class="fw-bold mb-1">Data Analytics Professional</h6>
        <p class="small text-muted mb-2">Professional Certificate</p>
      </div>
      <a href="https://www.credly.com/badges/1e2d3169-1977-44dc-b1e0-1d3ef03b88f5" target="_blank" rel="noopener noreferrer" class="small text-decoration-none">
        Verify Badge <i class="fas fa-external-link-alt fa-xs ms-1"></i>
      </a>
    </div>
  </div>
</div>
