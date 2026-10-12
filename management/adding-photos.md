---
title: "Guide: Adding Callouts and Images in Posts"
date: 2026-10-12
---

# Guide: Adding Callouts and Images in Posts

This guide covers how to use **callouts (prompts)** and **images** in posts using Jekyll's **Chirpy** theme (`jekyll-theme-chirpy`).

The Chirpy theme relies on [kramdown](https://kramdown.gettalong.org/)'s **Inline Attribute List (IAL)** syntax (`{: ... }`) to apply CSS classes, custom styling, dimensions, and behaviors to standard Markdown elements.

---

## Table of Contents
1. [Callouts (Prompts)](#1-callouts-prompts)
   - [Available Prompt Types](#available-prompt-types)
   - [Basic Syntax](#basic-syntax)
   - [Multi-line & Rich Content Callouts](#multi-line--rich-content-callouts)
   - [Important Rules & Common Mistakes](#important-rules--common-mistakes)
2. [Images](#2-images)
   - [Recommended File Organization](#recommended-file-organization)
   - [The `media_subpath` Shortcut (Best Practice)](#the-media_subpath-shortcut-best-practice)
   - [Featured / Preview Image (Post Cover)](#featured--preview-image-post-cover)
   - [Inline Images](#inline-images)
   - [Image Captions](#image-captions)
   - [Dimensions & Aspect Ratio (Prevent CLS)](#dimensions--aspect-ratio-prevent-cls)
   - [Alignment & Text Wrap](#alignment--text-wrap)
   - [Styling Classes: Shadows & Rounded Corners](#styling-classes-shadows--rounded-corners)
   - [Light Mode & Dark Mode Specific Images](#light-mode--dark-mode-specific-images)
   - [Combining Multiple Attributes](#combining-multiple-attributes)
   - [Lightbox Zoom Behavior](#lightbox-zoom-behavior)
   - [Low-Quality Image Placeholders (LQIP)](#low-quality-image-placeholders-lqip)
3. [Obsidian Integration & Workflow](#3-obsidian-integration--workflow)
4. [Quick Reference Cheat Sheet](#4-quick-reference-cheat-sheet)

---

## 1. Callouts (Prompts)

In Chirpy, callouts are called **Prompts**. They convert regular Markdown blockquotes (`>`) into highlighted, colored alert boxes with automatic Font Awesome icons.

### Available Prompt Types

Chirpy provides four prompt types:

| Type | Class | Color | Default Icon | Best Used For |
| :--- | :--- | :--- | :--- | :--- |
| **Tip** | `{: .prompt-tip }` | Green | Lightbulb | Pro-tips, recommendations, best practices, helpful hints |
| **Info** | `{: .prompt-info }` | Blue | Info Circle | Context, notes, disclaimers, references, prerequisite info |
| **Warning** | `{: .prompt-warning }` | Yellow / Orange | Exclamation Triangle | Caveats, limitations, breaking changes, things to double check |
| **Danger** | `{: .prompt-danger }` | Red | Exclamation Circle / Ban | Breaking pitfalls, security alerts, destructive actions, data loss |

---

### Basic Syntax

To create a prompt, write a standard Markdown blockquote line (`>`) and place the class attribute `{: .prompt-<type> }` on the **immediate next line**:

#### Tip
```markdown
> This is a helpful tip or best practice recommendation.
{: .prompt-tip }
```

#### Info
```markdown
> Here is contextual information, a project disclaimer, or extra reference.
{: .prompt-info }
```

#### Warning
```markdown
> Proceed with caution: this configuration requires an active API token.
{: .prompt-warning }
```

#### Danger
```markdown
> Do not execute this command on production databases without a tested backup.
{: .prompt-danger }
```

---

### Multi-line & Rich Content Callouts

Prompts fully support multi-line text, Markdown formatting (bold, italic, links, inline code), and bullet lists.

#### Multi-paragraph Prompt with Title:
```markdown
> **Project Disclaimer**
> 
> This tool was developed as a mock portfolio piece based on real-world workflows.
> Sensitive customer data and internal endpoints have been anonymized.
{: .prompt-info }
```

#### Prompt with Bullet List and Links:
```markdown
> **Prerequisites Checklist**
> - Ensure Ruby 3.x and Bundler are installed.
> - Verify that `_config.yml` has the correct `url` setting.
> - Check the official [Chirpy Documentation](https://chirpy.cotes.page) for updates.
{: .prompt-tip }
```

#### Prompt with Code Snippets:
```markdown
> Run the tests locally before pushing to GitHub:
> `bundle exec jekyll build`
{: .prompt-warning }
```

---

### Important Rules & Common Mistakes

1. **Keep the blockquote continuous on blank lines:**
   If you have multiple paragraphs inside a prompt, make sure empty lines also start with `>`:
   ```markdown
   <!-- CORRECT -->
   > First paragraph.
   > 
   > Second paragraph.
   {: .prompt-info }

   <!-- INCORRECT (breaks the blockquote into two separate elements) -->
   > First paragraph.

   > Second paragraph.
   {: .prompt-info }
   ```

2. **No empty lines between blockquote and the attribute:**
   The `{: .prompt-xxx }` tag must appear on the line immediately below the blockquote:
   ```markdown
   <!-- CORRECT -->
   > This is a tip.
   {: .prompt-tip }

   <!-- INCORRECT (kramdown will treat it as literal text) -->
   > This is a tip.

   {: .prompt-tip }
   ```

3. **Prefix spacing:**
   Always leave a space before and after the class name inside the curly braces: `{: .prompt-tip }`.

---

## 2. Images

Chirpy offers first-class image support, including automatic lightboxes, aspect-ratio reservation (to prevent layout shifts), captions, and dark/light mode responsiveness.

---

### Recommended File Organization

Keep your assets tidy by organizing images inside `assets/img/`. A clean convention for blog posts is:

```text
assets/
└── img/
    ├── posts/
    │   └── YYYY-MM-DD-post-slug/
    │       ├── cover.png
    │       ├── architecture-diagram.png
    │       └── dashboard-screenshot.png
    └── favicons/
```

For example, for a post dated `2026-10-12-my-new-tool.md`, place images under:
`assets/img/posts/2026-10-12-my-new-tool/`

---

### The `media_subpath` Shortcut (Best Practice)

Writing full paths like `/assets/img/posts/2026-10-12-my-new-tool/screenshot.png` multiple times can be tedious. Chirpy solves this with the `media_subpath` front-matter variable.

Define `media_subpath` once in the post's YAML front matter:

```yaml
---
title: "Building the Automated Monitoring Tool"
date: 2026-10-12
media_subpath: /assets/img/posts/2026-10-12-my-new-tool/
---
```

Once defined, all images (including the cover image and inline images) can use simple relative filenames:

```markdown
![Dashboard Overview](dashboard.png)
```

Jekyll will automatically resolve `dashboard.png` to `/assets/img/posts/2026-10-12-my-new-tool/dashboard.png` during the build.

---

### Featured / Preview Image (Post Cover)

To display a featured header image at the top of your post (and in social preview cards like Open Graph and Twitter):

#### Full Configuration (Recommended):
```yaml
---
title: "Volatility API with Python"
date: 2026-04-07
media_subpath: /assets/img/posts/2026-04-07/
image:
  path: cover.png
  alt: "Architecture diagram of the Volatility API pipeline"
  lqip: "data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAAR..."
---
```

* **`path`**: Path to the image file (or filename if `media_subpath` is set).
* **`alt`**: Description text; also rendered as the visible caption under the header image.
* **`lqip`**: (Optional) Low-Quality Image Placeholder base64 string or file path for smooth progressive loading.
* **Aspect Ratio**: Recommended dimensions are **1200 × 630 px** (aspect ratio **1.91 : 1**).

#### Minimal Configuration:
```yaml
---
title: "Volatility API with Python"
image: /assets/img/posts/cover.png
---
```

---

### Inline Images

To insert an image inside the post content, use standard Markdown:

```markdown
![Detailed descriptive alt text](/assets/img/posts/pipeline-diagram.png)
```

Or with `media_subpath`:
```markdown
![Detailed descriptive alt text](pipeline-diagram.png)
```

---

### Image Captions

In Chirpy, to add a visible centered caption under an image, place *italicized text* (`_..._`) on the **immediate next line** below the image without any blank line:

```markdown
![Database Schema Diagram](schema.png)
_Figure 1: Database table relationships and ingestion foreign keys_
```

> **Note on Captions & Floats:** Captions are only rendered when the image is centered (default). If you use floating alignment classes like `{: .left }` or `{: .right }`, the caption will not be rendered.
{: .prompt-warning }

---

### Dimensions & Aspect Ratio (Prevent CLS)

When web browsers load images without specified dimensions, the page layout jumps as images download (Cumulative Layout Shift). Chirpy supports explicit width and height attributes in pixels:

```markdown
<!-- Shorthand syntax (Chirpy v5.0+) -->
![Workflow Overview](workflow.png){: w="972" h="589" }

<!-- Standard syntax -->
![Workflow Overview](workflow.png){: width="972" height="589" }
```

> **SVG Images:** When using `.svg` images, you **must** specify at least the width (e.g. `{: w="400" }`), otherwise the SVG will not render properly.
{: .prompt-info }

---

### Alignment & Text Wrap

By default, Chirpy centers images. You can adjust the positioning using classes:

| Class | Description |
| :--- | :--- |
| *(Default / No class)* | Centered on its own block with caption support. |
| `{: .normal }` | Left-aligned in the standard block document flow (no text wrapping). |
| `{: .left }` | Floats the image to the **left**, with body text wrapping around its right. |
| `{: .right }` | Floats the image to the **right**, with body text wrapping around its left. |

#### Example: Float Left
```markdown
![Author Profile](profile.png){: .left w="200" h="200" }
This text will start on the right side of the profile photo and wrap underneath once the height of the image is exceeded.
```

---

### Styling Classes: Shadows & Rounded Corners

Chirpy includes built-in styling helper classes:

#### 1. Shadow (`.shadow`)
Adds a subtle drop shadow to the image. This is especially useful for screenshots of software windows, spreadsheets, and application mockups:
```markdown
![Application Mockup](mockup.png){: .shadow w="900" h="500" }
```

#### 2. Rounded Corners (`.rounded-10`)
Adds a `10px` border-radius to soften image corners:
```markdown
![Card Preview](card.png){: .rounded-10 w="600" h="350" }
```

---

### Light Mode & Dark Mode Specific Images

If you have diagrams, logos, or charts with transparent backgrounds that need different color palettes for Dark Mode vs Light Mode, you can include both images and tag them with `.light` and `.dark`:

```markdown
![Architecture Diagram (Light Theme)](architecture-light.png){: .light .shadow }
![Architecture Diagram (Dark Theme)](architecture-dark.png){: .dark .shadow }
```

- When the reader views the site in **Light Mode**, only `architecture-light.png` is displayed.
- When the reader toggles to **Dark Mode**, only `architecture-dark.png` is displayed.

---

### Combining Multiple Attributes

You can freely combine dimensions, styling classes, and theme modes into a single attribute list:

```markdown
![Ingestion Performance Graph](graph-light.png){: w="850" h="480" .light .shadow .rounded-10 }
![Ingestion Performance Graph](graph-dark.png){: w="850" h="480" .dark .shadow .rounded-10 }
_Real-time ingestion performance over a 24-hour cycle_
```

---

### Lightbox Zoom Behavior

* **Enabled by Default**: In Chirpy, inline images are automatically interactive. Clicking an image opens full-screen zoom powered by `glightbox`.
* If an image is small (e.g., an inline badge or icon) and should not trigger the lightbox, you can control its layout with appropriate sizing or utility wrappers.

---

### Low-Quality Image Placeholders (LQIP)

For large hero images or photography, you can pass an LQIP string to show a blurry preview while the full resolution image loads:

```markdown
![High Resolution Map](map.jpg){: lqip="data:image/webp;base64,UklGRpoAAABXR..." w="1200" h="800" }
```

---

## 3. Obsidian Integration & Workflow

If you write or draft posts inside **Obsidian** in this workspace:

1. **Pasted Images in Obsidian:**
   - Obsidian automatically saves pasted screenshots into the `attachments/` folder using syntax like `![[Pasted image 20261004083438.png]]`.
2. **Converting for Jekyll Chirpy:**
   - Jekyll's Kramdown parser does not support Obsidian's internal `![[...]]` wikilink syntax.
   - Move the image from `attachments/` to your post's folder, e.g.:
     `assets/img/posts/YYYY-MM-DD-title/my-screenshot.png`.
   - Update the Markdown link to standard syntax:
     ```markdown
     ![Screenshot description](my-screenshot.png){: .shadow w="800" h="450" }
     ```
3. **Drafting:**
   - You can work in `_drafts/` while writing. Once ready to publish, move the file into `_posts/` with the filename `YYYY-MM-DD-slug.md`.

---

## 4. Quick Reference Cheat Sheet

### Prompts

```markdown
> Useful advice or recommended method.
{: .prompt-tip }

> Helpful context, background note, or disclaimer.
{: .prompt-info }

> Important notice, caveat, or setting to check.
{: .prompt-warning }

> Dangerous action, potential data loss, or critical notice.
{: .prompt-danger }
```

---

### Images

```markdown
<!-- Standard Image with Dimensions & Caption -->
![Dashboard preview](dashboard.png){: w="800" h="450" }
_Figure 1: Production dashboard view_

<!-- Screenshot with Drop Shadow & Rounded Corners -->
![App Screenshot](screenshot.png){: w="900" h="500" .shadow .rounded-10 }

<!-- Image Floated to Left with Wrapped Text -->
![Thumbnail](thumb.png){: .left w="180" h="180" }
Text wrapping alongside the image...

<!-- Light & Dark Mode Alternates -->
![Chart (Light)](chart-light.png){: .light .shadow }
![Chart (Dark)](chart-dark.png){: .dark .shadow }
```

---

### Complete Example Post Template

```markdown
---
title: "Analyzing Pipeline Performance"
date: 2026-10-12 09:00:00 +0800
categories: [Data Engineering, Monitoring]
tags: [python, pipelines, automation]
media_subpath: /assets/img/posts/2026-10-12-pipeline-performance/
image:
  path: cover.png
  alt: "Pipeline Monitoring Architecture"
---

This post explains our real-time telemetry monitoring architecture.

> **Production Notice**
> All endpoints and server hostnames referenced in this guide are mock samples.
{: .prompt-info }

## Architecture Overview

Here is the high-level data flow diagram:

![Data Pipeline Diagram](pipeline-diagram.png){: w="880" h="450" .shadow }
_Figure: End-to-end ingestion pipeline and alert triggers_

> Never store unencrypted service credentials in environment files or source code.
{: .prompt-danger }

### Key Takeaways

> Remember to configure polling intervals according to the upstream rate limits.
{: .prompt-tip }
```
