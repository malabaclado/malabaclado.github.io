---
layout: post
title: "Generalized ARCH models"
categories: [Blog]
tags: [time series analysis]
math: true
---

**GARCH**, which stands for Generalized Autoregressive Conditional Heteroskedasticity, is a statistical model used to estimate and forecast the volatility of time series data. While standard financial models often assume that the "spread" or variance of returns is constant over time, GARCH recognizes that volatility changes and often "clusters" together.

## Core Concepts

To understand GARCH, it helps to break down the technical terms:

- **Autoregressive**: The current value is dependent on its own previous values.
- **Conditional**: The variance depends on the recent past.
- **Heteroskedasticity**: A fancy way of saying "changing variance" or "varying volatility."

## The GARCH(1,1) Model

The most common version is the $GARCH(1,1)$ model. It predicts the current variance ($\sigma_t^2$) using three main components:

$$\sigma_t^2 = \omega + \alpha \epsilon_{t-1}^2 + \beta \sigma_{t-1}^2$$


Where:

- $\sigma_t^2$: The forecast of the variance for the current period.

- $\omega$ (Omega): The long-term average variance (the "baseline").

- $\alpha$ (Alpha): The "shock" component. It looks at the squared residual ($\epsilon_{t-1}^2$) from the previous period to see how much recent market shocks impact today's volatility.

- $\beta$ (Beta): The "persistence" component. It looks at the previous period's predicted variance ($\sigma_{t-1}^2$), representing how long volatility tends to linger.

  

### The GARCH(p,q) model

  

The **GARCH(p, q)** model (Generalized Autoregressive Conditional Heteroskedasticity) expands upon the standard GARCH(1,1) by allowing for multiple lags in both the squared residuals and the conditional variance.

  

The model consists of two main equations: the **Mean Equation** and the **Variance Equation**.

  

#### 1. The Mean Equation

The mean equation describes the observed return ($Y_t$) at time $t$ as a function of other variables plus an error term (innovation).

  

$$Y_t = \mu_t + \epsilon_t$$

  

Where:

* $\mu_t$: The conditional mean (often assumed to be a constant or an ARMA process).

* $\epsilon_t$: The residual/innovation, defined as $\epsilon_t = \sigma_t z_t$.

* $z_t$: A sequence of independent and identically distributed (i.i.d.) random variables with mean 0 and variance 1 (often following a Normal or Student's t-distribution).

  

---

  

#### 2. The Variance Equation (GARCH(p, q))

The variance equation defines the conditional variance ($\sigma_t^2$) based on $q$ lags of the squared residuals (the **ARCH** terms) and $p$ lags of the previous variances (the **GARCH** terms).

  

$$\sigma_t^2 = \omega + \sum_{i=1}^{q} \alpha_i \epsilon_{t-i}^2 + \sum_{j=1}^{p} \beta_j \sigma_{t-j}^2$$

  
  
  

**Parameters and Variables:**

* $\sigma_t^2$: The conditional variance at time $t$.

* $\omega$: The constant (baseline) variance.

* $\alpha_i$: The coefficients for the $q$ lagged squared residuals ($\epsilon_{t-i}^2$). These represent short-term "shocks" or news from previous periods.

* $\beta_j$: The coefficients for the $p$ lagged conditional variances ($\sigma_{t-j}^2$). These represent the "persistence" or memory of volatility.

  

---

  

#### 3. Necessary Constraints

For the GARCH(p, q) model to be statistically valid and stable (stationary), the following conditions must be met:

  

1.  **Positivity:** To ensures that the predicted variance is always positive:

    * $\omega > 0$

    * $\alpha_i \ge 0$ for $i = 1, \dots, q$

    * $\beta_j \ge 0$ for $j = 1, \dots, p$

  

2.  **Stationarity:** To ensure the variance does not "explode" and eventually reverts to a long-term mean:

    * $\sum_{i=1}^{q} \alpha_i + \sum_{j=1}^{p} \beta_j < 1$

  

---

  

#### Summary of Notation

* **$q$ (ARCH order):** The number of autoregressive squared residual terms.

* **$p$ (GARCH order):** The number of moving average conditional variance terms.

  

In practice, a **GARCH(1,1)** is often sufficient for most financial time series, as it captures the majority of volatility clustering without the complexity of estimating numerous parameters.