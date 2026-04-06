---
layout: page
icon: "fa-solid fa-laptop-code"
order: 1
---

<div id="project-list">
  {% for post in site.categories.Projects %}
    <article class="px-md-4 pb-5">
      <h2 class="h4 mt-0"><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
      <div class="post-meta text-muted d-flex align-items-center mb-1">
        <span class="me-3">
          <i class="far fa-calendar fa-fw"></i>
          {{ post.date | date: "%b %d, %Y" }}
        </span>
      </div>
      <p>{{ post.excerpt | strip_html | truncate: 200 }}</p>
    </article>
  {% endfor %}
</div>
