---
title: "Prediction & Forecasting"
draft: false
tags: ["AI", "ML", "Stats", "Prediction", "Forecasting", "Regression", "Time Series"]
categories: ["AI", "ML", "Stats"]
weight: 500
menu: main
---

# Prediction & Forecasting

Prediction and forecasting use statistical models to estimate unknown or future values.

In this module, the focus is on correlation, regression, and time series forecasting.

{{% hint info %}}
**Key takeaway:**  
Prediction estimates a value using a model.

Forecasting is prediction where the order of time matters.
{{% /hint %}}

- Correlation
- Regression
- Time series analysis
- Components of time series data
- Moving average and weighted moving average
- AR model
- ARMA model
- ARIMA model
- SARIMA and SARIMAX
- VAR and VARMAX
- Simple exponential smoothing

---

## Prediction vs Forecasting ☆

| Concept | Meaning | Example |
|---|---|---|
| Prediction | Estimate an unknown output | Predict house price from area and rooms |
| Forecasting | Predict future values using time order | Forecast sales for next month |

{{% colour "red" %}}All forecasting is prediction, but not all prediction is forecasting.{{% /colour %}}

---

## Overall Workflow

{{< mermaid >}}
flowchart LR
    A[Data] --> B[Explore Pattern]
    B --> C[Choose Model]
    C --> D[Train or Fit]
    D --> E[Validate]
    E --> F[Predict or Forecast]
    F --> G[Interpret Error]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#C8E6C9
    style F fill:#E1F5FE
    style G fill:#FFF9C4
{{< /mermaid >}}

---

## Correlation ☆

Correlation measures the direction and strength of linear relationship between two variables.

The Pearson correlation coefficient is:

{{% colour "red" %}}
{{< katex display=true >}}
r = \frac{\sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^{n}(x_i - \bar{x})^2}\sqrt{\sum_{i=1}^{n}(y_i - \bar{y})^2}}
{{< /katex >}}
{{% /colour %}}

Range:

{{% colour "red" %}}
{{< katex display=true >}}
-1 \leq r \leq 1
{{< /katex >}}
{{% /colour %}}

| Value of correlation | Interpretation |
|---|---|
| Close to 1 | Strong positive linear relationship |
| Close to -1 | Strong negative linear relationship |
| Close to 0 | Weak or no linear relationship |

{{% hint warning %}}
Correlation does not prove causation.

Two variables may move together because of a third hidden factor.
{{% /hint %}}

---

## Regression ☆

Regression models the relationship between an input variable and an output variable.

Simple linear regression has the form:

{{% colour "red" %}}
{{< katex display=true >}}
\hat{y} = b_0 + b_1x
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} \hat{y} {{< /katex >}} is predicted value
- {{< katex >}} b_0 {{< /katex >}} is intercept
- {{< katex >}} b_1 {{< /katex >}} is slope
- {{< katex >}} x {{< /katex >}} is input variable

### Regression Residual

A residual is the difference between actual and predicted value.

{{% colour "red" %}}
{{< katex display=true >}}
e_i = y_i - \hat{y}_i
{{< /katex >}}
{{% /colour %}}

---

## Least Squares Method ☆

Linear regression commonly estimates parameters by minimising the sum of squared errors.

{{% colour "red" %}}
{{< katex display=true >}}
SSE = \sum_{i=1}^{n}(y_i - \hat{y}_i)^2
{{< /katex >}}
{{% /colour %}}

The best-fit line is the line that minimises this quantity.

### Slope and Intercept

{{% colour "red" %}}
{{< katex display=true >}}
b_1 = \frac{\sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=1}^{n}(x_i - \bar{x})^2}
{{< /katex >}}
{{% /colour %}}

{{% colour "red" %}}
{{< katex display=true >}}
b_0 = \bar{y} - b_1\bar{x}
{{< /katex >}}
{{% /colour %}}

---

## Model Evaluation for Regression ☆

### Mean Absolute Error

{{% colour "red" %}}
{{< katex display=true >}}
MAE = \frac{1}{n}\sum_{i=1}^{n}|y_i - \hat{y}_i|
{{< /katex >}}
{{% /colour %}}

### Mean Squared Error

{{% colour "red" %}}
{{< katex display=true >}}
MSE = \frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y}_i)^2
{{< /katex >}}
{{% /colour %}}

### Root Mean Squared Error

{{% colour "red" %}}
{{< katex display=true >}}
RMSE = \sqrt{\frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y}_i)^2}
{{< /katex >}}
{{% /colour %}}

### Coefficient of Determination

{{% colour "red" %}}
{{< katex display=true >}}
R^2 = 1 - \frac{\sum_{i=1}^{n}(y_i - \hat{y}_i)^2}{\sum_{i=1}^{n}(y_i - \bar{y})^2}
{{< /katex >}}
{{% /colour %}}

{{% hint info %}}
A higher {{< katex >}} R^2 {{< /katex >}} means the regression model explains more variation in the output variable.
{{% /hint %}}

---

## Time Series Analysis ☆

A time series is a sequence of observations collected over time.

Examples:

- daily stock price
- monthly sales
- hourly temperature
- weekly website traffic

The order of observations matters.

{{% hint warning %}}
Do not randomly shuffle time series data before forecasting.

Shuffling destroys the temporal pattern.
{{% /hint %}}

---

## Components of Time Series Data ☆

| Component | Meaning | Example |
|---|---|---|
| Trend | Long-term increase or decrease | Sales rising over years |
| Seasonality | Repeating pattern at fixed intervals | Higher sales every December |
| Cyclic pattern | Long-term waves without fixed period | Business cycles |
| Irregular noise | Random variation | Unexpected shocks |

---

## Moving Average ☆

Moving average smooths short-term fluctuations.

For a window of size {{< katex >}} m {{< /katex >}}:

{{% colour "red" %}}
{{< katex display=true >}}
MA_t = \frac{Y_t + Y_{t-1} + \cdots + Y_{t-m+1}}{m}
{{< /katex >}}
{{% /colour %}}

### Weighted Moving Average

Weighted moving average gives different importance to different time points.

{{% colour "red" %}}
{{< katex display=true >}}
WMA_t = \sum_{i=0}^{m-1} w_iY_{t-i}
{{< /katex >}}
{{% /colour %}}

where:

{{% colour "red" %}}
{{< katex display=true >}}
\sum_{i=0}^{m-1} w_i = 1
{{< /katex >}}
{{% /colour %}}

---

## Simple Exponential Smoothing ☆

Simple exponential smoothing updates the forecast using the latest observation and previous forecast.

{{% colour "red" %}}
{{< katex display=true >}}
F_{t+1} = \alpha Y_t + (1 - \alpha)F_t
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} F_{t+1} {{< /katex >}} is next forecast
- {{< katex >}} Y_t {{< /katex >}} is current observation
- {{< katex >}} F_t {{< /katex >}} is current forecast
- {{< katex >}} \alpha {{< /katex >}} is smoothing constant

{{% colour "red" %}}
{{< katex display=true >}}
0 < \alpha < 1
{{< /katex >}}
{{% /colour %}}

---

## Stationarity ☆

A stationary time series has statistical properties that do not change strongly over time.

Commonly, stationarity means approximately stable:

- mean
- variance
- autocorrelation pattern

Many AR, MA and ARMA models assume stationarity.

---

## Differencing ☆

Differencing is used to reduce trend and make a time series more stationary.

{{% colour "red" %}}
{{< katex display=true >}}
Z_t = Y_t - Y_{t-1}
{{< /katex >}}
{{% /colour %}}

If the first difference is not enough, higher-order differencing may be used.

---

## Autocorrelation and Partial Autocorrelation ☆

Autocorrelation measures how a time series relates to its own past values.

{{% colour "red" %}}
{{< katex display=true >}}
\rho_k = \text{Corr}(Y_t, Y_{t-k})
{{< /katex >}}
{{% /colour %}}

| Tool | Helps Identify |
|---|---|
| ACF | Moving average order |
| PACF | Autoregressive order |
| AIC and BIC | Model selection among candidates |

---

## AR Model ☆

An autoregressive model predicts the current value using previous values of the same series.

AR(1):

{{% colour "red" %}}
{{< katex display=true >}}
Y_t = \phi_0 + \phi_1Y_{t-1} + \epsilon_t
{{< /katex >}}
{{% /colour %}}

AR(p):

{{% colour "red" %}}
{{< katex display=true >}}
Y_t = \phi_0 + \phi_1Y_{t-1} + \phi_2Y_{t-2} + \cdots + \phi_pY_{t-p} + \epsilon_t
{{< /katex >}}
{{% /colour %}}

---

## MA Model ☆

A moving average model predicts the current value using current and past error terms.

MA(1):

{{% colour "red" %}}
{{< katex display=true >}}
Y_t = \mu + \epsilon_t + \theta_1\epsilon_{t-1}
{{< /katex >}}
{{% /colour %}}

MA(q):

{{% colour "red" %}}
{{< katex display=true >}}
Y_t = \mu + \epsilon_t + \theta_1\epsilon_{t-1} + \cdots + \theta_q\epsilon_{t-q}
{{< /katex >}}
{{% /colour %}}

---

## ARMA Model ☆

ARMA combines autoregressive and moving average components.

ARMA(1,1):

{{% colour "red" %}}
{{< katex display=true >}}
Y_t = \phi_0 + \phi_1Y_{t-1} + \theta_1\epsilon_{t-1} + \epsilon_t
{{< /katex >}}
{{% /colour %}}

ARMA is normally used for stationary series.

---

## ARIMA Model ☆

ARIMA stands for **Autoregressive Integrated Moving Average**.

It combines:

- AR: past values
- I: differencing to make the series stationary
- MA: past forecast errors

ARIMA is written as:

{{% colour "red" %}}
{{< katex display=true >}}
ARIMA(p,d,q)
{{< /katex >}}
{{% /colour %}}

Where:

- {{< katex >}} p {{< /katex >}} is AR order
- {{< katex >}} d {{< /katex >}} is differencing order
- {{< katex >}} q {{< /katex >}} is MA order

---

## SARIMA and SARIMAX ☆

SARIMA extends ARIMA by modelling seasonality.

{{% colour "red" %}}
{{< katex display=true >}}
SARIMA(p,d,q)(P,D,Q)_s
{{< /katex >}}
{{% /colour %}}

SARIMAX further includes external variables.

Example external variables:

- holiday indicator
- marketing spend
- temperature
- interest rate

---

## VAR and VARMAX ☆

VAR is used for multivariate time series.

Each variable can depend on its own past values and the past values of other variables.

VARMAX extends VAR by adding moving-average terms and exogenous variables.

| Model | Use Case |
|---|---|
| AR | One series, depends on its own past |
| MA | One series, depends on past errors |
| ARMA | Stationary series with AR and MA behaviour |
| ARIMA | Non-stationary series that needs differencing |
| SARIMA | Seasonal univariate forecasting |
| SARIMAX | Seasonal forecasting with external variables |
| VAR | Multiple interacting time series |
| VARMAX | Multiple series with errors and external variables |

---

## Exam-Oriented Model Selection ☆

{{< mermaid >}}
flowchart TD
    A[Time Series Data] --> B{Stationary?}
    B -- Yes --> C{AR or MA pattern?}
    C -- AR only --> D[AR]
    C -- MA only --> E[MA]
    C -- Both --> F[ARMA]
    B -- No --> G[Differencing]
    G --> H[ARIMA]
    H --> I{Seasonality?}
    I -- Yes --> J[SARIMA]
    I -- No --> K[ARIMA]
    J --> L{External variables?}
    L -- Yes --> M[SARIMAX]
    L -- No --> J
    A --> N{Multiple variables?}
    N -- Yes --> O[VAR or VARMAX]

    style A fill:#E1F5FE
    style B fill:#FFF9C4
    style C fill:#FFF9C4
    style D fill:#C8E6C9
    style E fill:#C8E6C9
    style F fill:#EDE7F6
    style G fill:#E1F5FE
    style H fill:#C8E6C9
    style I fill:#FFF9C4
    style J fill:#EDE7F6
    style K fill:#C8E6C9
    style L fill:#FFF9C4
    style M fill:#E1F5FE
    style N fill:#FFF9C4
    style O fill:#C8E6C9
{{< /mermaid >}}

---

## Why It Matters in AI and ML

Prediction and forecasting are core ML tasks.

They are used in:

- demand forecasting
- stock and finance modelling
- energy usage prediction
- anomaly detection
- recommendation systems
- risk modelling
- sensor and IoT monitoring

{{% hint success %}}
Regression predicts from relationships between variables.

Time series forecasting predicts from temporal patterns.
{{% /hint %}}

---

## Revision Checklist ☆

- Can I distinguish prediction from forecasting?
- Can I explain correlation without claiming causation?
- Can I write the simple linear regression equation?
- Can I calculate residuals and interpret error metrics?
- Can I identify trend, seasonality and noise?
- Can I explain AR, MA, ARMA and ARIMA?
- Can I choose SARIMA when seasonality is present?
- Can I choose VAR when multiple time series interact?

---
{{< home-link "Home" >}} | {{< section-index >}}
