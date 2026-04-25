---
layout: post
title: "Multi-asset Portfolio VaR Calculation using Monte Carlo Method"
categories: [Projects]
tags: [excel, vba, monte carlo, risk management]
---

In this project, I created a VBA-based application on Excel to calculate the value at risk of a multi-asset portfolio using Monte Carlo method.

> You can find the Excel file and documentation for this project on [GitHub](https://github.com/malabaclado/multi-asset-portfolio-var-using-monte-carlo-simulation). 
{: .prompt-info }

## Project Overview

Value-at-Risk (VaR) is a widely used market risk measure used to calculate the expected possible loss on a particular investment for a certain period of time, given some probability. This measure is used by investment and commercial banks to determin the extent and probabilities of potential losses in their institutional portfolios.

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
### Button 1: Download data from API

```VBA
Private Sub CommandButton1_Click() Dim i As Integer For i = 0 To Worksheets(1).Cells(5, 3) - 1 Worksheets(2).Cells(1, 1 + (6 * i)) = Worksheets(1).Cells(8 + i, 2) Dim stock As String stock = Worksheets(2).Cells(1, 1 + (6 * i)) Worksheets(2).Cells(2, 1 + (6 * i)).Formula2 = _ "=AlphaVantage.EquityTimeSeries(""" & stock & """,""daily"")" Next End Sub
```

### Button 2: Calculate returns
```VBA
Private Sub CommandButton2_Click()
Dim FirstDay As String
Dim LastDay As String
Dim currentPrice As Double
Dim previousPrice As Double
Dim numberOfAssets As Integer

Dim returnRanges() As String
Dim weights As String
Dim assetExpectedReturns As String
Dim assetReturnSTD As String
Dim covMatrix As String
Dim totalInvestment As String
Dim calculatedNumberOfDays As String

numberOfAssets = Worksheets(1).Cells(5, 3)

'DELETE EXISTING DATA
Worksheets(3).Range(Worksheets(3).Cells(19, 2).Address & ":" & Worksheets(3).Cells(19 + 257, 4 + 30).Address).ClearContents

ReDim returnRanges(0 To numberOfAssets - 1)

For i = 0 To numberOfAssets - 1

'ADD ASSET TICKERS
Worksheets(3).Cells(19, 2 + i) = Worksheets(1).Cells(8 + i, 2)

For j = 1 To 251
currentPrice = Worksheets(2).Cells(2 + j + 1, 5 + (6 * i))
previousPrice = Worksheets(2).Cells(2 + j, 5 + (6 * i))

'Calculate returns
Worksheets(3).Cells(24 + j, 2 + i) = (currentPrice - previousPrice) / previousPrice

'Format as percentage
Worksheets(3).Cells(24 + j, 2 + i).NumberFormat = "0.00%"
Next

FirstDay = Worksheets(3).Cells(25, 2 + i).Address(False, False)
LastDay = Worksheets(3).Cells(25 + 251 - 1, 2 + i).Address(False, False)

'ADD AVERAGE()
Worksheets(3).Cells(20, 2 + i).Formula2 = "=AVERAGE(" & FirstDay & ":" & LastDay & ")* 251"

'Format as percentage
Worksheets(3).Cells(20, 2 + i).NumberFormat = "0.00%"

'ADD STD
Worksheets(3).Cells(21, 2 + i).Formula2 = "=STDEV.S(" & FirstDay & ":" & LastDay & ") * 252^0.5"

'Format as percentage
Worksheets(3).Cells(21, 2 + i).NumberFormat = "0.00%"

'ADD "Returns" HEADER
Worksheets(3).Cells(24, 2 + i) = "Returns " & i + 1

'ADD COVARIANCE MATRIX TABLE
Worksheets(3).Cells(19, 4 + numberOfAssets) = "Covariance"
Worksheets(3).Cells(19, 5 + numberOfAssets + i) = "Returns " & i + 1 'ADD COLUMN NAMES
Worksheets(3).Cells(20 + i, 4 + numberOfAssets) = "Returns " & i + 1 'ADD ROW NAMES

returnRanges(i) = FirstDay & ":" & LastDay
Next

For m = 0 To numberOfAssets - 1
For n = 0 To numberOfAssets - 1

If m = n Then
Worksheets(3).Cells(20 + m, 5 + numberOfAssets + n) = 0#
Else
Worksheets(3).Cells(20 + m, 5 + numberOfAssets + n).Formula2 = "=COVARIANCE.S(" & returnRanges(m) & ", " & returnRanges(n) & ")*252"
End If

Next
Next

'CALCULATE PORTFOLIO VARIANCE
weights = Worksheets(1).Cells(8, 3).Address & ":" & Worksheets(1).Cells(8 + numberOfAssets - 1, 3).Address
totalInvestment = Worksheets(1).Cells(4, 3).Address
calculatedNumberOfDays = Worksheets(1).Cells(3, 3).Address

assetExpectedReturns = Worksheets(3).Cells(20, 2).Address & ":" & Worksheets(3).Cells(20, 2 + numberOfAssets - 1).Address
assetReturnSTD = Worksheets(3).Cells(21, 2).Address & ":" & Worksheets(3).Cells(21, 2 + numberOfAssets - 1).Address
covMatrix = Worksheets(3).Cells(20, 5 + numberOfAssets).Address & ":" & Worksheets(3).Cells(20 + numberOfAssets - 1, 5 + 2 * numberOfAssets - 1).Address


Worksheets(3).Cells(14, 2).Formula2 = "=SUMPRODUCT('value-at-risk'!" & weights & "^2, TRANSPOSE(" & assetReturnSTD & "^2)) +SUM(MMULT(MMULT('value-at-risk'!" & weights & ",TRANSPOSE('value-at-risk'!" & weights & "))," & covMatrix & "))"

'CALCULATE PORTFOLIO EXPECTED RETURNS

Worksheets(3).Cells(16, 2).Formula2 = "='value-at-risk'!" & totalInvestment & "*SUMPRODUCT('value-at-risk'!" & weights & ", TRANSPOSE(" & assetExpectedReturns & ")) * ('value-at-risk'!" & calculatedNumberOfDays & "/252)"

End Sub
```

### Button 3: Simulate Monte Carlo Returns

```VBA
Private Sub CommandButton3_Click()

Worksheets(1).Cells(3, 22).Formula2 = "=NORM.S.INV(RAND())"
End Sub
```

## Demo Instructions
1. Configure the prelimenary parameters including the assets in the portfolio and the percent allocation for each asset, total amount of investment and the number of trading days to be considered.
2. Click the button "Download data from API".
3. Click the button "Calculate Returns"
4. Click the button "Simulate"
