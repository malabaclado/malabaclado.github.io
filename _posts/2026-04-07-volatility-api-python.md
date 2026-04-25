---
layout: post
title: "Volatility Forecasting API: Historical Stock Volatility Forecasting with GARCH and FastAPI"
date: 2026-04-06 10:00:00 -0500
categories: [Projects]
tags: [api, finance, python, FastAPI, time series analysis]
---

In this project, I created a program that pulls historical stock prices data from Twelve Data API, stores it in an SQLite database, and trains a ARCH model to predict volatility. The program is deployed as a RESTful API via FastAPI.


> You can find the source code and documentation for this project on [GitHub](https://github.com/malabaclado/predicting-stock-volatility-using-Python). 
{: .prompt-info }

## Project Overview

<!-- What is volatility and why is it important? -->
<!-- What is ARCH and how does it predict volatility? -->

Technologies used:
- Backend: FastAPI/Python
- Data Source: Twelve Data API
- Database: SQLite
- Model: ARCH
- Concepts: Time-series analysis, RESTful API design, CRUD operations.

## Program Structure

```
 predicting-stock-volatility/
 ├── data/                   # Directory for stocks.sqlite
 ├── models/                 # Saved .pkl files
 ├── main.py                 # main program
 ├── data.py                 # data layer
 ├── models.py               # model layer
 └── Dockerfile              # Renamed from dockerfile.txt
```

## Installation & Demo

1. Install docker
2. Clone the repo from Github.
3. Run `docker compose up`

## API Guide
### `POST /fit`

Description: Trains an ARCH model on historical data and saves it in the `/models` folder.

Parameters:
- `ticker`: A stock ticker symbol (e.g., "AAPL").
- `n_observations`: Number of past data points to use for training (integer).
- `p`, `q`: ARCH model parameters (integers).

Returns: A JSON message with the name of the trained model.

### `POST /predict`

Description: Uses a trained model to predict future volatility.

Parameters:
- `ticker`: A stock ticker symbol.
- `n_days`: Number of days to predict ahead (integer).
- `use_model`: Model to use ("latest" for the most recent model in `/models`).

Returns: Predicted volatility values as JSON.

## Future Enhancements
* Integrate a frontend dashboard using Streamlit or Dash.
