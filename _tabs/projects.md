---
layout: page
icon: "fa-solid fa-laptop-code"
order: 1
---

<div id="project-list" class="mb-3">
  {% for post in site.categories.Projects %}
    <article class="border rounded-2 p-3 mb-3" style="background: var(--card-bg, inherit);">
      <div class="d-flex justify-content-between align-items-start flex-wrap gap-2 mb-1">
        <h2 class="h6 mb-0 fw-semibold">
          <a href="{{ post.url | relative_url }}" class="text-decoration-none">{{ post.title }}</a>
        </h2>
        <span class="small text-muted">
          <i class="far fa-calendar fa-fw"></i> {{ post.date | date: "%b %d, %Y" }}
        </span>
      </div>
      {% if post.tags.size > 0 %}
        <div class="mb-2">
          {% for tag in post.tags %}
            <span class="badge border text-muted fw-normal me-1 mb-1">{{ tag }}</span>
          {% endfor %}
        </div>
      {% endif %}
      <p class="small text-muted mb-2">{{ post.excerpt | strip_html | truncate: 200 }}</p>
      <div>
        <a href="{{ post.url | relative_url }}" class="small text-primary text-decoration-none fw-semibold">Read Case Study &rarr;</a>
      </div>
    </article>
  {% endfor %}
</div>
