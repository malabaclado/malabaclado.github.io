---
title: "Custom SCSS Documentation"
date: 2026-10-09
---

# Custom SCSS Documentation

This document explains the purpose, configuration, and maintenance of the custom SCSS stylesheet in this repository.

---

## 1. Purpose

### The Problem:
* **In Local Development (`development`)**: Jekyll Chirpy's `_includes/head.html` automatically injects the full **Bootstrap 5.3.8 CDN** (`https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/css/bootstrap.min.css`). As a result, utility classes like `.p-3`, `.gap-2`, `.g-2`, `.rounded-2`, and `.badge` render properly on localhost.
* **In GitHub Actions / GitHub Pages (`production`)**: The theme disables the external Bootstrap CDN and exclusively compiles Chirpy's internal SCSS bundle (`_bootstrap.scss`). This internal bundle is intentionally stripped of most utility classes to reduce file size.
* **HTML Compression**: In production, `compress_html` strips all whitespace between HTML elements. Combined with missing flex `gap` and padding classes, cards and badges collapsed and collided together when published to GitHub Pages.

### The Solution:
By creating a local override file at `assets/css/jekyll-theme-chirpy.scss`, Jekyll compiles our custom utility definitions directly into the production CSS bundle (`_site/assets/css/jekyll-theme-chirpy.css`), ensuring consistent layout and spacing across both localhost and GitHub Pages.

---

## 2. File Location & Theme Hierarchy

* **File Path**: `assets/css/jekyll-theme-chirpy.scss`
* **Mechanism**: In Jekyll themes distributed as Ruby gems, placing a file at the matching path inside your repository takes precedence over the gem's default template.

---

## 3. Configuration & Structure

The file begins with empty YAML front matter (required by Jekyll to process Sass) followed by the theme's core bundle import:

```scss
---
---

/* prettier-ignore */
@use 'main
{%- if jekyll.environment == 'production' -%}
  .bundle
{%- endif -%}
';

/* Custom Bootstrap & Utility definitions for consistent rendering in production */
...
```

* **`@use 'main...'`**: Imports Chirpy's base styles and CSS variables.
* **Custom Rules Below**: Any CSS or SCSS added below this import is automatically compiled and appended to the final CSS bundle.

---

## 4. Current Defined Utilities

The file currently defines the following utility classes:

| Class | CSS Applied | Usage / Purpose |
| :--- | :--- | :--- |
| `.rounded-2` | `border-radius: 0.5rem !important;` | 8px rounded corners for Material-style cards |
| `.rounded-3` | `border-radius: 0.75rem !important;` | 12px rounded corners |
| `.rounded-pill` | `border-radius: 50rem !important;` | Pill-shaped buttons |
| `.p-3` | `padding: 1rem !important;` | Standard 16px internal card padding |
| `.p-4` | `padding: 1.5rem !important;` | Generous 24px internal card padding |
| `.gap-1` | `gap: 0.25rem !important;` | 4px flex gap |
| `.gap-2` | `gap: 0.5rem !important;` | 8px flex gap between buttons and tags |
| `.gap-3` | `gap: 1rem !important;` | 16px flex gap |
| `.g-2` | `--bs-gutter-x: 0.5rem; --bs-gutter-y: 0.5rem;` | Bootstrap row grid gutters |
| `.align-items-start` | `align-items: flex-start !important;` | Flex alignment |
| `.fw-normal` | `font-weight: 400 !important;` | Regular font weight |
| `.fw-semibold` | `font-weight: 600 !important;` | Semi-bold typography |
| `.fw-bold` | `font-weight: 700 !important;` | Bold typography |
| `.badge` | `display: inline-block; padding: 0.25em 0.55em; font-size: 0.75em; border-radius: 0.375rem; ...` | Inline badge styling |
| `.badge.border` | Uses theme variables `var(--btn-border-color)` and `var(--text-muted-color)` | Theme-adaptive badges (light and dark mode) |

---

## 5. What Can Be Modified Manually

You can freely edit `assets/css/jekyll-theme-chirpy.scss` to:

1. **Add Additional Bootstrap Utilities**:
   If you use new Bootstrap classes in posts or tabs that Chirpy doesn't compile (e.g., `.m-3`, `.shadow-sm`, `.border-top`), define them at the bottom of the file.
2. **Customize Component Styling**:
   Target specific IDs or classes (e.g., `#project-list article`, `.hero-section`) to customize hover effects, borders, or shadows.
3. **Override Theme Variables**:
   Chirpy uses CSS custom properties defined on `:root` and `html[data-mode='dark']`. You can override colors, fonts, or card backgrounds here:
   ```scss
   :root {
     --card-bg: #ffffff;
   }
   
   html[data-mode='dark'] {
     --card-bg: #1e1e24;
   }
   ```
