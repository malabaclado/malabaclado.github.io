# Title

{Short Summary}

{**Anonymization & Compliance Disclaimer:** A brief callout clarifying that all proprietary identifiers, endpoints, and credentials have been scrubbed, and the code runs against a mock local server/fixture data for demonstration purposes.}


# Architecture and Design

**Project Directory Structure**

**Key Technical Modules**
- main.py
- get_data
- write_data
- parse_data


**Why Direct API Ingestion Over Headless Scraping**
{Concise side-by-side comparison table (Compute overhead, latency, selector fragility vs. schema stability).}

# Production considerations
- Session reuse via persistent HTTP sessions (`requests.Session` / `httpx.Client`).
- Exponential backoff and retry policies (`urllib3.util.retry` / `tenacity`).


# Demo

**Prerequisites**
- Python
- Virtual environment

**Install dependencies**
{How to install Python dependencies}
```
py -m pip install -r requirements.txt
```

**Setting up the environment variables**
- Copy `.sample.env` to `.env` and fill out the details.
- Set up `config.py`

**Execution**
{How to run the program}

