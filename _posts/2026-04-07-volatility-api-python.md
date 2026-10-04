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

<!-- What is volatility and why is it important? -->

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

<!-- What is ARCH and how does it predict volatility? -->

### Time series methods for predicting volatility - GARCH models

**GARCH**, which stands for Generalized Autoregressive Conditional Heteroskedasticity, is a statistical model used to estimate and forecast the volatility of time series data. While standard financial models often assume that the "spread" or variance of returns is constant over time, GARCH recognizes that volatility changes and often "clusters" together.

#### Core Concepts

To understand GARCH, it helps to break down the technical terms:

- **Autoregressive**: The current value is dependent on its own previous values.
- **Conditional**: The variance depends on the recent past.
- **Heteroskedasticity**: A fancy way of saying "changing variance" or "varying volatility."

The primary strength of GARCH is its ability to capture volatility clustering—the empirical observation in financial markets where large price swings tend to be followed by more large swings, and calm periods tend to be followed by more calm.

This project uses **GARCH(p,q)** models where p and q are parameters defined in the API.



## Project Structure

The project is separated into three layers: the main program, the data layer, and the model layer. The `/data` folder contains the SQLite files while the `/models` folder contains the saved GARCH models as `.pkl` files.

```
 predicting-stock-volatility/
 ├── data/                       # Directory for stocks.sqlite
 ├── models/                     # Saved .pkl files
 ├── main.py                     # main program
 ├── data.py                     # data layer
    └── TwelveDataAPI class
        └── get_daily()
    └── SQLRepository class
        ├── insert_table()
        └── read_table()
 ├── models.py                   # model layer
    └── GarchModel class
        ├── wrangle_data()
        ├── fit()
        ├── predict_volatility()
        ├── dump()
        └── load()
 └── Dockerfile                  # Renamed from dockerfile.txt
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
## Installation & API Guide

> Please see detailed instructions on how to run this project on [GitHub](https://github.com/malabaclado/predicting-stock-volatility-using-Python). 
{: .prompt-info }

## Future Enhancements
* Integrate a frontend dashboard using Streamlit or Dash.
