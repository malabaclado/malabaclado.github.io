---
title: "Sidebar & Navigation Improvement Plan"
date: 2026-10-09
---

# Sidebar & Navigation Improvement Plan

This plan outlines the enhancements to streamline navigation, sharpen professional positioning, and eliminate redundant or broken links in the sidebar.

---

## 1. Streamline Navigation Tabs (`_tabs/`)

### Objective:
Replace sparse taxonomy archives with high-value destination tabs relevant to recruiters and visitors.

### Proposed Tab Structure:

| Order | Tab Name | File | Route | Icon | Description |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **1** | **Projects** | `_tabs/projects.md` | `/projects/` | `fa-solid fa-laptop-code` | Showcase of technical builds & case studies |
| **2** | **Blog** | `_tabs/blog.md` | `/blog/` | `fa-solid fa-pen-nib` | Technical notes, articles, and econometric write-ups |
| **3** | **CV** | `_tabs/cv.md` | `/cv/` | `fa-solid fa-file-lines` | Structured online CV with direct PDF download/request option |
| **4** | **Tags** | `_tabs/tags.md` | `/tags/` | `fa-solid fa-tags` | Cross-cutting topic taxonomy |

### Files to Retire from Active Navigation:
* Move `_tabs/archives.md` and `_tabs/categories.md` into `archive/` so they no longer clutter the sidebar with near-empty index pages.

---

## 2. Update Sidebar Tagline (`_config.yml`)

### Selected Tagline (Option C — Minimalist):
```yaml
tagline: |
  Lead API Specialist @ FactSet <br>
  BS Mathematics, UP Diliman
```

* **Rationale**: Clean, concise, and immediately communicates enterprise seniority and quantitative academic pedigree without visual noise.

---

## 3. Clean & Reorder Contact Options (`_data/contact.yml`)

### Changes:
1. **Remove / Comment Out Twitter (X)**:
   * Prevents broken/dead clicks since no Twitter handle is currently configured.
2. **Reorder by Professional Impact**:
   1. **GitHub** (`fab fa-github`) — Direct proof of code and repository artifacts.
   2. **LinkedIn** (`fab fa-linkedin`) — Professional profile and network.
   3. **Email** (`fas fa-envelope`) — Direct contact channel.

---

## 4. Implementation Steps (Awaiting Confirmation)

1. **Step 1: Configuration**:
   * Update `tagline` in `_config.yml`.
   * Update and reorder social links in `_data/contact.yml`.
2. **Step 2: Tab Updates**:
   * Create `_tabs/blog.md` (listing posts under `site.categories.Blog`).
   * Create `_tabs/cv.md` (clean web CV with experience, education, certs, and contact/download links).
   * Relocate `_tabs/archives.md` and `_tabs/categories.md` to `archive/`.
   * Update tab orders and icons to maintain consistent numbering (1 to 4).
3. **Step 3: Verification**:
   * Confirm the sidebar displays the clean 4-tab list, correct icons, clean tagline, and active contact links.

---

# Changes

The following changes have been implemented:

1. **`_config.yml`**:
   - Updated `tagline` to Option C:
     ```yaml
     tagline: |
       Lead API Specialist @ FactSet <br>
       BS Mathematics, UP Diliman
     ```
   - Added `management` to the `exclude` list to prevent internal notes and plans from compiling into static site outputs.

2. **`_data/contact.yml`**:
   - Reordered contact options to prioritize professional proof:
     1. **GitHub** (`fab fa-github`)
     2. **LinkedIn** (`fab fa-linkedin`)
     3. **Email** (`fas fa-envelope`)
   - Commented out the unconfigured Twitter entry to eliminate broken links.

3. **`_tabs/blog.md`** *(New File)*:
   - Added a dedicated writing tab (`title: Blog`, `icon: fa-solid fa-pen-nib`, `order: 2`).
   - Dynamically lists posts grouped under `site.categories.Blog`.

4. **`_tabs/cv.md`** *(New File)*:
   - Added an online CV page (`title: Curriculum Vitae`, `icon: fa-solid fa-file-lines`, `order: 3`).
   - Structured into Executive Summary, Professional Experience (FactSet), Education (BS Mathematics, UP Diliman), Credentials (Snowflake, WorldQuant, Google), and Technical Competencies, with a PDF request action button.

5. **`_tabs/tags.md`**:
   - Adjusted sequence to `order: 4` to maintain clean navigation order.

6. **`archive/`**:
   - Relocated `archives.md` and `categories.md` out of `_tabs/` and into `archive/` to keep the active sidebar focused.

7. **`index.md`** *(Home Page Optimization)*:
   - Removed `title: About` front matter so Chirpy does not render the default heading.
   - Introduced a conversational hero greeting (*"Hi, I'm Mark 👋"*).
   - Removed the bulky metrics section.
   - Reordered sections to present **Technical Competencies** before **Featured Projects**.
   - Archived previous versions in `management/`.
