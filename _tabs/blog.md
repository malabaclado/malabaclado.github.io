---
layout: page
title: Blog
icon: fa-solid fa-pen-nib
order: 2
---

<div id="blog-list">
  {% for post in site.categories.Blog %}
    <article class="border-bottom pb-4 mb-4">
      <h2 class="h5 mt-0">
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </h2>
      <div class="post-meta text-muted d-flex flex-wrap align-items-center mb-2">
        <span class="me-3">
          <i class="far fa-calendar fa-fw"></i>
          {{ post.date | date: "%b %d, %Y" }}
        </span>
        {% if post.tags.size > 0 %}
          <span class="d-inline-flex flex-wrap align-items-center">
            <i class="fa fa-tags fa-fw me-1"></i>
            {% for tag in post.tags %}
              <span class="badge text-muted border me-1 mb-1">{{ tag }}</span>
            {% endfor %}
          </span>
        {% endif %}
      </div>
      <p class="small text-muted mb-2">{{ post.excerpt | strip_html | truncate: 220 }}</p>
      <a href="{{ post.url | relative_url }}" class="small text-primary font-weight-bold text-decoration-none">Read Article &rarr;</a>
    </article>
  {% endfor %}
</div>
