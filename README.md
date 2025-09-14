# Time Series Forecasting for Stock Prices using ARIMA and LSTM

## Overview

This project explores **time series forecasting techniques** for predicting the **S&P 500 close price**. Both classical statistical models and deep learning methods were implemented and compared across short- and long-term forecast horizons. The goal was to evaluate how adaptiveness (rolling retraining) and forecast length impact predictive performance.

---

## Data

* Source: Historical S&P 500 daily prices (Open, High, Low, Close, Volume).  
* Period used: **1987 onwards** (to capture consistent trading activity).

---

## Exploratory Data Analysis & Feature Engineering

* Added **daily returns**.  
* Visualized price trends, returns distribution, and candlestick plots.  
* Applied **time series resampling** (weekly, monthly).  
* Engineered common **technical indicators**:
  * SMA (5, 15), EMA (5, 15)  
  * Relative Strength Index (RSI)  
  * Moving Average Convergence Divergence (MACD)

---

## Modeling Approach

### 1. ARIMA

* Differenced series to achieve stationarity.  
* Used **ACF/PACF** for lag order identification.  
* Applied **grid search with AIC** to select the best ARIMA model.
* Predicted the entire test period at once- obviously preformed abysmally
* Implemented **rolling forecasts** for adaptiveness - predicts 20 days ahead forecasts- model gets refitted every 20 days with the new actual data

### 2. LSTM Variants

* **Rolling univariate LSTM (1-day ahead)** → trained once- retuned every 20 days. predicts one day ahead forecast- the actual value then gets appended to the history- highly adaptive  
* **Univariate LSTM (20-day ahead)** → trained once- retuned every 40 days. predicts 20 days ahead forecast- the actual value then gets appended to the history- highly adaptive
* **Static multivariate LSTM** → trained once with additional features (Volume, RSI, MACD).

### 3. Residual Analysis

* Performed residual analysis for all the models discussed above- through distribution plot, histogram and acf plots - along with key summary statistics

---

## Results

| Model                      | Test RMSE | Error Std Dev | Mean Error |
| -------------------------- | --------: | ------------: | ---------: |
| **Rolling ARIMA (20-day)** | **83.27** |         83.06 |   **6.22** |
| **Rolling 1-day LSTM**     | **48.28** |     **44.28** |      19.26 |
| 20-day Univariate LSTM     |     87.86 |         85.00 |      22.35 |
| Static Multivariate LSTM   |    139.50 |         97.33 |      99.97 |

**Key Findings**

* **Best short-term (1-day)**: Rolling univariate LSTM (RMSE **48.28**).  
* **Best long-term (20-day)**: Rolling ARIMA (RMSE **83.27**) slightly outperformed LSTM (87.86).  
* **Adaptiveness > Features**: Rolling/retraining strategies consistently outperformed static models, even when the latter used more features.

---

## Conclusions

* LSTMs excel at **short-term forecasting** but lose accuracy over longer horizons.  
* ARIMA remains competitive for **longer-term forecasts**, especially with rolling retraining.  
* Adding more features (e.g., RSI, MACD) without model adaptiveness can harm performance.  
* **Adaptiveness is the key**: regularly updating models to incorporate new market information leads to more reliable predictions.

---

## Future Directions

* Explore **ARIMAX** and **multivariate LSTM** with exogenous macroeconomic features.  
* Compare against modern **Transformer-based time series models**.  
* Incorporate **probabilistic forecasts** (prediction intervals).  
* Test **ensemble methods** combining ARIMA and LSTM.
