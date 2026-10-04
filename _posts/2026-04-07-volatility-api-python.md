---
layout: post
title: "Volatility Forecasting API: Historical Stock Volatility Forecasting with GARCH and FastAPI"
date: 2026-04-06 10:00:00 -0500
categories: [Projects]
tags: [api, finance, python, FastAPI, time series analysis]
math: true
---

In this project, I created a Python program that pulls historical stock prices data from Twelve Data API, stores it in a SQLite database, and trains a GARCH model to predict volatility. The program is deployed as a RESTful API via FastAPI.


> You can find the source code and documentation for this project on [GitHub](https://github.com/malabaclado/predicting-stock-volatility-using-Python). 
{: .prompt-info }

## Project Overview

Technologies used:
- Backend: FastAPI/Python
- Data Source: Twelve Data API
- Database: SQLite
- Model: GARCH
- Concepts: Time-series analysis, RESTful API design, CRUD operations.

<!---

In finance, **volatility** is a statistical measure of the dispersion of returns for a given security or market index. It represents the degree to which an asset's price fluctuates over time. Mathematically, it is most often expressed as the standard deviation ($\sigma$) of logarithmic returns, calculated as:

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



### Time series methods for predicting volatility - GARCH models

**GARCH**, which stands for Generalized Autoregressive Conditional Heteroskedasticity, is a statistical model used to estimate and forecast the volatility of time series data. While standard financial models often assume that the "spread" or variance of returns is constant over time, GARCH recognizes that volatility changes and often "clusters" together.

To understand GARCH, it helps to break down the technical terms:

- **Autoregressive**: The current value is dependent on its own previous values.
- **Conditional**: The variance depends on the recent past.
- **Heteroskedasticity**: A fancy way of saying "changing variance" or "varying volatility."

The primary strength of GARCH is its ability to capture volatility clustering—the empirical observation in financial markets where large price swings tend to be followed by more large swings, and calm periods tend to be followed by more calm.

This project uses **GARCH(p,q)** models where p and q are parameters defined in the API.

-->
### High-Level Project Overview

The project follows a modular, layered architecture with a clear separation of concerns across configuration, data access, domain modeling, numerical computation, schema validation, and presentation (HTTP API).

```
                                  ┌────────────────────────┐
                                  │   HTTP Request (REST)  │
                                  └───────────┬────────────┘
                                              │
                                              ▼
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ 1. Presentation & Routing Layer (`main.py`)                                            │
│    • Centralized structured logging (`logging.basicConfig`)                            │
│    • /diagnostics/check    • /model/search    • /models/fit    • /models/forecast      │
└────────┬────────────────────────────┬───────────────────────────────┬──────────────────┘
         │                            │                               │
         ▼                            ▼                               ▼
┌─────────────────────────┐  ┌─────────────────────────┐  ┌─────────────────────────┐
│ 2. Schema Validation    │  │ 3. Domain Model Layer   │  │ 4. Mathematical Engine  │
│    (`src/schemas.py`)   │  │    (`src/model.py`)     │  │    (`src/math_helper.py`)│
│ • Pydantic V2 Models    │  │ • GarchModel lifecycle  │  │ • Student-t quantiles   │
│ • Enums & Bounds        │  │ • arch_model calibration│  │ • Expected Shortfall(ES)│
│ • Field/Model Validators│  │ • Artifact dump/load    │  │ • Volatility aggregation│
│ • Ticker Sanitization   │  │ • Model Registry Search │  │ • Regime classification │
└─────────────────────────┘  └────────────┬────────────┘  └─────────────────────────┘
                                          │
                                          ▼
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ 5. Data Access & Ingestion Layer (`src/data.py`, `config.py`)                          │
│    • TwelveDataAPI: External daily equity data retrieval via REST                      │
│    • SQLRepository: Generic SQLite CRUD operations                                     │
│    • Timezone-aware NYSE EOD schedule checks (America/New_York)                        │
└────────────────────────┬────────────────────────────────┬──────────────────────────────┘
                         │                                │
                         ▼                                ▼
         ┌───────────────────────────────┐ ┌───────────────────────────────┐
         │ `market_data.sqlite`          │ │ `models.sqlite` & `models/`   │
         │ (Cached historical OHLCV data)│ │ (Model registry & artifacts)  │
         └───────────────────────────────┘ └───────────────────────────────┘
```

### Layered Project Structure

```
predicting-stock-volatility-using-Python/
│
├── config.py                     # Centralized Pydantic Settings & environment variable configuration
├── main.py                       # FastAPI entrypoint, routes, diagnostics, and structured logging
│
├── src/                          # Core application engine
│   ├── data.py                   # Market data acquisition & SQLite caching layer
│   │   ├── TwelveDataAPI         # REST client for external market data extraction
│   │   │   └── fetch_data_from_api()         # Retrieves historical daily OHLCV prices
│   │   ├── SQLRepository         # SQLite persistence and query abstraction
│   │   │   ├── insert_table()                # Writes/updates time-series records in SQLite
│   │   │   └── read_table()                  # Reads filtered historical prices by date range
│   │   ├── get_latest_expected_eod()         # Market-close & exchange timezone-aware date resolver
│   │   └── get_start_date()                  # Computes lookback window start dates (1y, 3y, 5y)
│   │
│   ├── math_helper.py            # Quantitative analysis & analytical tail-risk engine
│   │   ├── compute_distribution_quantiles()  # VaR & closed-form ES factors (Normal & Student's t)
│   │   ├── calculate_risk_metrics_for_horizon() # Horizon-level VaR and Expected Shortfall calculations
│   │   ├── get_volatility_summary()          # 1-day conditional vol, annualized vol, and trend classification
│   │   ├── get_horizon_forecasts()           # Multi-period variance aggregation term structure
│   │   └── advance_business_days()           # Forward calendar date projection (skips weekends)
│   │
│   └── model.py                  # Econometric modeling & model registry operations
│       ├── GarchModel            # Core domain model managing calibration, forecast, and persistence
│       │   ├── get_daily_returns()           # Double-ended cache validation, API fallback & log returns
│       │   ├── fit()                         # Calibrates zero-mean GARCH(p, q) process via arch library
│       │   ├── predict_volatility()          # Generates multi-horizon conditional variance predictions
│       │   ├── dump()                        # Serializes trained model artifact to timestamped .pkl
│       │   └── load()                        # Deserializes model artifact by name or 'latest'
│       ├── build_model()         # Factory function initializing GarchModel with SQL repository
│       ├── filter_saved_models() # Queries & filters model registry by performance benchmarks
│       ├── read_models_table()   # Reads cataloged model records from SQLite
│       └── save_model_to_db()    # Records model hyperparameters, AIC/BIC, and persistence in SQLite
│
├── market_data.sqlite            # Local SQLite database caching historical equity price time series
├── models.sqlite                 # SQLite database cataloging fitted model metadata & audit history
└── models/                       # Directory storing serialized GARCH model artifacts (.pkl files)
```




<!---

> The following sections will discuss the inner workings of the program. Skip over to **Installation and Demo** section for setting up the program.
{: .prompt-info }

### The data layer

The data layer (`data.py`) isolates all data ingestion and persistence logic from the core business logic. By modularizing the data layer, the application is highly maintainable. It consists of two object classes: `TwelveDataAPI` and `SQLRepository`.

#### The TwelveDataAPI class

The `TwelveDataAPI` class handles external HTTP requests, data wrangling, and converting raw JSON into clean Pandas dataframes.


##### get_daily() function

The `get_daily()` function sends a `get` request to Twelve Data API, converts the `.json` response to a DataFrame, formats it, and returns the DataFrame. The function takes a ticker and the number of days. 

```python
def get_daily(self, ticker, output_size=90, interval="1day"):
    
        """Get daily time series of an equity from Twelve Data API.

        Parameters
        ----------
        ticker : str
            The ticker symbol of the equity.
        output_size : int, optional
            Number of observations to retrieve. "compact" returns the
            latest 100 observations. "full" returns all observations for
            equity. By default "full".
        interval : str, optional
            Time interval between observations. By default "1day".

        Returns
        -------
        pd.DataFrame
            Columns are 'open', 'high', 'low', 'close', and 'volume'.
            All columns are numeric.
        """
        url = (
            "https://api.twelvedata.com/time_series?"
            f"symbol={ticker}&"
            f"interval={interval}&"
            f"outputsize={output_size}&"
            f"apikey={self.__api_key}"
        )

        # Send request to API
        response = requests.get(url)
        response_data = response.json() #returns a json with two keys: meta and values

        # Error handling: if API call was unsuccessful, raise exception with error message
        if "meta" not in response_data.keys():
            error_msg = response_data['message']
            raise Exception(
                f"Invalid API call. Error message: {error_msg}"
            )

        # Convert API response to DataFrame
        df = pd.DataFrame(response_data['values'])

        # Set 'datetime' column as index
        df.set_index('datetime', inplace=True)
        df.index = pd.to_datetime(df.index)
        df.index.name = "date"

        # Convert 'open', 'high', 'low', 'close' to float 
        df[['open', 'high', 'low', 'close']] = df[['open', 'high', 'low', 'close']].astype(float) 

        # Convert 'volume' to integer
        df['volume'] = df['volume'].astype(int) 

        # Return results
        return df
```


#### The SQLRepository class
The `SQLReporitory` class handles CRUD operations with the SQLite database, acting as local cache to bypass API rate limits and speed up model training. 


#####  `insert_table()` function
The `insert_table` function writes the data into the SQLite database.

```py
def insert_table(self, table_name, records, if_exists="replace"):
    
        """Insert DataFrame into SQLite database as table

        Parameters
        ----------
        table_name : str
        records : pd.DataFrame
        if_exists : str, optional
            How to behave if the table already exists.

            - 'fail': Raise a ValueError.
            - 'replace': Drop the table before inserting new values.
            - 'append': Insert new values to the existing table.

            Dafault: 'fail'

        Returns
        -------
        dict
            Dictionary has two keys:

            - 'transaction_successful', followed by bool
            - 'records_inserted', followed by int
        """
        
        n_inserted = records.to_sql(name=table_name, con=self.connection, if_exists=if_exists)
        
        return {
            'transaction_successful':True, 'records_inserted':n_inserted
        }
```



### The model layer

The primary purpose of the model layer (`model.py`) is to encapsulate the entire lifecycle of the statistical model. It consists of the `GarchModel` object class.

### The main program

The `main.py` runs the application and uses FastAPI to deploy the model. It uses the previously mentioned object classes from `data.py` and `model.py`.

--->
### Installation & Setup

> Please see detailed instructions on how to run this project on [GitHub](https://github.com/malabaclado/predicting-stock-volatility-using-Python). 
{: .prompt-info }

### API Guide

#### ``

<!---
## Challenges and Lessons
- Data validation is important -> Pydantic schemas
- Model and Field validation -> FastAPI

--->
## Limitations
- Models assume zero-mean.
- Only supports GARCH models.
- Error distribution is only normal and student’s t distribution
- Value-at-Risk calculation method is parametric only.

## Future Enhancements
* Integrate a frontend dashboard using Streamlit or Dash.
* Include support for other ARCH model types, error distribution, and value-at-risk calculation methods.
