---
title: "Portfolio Review & Recruiter Recommendations"
date: 2026-10-09
---

# Portfolio Analysis & Recommendations for Recruiters

## 1. Executive Summary & Current Strengths

### What is working well:
* **Strong Mathematical & Domain Foundation**: A BS in Mathematics from UP Diliman combined with enterprise experience (Lead Specialist at FactSet with Digital APIs) provides an advantage over generalist candidates.
* **Standout Flagship Project**: The [Financial Econometrics API](_posts/2026-04-07-volatility-api-python.md) features complete architecture documentation, rigorous statistical diagnostics (ADF, Engle LM, GARCH, VaR), layered design (FastAPI, SQLite, Pydantic), and containerization.
* **Credibility**: Snowflake SnowPro Core certification and verified Credly badges add verifiable third-party validation.

### Primary Friction Points for Recruiters:
1. **The 15-Second Scan Test**: Recruiters and hiring managers spend 15–30 seconds evaluating a portfolio. Currently, `/` displays a bio and badges without immediate technical projects visible above the fold.
2. **Low Published Project Count**: The "Projects" tab currently contains only one published project (`volatility-api-python`), while several strong projects remain in `_drafts`.
3. **Missing Resume / Direct CTA**: There is no direct button or link to download or view a 1-page PDF resume.
4. **Unconfigured SEO / Site Metadata**: `_config.yml` still contains default Chirpy theme description text (`"A minimal, responsive and feature-rich Jekyll theme for technical writing."`), which displays when sharing links on LinkedIn or search engines.

---

## 2. Immediate High-Impact Recommendations

### A. Optimize the Landing Page (`index.md`)
* **Add a "Quick Resume / Contact" Hero Bar**:
  Place a call-to-action near the top:
  `[📄 Download Resume (PDF)]` `[💼 LinkedIn]` `[💻 GitHub]` `[✉️ Email]`
* **Curate a "Tech Stack & Core Competencies" Grid**:
  Align keywords with target job descriptions:
  * **Languages & Frameworks**: Python (FastAPI, Pandas, NumPy, SciPy, arch), SQL, VBA, Git.
  * **Data & Cloud**: Snowflake (SnowPro Core), SQLite, REST APIs, Power Platform.
  * **Quantitative & Analytical**: Time Series Analysis (GARCH, ARIMA), Value-at-Risk (Parametric & Monte Carlo), Statistical Testing, Machine Learning.
* **Showcase 1–2 "Featured Projects" Directly on the Home Page**:
  Highlight the Financial Econometrics API and an enterprise automation project directly after the bio to present core competencies immediately.

### B. Publish & Polish High-Value Drafts
Prioritize polishing and moving these drafts to `_posts/`:
1. **`_drafts/2026-10-09-client-dashboard-ingestion-monitoring-tool.md`**:
   * **Why it sells**: Demonstrates initiative and end-to-end engineering—reverse-engineering an API, session handling, resilient retries, CLI tooling, and saving ~50% monitoring time.
2. **`_drafts/2026-04-25-multi-asset-var-calculator.md`**:
   * **Why it sells**: Monte Carlo simulation for multi-asset risk reinforces quantitative finance skills and complements the GARCH API project.

### C. Adopt the "STAR + Artifacts" Case Study Template
Ensure the top 20% of every project post includes:
* **One-sentence Hook**: The core business or technical problem solved.
* **Live Demo / Repo Links**: Direct links to GitHub repo, documentation, or interactive demos.
* **Tech Stack Badges**: Key tags (`#FastAPI`, `#SQLite`, `#Docker`, `#GARCH`).
* **Quantified Impact**: Concrete metrics (e.g., *"Reduced acknowledgment times by 50%"* or *"Eliminated 80% redundant API calls through SQLite caching"*).

### D. Fix Site Metadata & Configuration (`_config.yml`)
* Update `description` in `_config.yml`:
  ```yaml
  description: >-
    Portfolio & technical writing by Mark Labaclado. BS Mathematics (UP Diliman), 
    Lead Specialist at FactSet. Focused on Quantitative Finance, Python APIs, and Data Science.
  ```
* Update `tagline` in `_config.yml`:
  ```yaml
  tagline: |
    Quantitative Developer & Data Specialist <br>
    FactSet | BS Mathematics, UP Diliman
  ```
* Update `_data/contact.yml`: configure or comment out inactive social links (such as Twitter/X).

---

## 3. Strategic Positioning: Framing the Career Pivot

| Target Role | Hiring Manager Priorities | Projects / Experience to Emphasize |
| :--- | :--- | :--- |
| **Quantitative Developer / Analyst** | Mathematical theory, numerical computing, market risk models | GARCH API, Monte Carlo VaR calculator, statistical hypothesis testing (ADF, ARCH LM). |
| **Data Scientist / ML Engineer** | Feature engineering, model evaluation, reproducible pipelines | Time series forecasting, SVR model from undergraduate thesis, model registry design (`models.sqlite`). |
| **Data / Analytics Engineer** | Data modeling, API pipelines, ETL orchestration, query performance | Snowflake SnowPro certification, TwelveData API ingestion pipeline, SQLite caching layer. |

---

## 4. Suggested Implementation Roadmap

1. **Step 1 (Quick Win)**: Update `_config.yml` metadata, tagline, and contact links.
2. **Step 2 (Content)**: Finalize and publish `client-dashboard-ingestion-monitoring-tool.md` to `_posts/`.
3. **Step 3 (Landing Experience)**: Add a concise Tech Stack section and link to resume on `index.md`.
4. **Step 4 (Navigation)**: Embed a featured projects preview on the landing page.
