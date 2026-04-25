---
layout: post
title: "Multi-asset Portfolio VaR Calculation using Monte Carlo Method"
categories: [Projects]
tags: [excel, vba, monte carlo, risk management]
---

In this project, I created a VBA-based application on Excel to calculate the value at risk of a multi-asset portfolio using Monte Carlo method.

Value-at-Risk (VaR) is a widely used market risk measure used to calculate the expected possible loss on a particular investment for a certain period of time, given some probability. This measure is used by investment and commercial banks to determin the extent and probabilities of potential losses in their institutional portfolios.

This project aims to create an Excel application that pulls stock market data and calculate the Value-at-Risk of a multi-asset portfolio using Monte Carlo Simulation.

## Mathematics: Value-at-Risk Calculation using Monte Carlo Simulation
Steps in Calculating VaR using Monte Carlo Simulation

1. Calculate daily returns for each asset
2. Calculate annual expected returns and returns standard deviation
3. Calculate the covariance matrix of the returns
4. Calculate the portfolio variance and standard deviation
5. Calculate the portfolio expected returns
6. Simulate 10,000 cases
7. Calculate the Value-at-Risk by getting the percentile of all simulated case.

## VBA Codes
Click here to see the VBA codes written in the Excel file.

## Demo Instructions
Configure the prelimenary parameters including the assets in the portfolio and the percent allocation for each asset, total amount of investment and the number of trading days to be considered.
Click the button "Download data from API".
Click the button "Calculate Returns"
Click the button "Simulate"

The above steps would direct you to a VaR analysis of the multi-asset portfolio.