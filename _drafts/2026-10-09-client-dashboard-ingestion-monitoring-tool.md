---
title: Automated CMS Dashboard Monitoring Tool
---
{One paragraph hook into what I did}

# The Problem With Our Daily Monitoring

*{Intro: How I come up with the idea; include operational friction - wasted time, cognitive fatigue, missed monitoring}*

As a Lead Specialist at FactSet, part of our function is to execute different custom workflows for our customers. In one of our custome

**Core Tech Stack**

# How the tool solved the problem

*{The spark: How my solution solved the problem}*

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

