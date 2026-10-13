---
title: Automated CMS Dashboard Monitoring Tool
layout: post
media_subpath: /assets/img/posts/2026-10-09-automated-cms-dashboard-monitoring/
categories:
  - Projects
---

In this project, I will show you how I tackled a real operational gap at my workplace by automating a manually repetitive and time-consuming workflow. 

Every morning, someone from our team spent up to half an hour copying health check results from a web dashboard into an Excel sheet. I replaced that routine with a Python tool that fetches the data, filters out the noise, logs the results, and drafts the error report for engineering, all from a single click. The daily task now takes 5 to 10 minutes, and the team has adopted it in production.


> **Disclaimer**
> This is a portfolio copy of a tool built for real work, and it has been anonymized.
> - **Client details are removed or replaced.** The customer name, dashboard URL, email addresses and engineer names are placeholders (`Client`, `www.placeholder-url.com`, `example.com` addresses, fictional names).
> - **The sample data is a snapshot.** The files in `mock_input/` were derived from real health check responses. The customer name was removed from the file names and the connection details, but the fund and asset names, vendor names and statuses inside them are kept so the parsers have realistic input. These are public market data, and some fund names include the name of a well-known bank.
> - **It runs as a mock, for demo purposes only.** By default the tool does **not** call any live service. It reads the sample responses from `mock_input/` and shows the same workflow end to end. The placeholder URL is not a real endpoint, so the live code path will fail if you switch demo mode off.
> - **Nothing is monitored.** The results you see are the same sample data on every run.
>{: .prompt-info }


> You can find the source code and documentation for this project on [GitHub](https://github.com/malabaclado/predicting-stock-volatility-using-Python). 
{: .prompt-info }


# The Problem With Our Daily Monitoring

My team runs a daily monitoring routine on a client's CMS dashboard. The task has two parts. First, we check that the data importers ran correctly, and if they didn't, we report the failures to engineering. Second, we check the content itself for stale or inactive instruments.

In practice, the routine looked like this:
1. Log in to the client's CMS portal.
2. Copy the content and importer health check statuses from the dashboard into an Excel sheet that the team keeps as its log.
3. Report failed or missing imports to engineering by email.
4. Check for inactive or outdated instruments against a set of specific rules, and open an internal ticket when one is found.

The duty rotates between four people on the team and takes 15 to 30 minutes each day. Most of that time goes to the copying itself: switching between the dashboard and the Excel file, pasting each status, and then reformatting the log because data copied out of a web table never lands cleanly in a spreadsheet.

None of this work is difficult, but it is demanding in a way that adds up. **A monitoring and reporting task is supposed to be about judgment, spotting what is wrong and deciding who needs to know.** Instead, most of the effort went into mechanical steps where a single slip meant a missing or misformatted log entry. Many of my colleagues, myself included, dreaded the days the rotation landed on us. That was the point where I started asking how to make this workflow simpler and faster.

# How the Tool Solved the Problem

Since I have a background in Python and had built automation projects before, so I took on the challenge. I began by separating the work that needs a human from the work that doesn't. Four steps turned out to be purely repetitive:

1. Copying health check statuses from the dashboard into Excel.
2. Deciding what is worth logging. We already skip most of the dashboard, especially anything marked "Healthy" or "No Error".
3. Identifying the urgent errors in the importer health check.
4. Drafting the importer error report to our engineering colleagues.

The result is a Python-based tool that runs from a single shortcut click and handles all four. It walks the user through a short, four-step workflow: check whether today's data already exists, fetch and filter the health checks, email a summary to the team, and open a pre-filled draft for the engineers. The tool does the legwork, and the human still decides what gets sent.

# How I Built the Tool

## Reverse Engineering the Site's API

The dashboard has no public API or export feature, so the first question was where its data comes from. A web page that shows a table has to get that table from somewhere. By inspecting the Network tab (filtered to Fetch/XHR) in the browser's developer tools while the dashboard loaded, I found that the page fills itself from two JSON endpoints, one for the content check and one for the importer check (for example, `/healthcheck/content/`). The same inspection gave me the request headers the browser sends along with each call.

That meant I could skip the page entirely. Instead of scraping rendered HTML, the tool sends the same requests the browser sends and receives the raw health check data directly. This is faster and more stable than scraping, because it doesn't depend on how the page looks.

![[Pasted image 20261012153658.png]]

## The Cookie Problem

Calling the endpoints directly created a new problem: *authentication*. The dashboard identifies a logged-in user through *session cookies* and a *CSRF token*, and these expire after a few hours. A script that can't log in is useless, but I deliberately chose not to automate the login itself, for three reasons:

1. **Lockout risk.** Repeated automated sign-ins during testing could lock the monitoring account.
2. **Unwanted attention.** Scripted logins would draw unnecessary attention to the monitoring account.
3. **Security.** Our enterprise machines have strict security in place. Browser-based automation would have added complexity and a possible security risk that I didn't want to introduce.

I took a more practical route. When the tool detects that its saved cookies are older than six hours, it asks the user to copy any dashboard request from the browser's developer tools using "Copy as cURL" and paste it into the terminal. The tool parses the pasted text with regular expressions, extracts the session ID, CSRF token and user analysis values, and saves them for the next six hours. The same pasted request also provides the headers, so every call looks like it came from a normal browser session.

This keeps the user in control of the login and still removes most of the friction. Refreshing the cookies takes seconds, and once it is done, later runs within the window skip that step completely.

![[Pasted image 20261012153851.png]]


```python
def update_cookies(cookies: dict):
    """Interactively refresh stale cookies from a pasted browser cURL command.
    Expects the user to paste a "Copy as cURL (cmd)" request from DevTools
    (see dialogues['refresh_cookies_instruction'] for the exact steps) and
    reads it line-by-line from stdin until a blank 'END' line, extracting the
    CSRF token, session ID, and user analysis values via regex.
    """
    print('Cookies are stale. Please update to ensure the program works correctly.')
    print(dialogues['refresh_cookies_instruction'])
    cookies_updated = False
    print("Enter your text (Press 'ENTER' to finish input):")
    for line in iter(input, ""):
        if '^"X-CSRFToken:' in line:
            x_csrf_token = extract_regex(r'X-CSRFToken: (.+?)\^"', line)
            modify_cookies(cookies=cookies, key="X_CSRF_TOKEN", value=x_csrf_token)
            # update_env("X_CSRF_TOKEN", x_csrf_token)
            # print('X-CSRFTOKEN=' + x_csrf_token)
        elif '^"USER_ANALYSIS' in line:
            print('Started')
            sessionid = extract_regex(r'PHPSESSID=(.+?)\^"',line)
            modify_cookies(cookies=cookies, key="PHPSESSID", value=sessionid)
            # update_env("PHPSESSID", sessionid)
            # print('SESSIONID=' + sessionid)
            csrf_token = extract_regex(r'CSRF_TOKEN=(.+?);',line)
            modify_cookies(cookies=cookies, key="CSRF_TOKEN", value=csrf_token)
            # update_env("CSRF_TOKEN", csrf_token)
            # print('CSRFTOKEN=' + csrf_token)
            user_analysis = extract_regex(r'USER_ANALYSIS=(.+?);',line)
            modify_cookies(cookies=cookies, key="USER_ANALYSIS", value=user_analysis)
            # update_env("USER_ANALYSIS", user_analysis)
            # print('USER_ANALYSIS=' + user_analysis)
            # cookies_updated = TRUE only if Cookie line detected and above code ran
            now = datetime.now()
            modify_cookies(cookies, key='LAST_UPDATED', value = datetime.now().strftime("%Y-%m-%d %H:%M:%S"))
            cookies_updated = True
        elif line == '':
            continue
        elif line == 'END':
            break
    if cookies_updated == True:
        print('BROWSER COOKIES UPDATED!')
    else:
        print('BROWSER COOKIES UPDATE FAILED.')

```
## Filtering Through the Noise

Getting the data was only half the job. The raw responses are deeply nested trees of asset classes, importers and checks, and the details are embedded as HTML fragments inside the JSON. Most of what they contain is healthy status that nobody needs to read.

The parser turns that into a short list of things that need attention:

- **Flatten and filter.** It walks the tree, drops every healthy branch (status 1) and keeps only Alerts (2) and Errors (3). It also drops "no issues found" messages and the entries our team has agreed not to track, such as the Structured Products asset class.
- **Extract the details.** Using BeautifulSoup and regular expressions, it pulls out what a person actually needs from the HTML: the ISIN, instrument title, last update timestamp or error message, depending on the type of check.
- **Handle the special cases.** Some importer checks need their own treatment. Night file checks arrive as many separate ISIN entries and are merged into one report. Error logs are converted from HTML to plain text, and oversized descriptions are truncated so that a single log can't bloat the report.

The output is a readable text summary plus pipe-delimited CSV files for each check. The filtering rules, and the reasoning behind each one, are written down in a separate documentation file so that anyone on the team can tell why an item does or doesn't appear in a report.

## Productionizing the Tool

A script that works on my machine is not the same as a tool the team can rely on every morning. Three improvements made the difference:

- **Exponential retries.** Network calls fail occasionally. Instead of crashing, the tool retries with increasing delays of 5, 10 and 20 seconds, capped at 30, and only reports a failure once the retries are exhausted.
- **A better user experience with Rich.** The terminal output uses panels, spinners and a final execution summary, so the user always knows which step is running and what happened.
- **CLI parameters for auto mode.** An `--auto` flag takes the default answer at every prompt, so the tool can run unattended once the cookies are fresh.

## Streamlining the Workflow

The final piece was connecting the tool's output to the rest of the routine, so that nothing is left to copy by hand:

- **Logging.** In step 2 of the workflow, the tool emails both CSV reports to the team's mailbox so everyone can see the results. That email also triggers a Power Automate flow, which saves the CSV files to a dedicated SharePoint folder. Our daily log review Excel sheet pulls the files from that folder through Power Query and displays them in a single table. The log now fills itself in without anyone copying or pasting.
- **Reporting to engineers.** The importer errors are compiled into an HTML table and opened as an Outlook draft addressed to the engineers. I chose a draft over automatic sending on purpose: a human reviews it first, which keeps accountability with the person on duty.
- **Adoption.** I wrote a usage guide so that all four people on the rotation could pick up the tool without needing me.

# Power Automate Flow

![[Pasted image 20261012161347.png]]

# Impact

- I demoed the tool to the entire team, and they approved it. It is now in production.
- A 20-minute routine now takes 5 to 10 minutes, saving roughly 50% of the time and a good share of the mental effort.
- The team's daily log now builds itself. The CSV reports land in SharePoint through Power Automate, and Power Query combines them into a single table in the review sheet, so no one has to paste or reformat anything.

![[Pasted image 20261012161736.png]]

# Limitations and Future Work

- **The tool depends on an undocumented API.** Vendor APIs don't stay static forever. Today the tool relies on defensive error handling. As a next step, I plan to add strict schema validation so the pipeline fails loudly and clearly if the contract changes, instead of silently producing wrong reports.
- **Cookie refresh is still manual.** The paste step takes seconds, but it is the one part of the routine that needs a person every few hours. Automating it would require handling credentials, which I would only do with the client's and security team's approval.
- **The tool is tied to Windows and Outlook.** The email steps use Outlook through COM. Moving to a different mail system would need a small rewrite of those functions.
- **The filtering rules are hardcoded.** Excluded checks and asset classes live in the code. Moving them into a configuration file would let the team adjust them without editing Python.
