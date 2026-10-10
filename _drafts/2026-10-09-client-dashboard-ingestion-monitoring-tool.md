---
title: Automated CMS Dashboard Monitoring Tool
---
{One paragraph hook into what I did}

# The Problem With Our Daily Monitoring

*{Writing note: In this section, outline the problem and operational friction on the manual daily monitoring workflow on our team.}*

At my work, we have a daily monitoring of their CMS dashboard. The monitoring task is two-fold: we check if the import is fine, if not, we report this to our engineering. Then we check the content if there are stale or inactive instruments. 

The monitoring involves the following steps:
1. Logging in to the client's CMS portal
2. Copy and pasting multiple content and importer health check status from the CMS dashboard to an Excel sheet that the team keeps as logs.
3. Report failed or missing imports to engineering via mail.
4. Check inactive or outdated instruments based on specific rules, open an internal ticket.

This daily monitoring is rotated between 4 resources in the team, and takes around 15-30 minutes on a daily basis. The majority of this time is spent on the manual task of copying each health check  status, going back and forth between the CMS dashboard and the Excel file, not to mention, the tedious process of having to format the logs properly since it is being copied from a table format. For a monitoring and reporting workflow, the task is cognitively demanding simply because there are a lot of manual steps to be done. Many of our colleagues, honestly, me included, dread the time when we have to work on this monitoring. Since then, I have been thinking how to make this workflow simpler and faster.

# How the tool solved the problem

*{Writing note: In this section, outline how the tool solves the problem discussed in the previous section.}*

Having a background in Python programming and having worked on previous automation projects, I took on the challenge of streamlining this workflow. To start, I outlined what steps are repetitive manual work and can be automated:
1. Copy-pasting health check statuses from the CMS dashboard to Excel
2. The decision logic of choosing what needs to be copied or logged (we skip a lot of items from the dashboard especially if "Healthy/No Error".)
3. Identifying urgent error items from importer health check.
4. Drafting an importer error report mail to our engineering colleagues. 

The result: A Python-based companion that can be executed on a single click (via shortcut!) and does the above tasks for me. 

# How I built the tool

## Reverse Engineering the site's API

Explain how I retrieve the data from the dashboard

## The Cookie Problem

Explain the problems with cookies and how I solved it.
Explain how I prepared the headers

## Filtering through the Noise
turning raw data into an immediate operational signal.
Explain how I parsed the data. 

## Productionizing the tool
Discuss further improvements I added to the tool to make it production-ready:
- Adding exponential retries
- Enriching user experience with rich
- Adding CLI parameters

## Streamlining the workflow
- Sending the data power automate to the logs
- Drafting email to engineers
- Wrote a usage guide for the team

# Impact
- I demo'd the tool to the entire team. The tool is now in production.
- Saves ~50% time.
- Improved recall and review of previous days
# Further work
- Anticipating that vendor APIs don't stay static forever; using strict schema validation and defensive error catching so the pipeline fails loud and clear if contracts shift.

