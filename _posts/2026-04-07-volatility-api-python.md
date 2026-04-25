---
layout: post
title: "Volatility Forecasting API: Historical Stock Volatility Forecasting with GARCH and FastAPI"
date: 2026-04-06 10:00:00 -0500
categories: [Projects]
tags: [api, finance, python, FastAPI, time series analysis]
math: true
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


In finance, volatility is a statistical measure of the dispersion of returns for a given security or market index. It represents the degree to which an asset's price fluctuates over time. Mathematically, it is most often expressed as the standard deviation ($\sigma$) of logarithmic returns, calculated as:

$$\sigma = \sqrt{\frac{1}{N-1} \sum_{i=1}^{N} (R_i - \bar{R})^2}$$

Where:
- $R_i$ is the return in period $i$
- $\bar{R}$ is the average return
- $N$ is the number of periods

### Why Volatility Matters to Investors

While often viewed negatively as "risk," volatility is a multi-faceted tool for market participants:

- **Risk Assessment**: Volatility is the primary gauge of uncertainty. A highly volatile stock indicates a wider range of potential outcomes, requiring investors to determine if the potential "risk premium" (the extra return for holding a risky asset) justifies the price swings.
- **Portfolio Diversification**: By understanding the volatility of different assets, investors can combine them to reduce the overall "bumpiness" of their portfolio. Assets that aren't volatile in the same way (low correlation) help stabilize long-term returns.
- **Market Sentimen**t: Broad volatility indices, such as the VIX (CBOE Volatility Index), reflect the market's expectation of near-term price changes. High levels often signal "fear" or panic, while low levels suggest "greed" or complacency.
- **Price Discovery and Opportunity**: For active investors, volatility provides the price movement necessary to find entry and exit points. Without price fluctuations, there would be no opportunity to buy undervalued assets or sell overvalued ones.

### Time series methods for predicting volatility

**GARCH**, which stands for Generalized Autoregressive Conditional Heteroskedasticity, is a statistical model used to estimate and forecast the volatility of time series data. While standard financial models often assume that the "spread" or variance of returns is constant over time, GARCH recognizes that volatility changes and often "clusters" together.

#### Core Concepts

To understand GARCH, it helps to break down the technical terms:

- **Autoregressive**: The current value is dependent on its own previous values.
- **Conditional**: The variance depends on the recent past.
- **Heteroskedasticity**: A fancy way of saying "changing variance" or "varying volatility."

The primary strength of GARCH is its ability to capture volatility clustering—the empirical observation in financial markets where large price swings tend to be followed by more large swings, and calm periods tend to be followed by more calm.

This project uses **GARCH(p,q)** models where p and q are parameters defined in the API.



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

Description: Trains an GARCH model on historical data and saves it in the `/models` folder.

Parameters:
- `ticker`: A stock ticker symbol (e.g., "AAPL").
- `n_observations`: Number of past data points to use for training (integer).
- `p`, `q`: GARCH model parameters (integers).

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
