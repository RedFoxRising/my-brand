---
title: "Forecasting the Future: My Strategy for the Banco de Occidente Macro Challenge"
date: 2026-02-21
draft: true
summary: "A deep dive into the predictive models and econometric strategies I am developing for a national macroeconomic forecasting tournament."
tags: ["Econometrics", "Forecasting", "Data Analysis", "Python", "Finance"]
---

## The Challenge

[cite_start]Currently, I am competing in the **Macro Forecasting Challenge**, a nationwide tournament organized by Banco de Occidente and Occieconómicas[cite: 34, 35, 36]. [cite_start]The objective is to evaluate analytical capabilities by forecasting 10 key international and Colombian macroeconomic and financial variables[cite: 39]. 

[cite_start]The stakes are high: the team with the lowest average relative error across three submission rounds will secure a 6-week summer internship at the bank's Trading Desk (Mesa de Dinero) and the Occieconómicas division[cite: 76, 92].

### The Target Variables

[cite_start]To win, my models need to accurately predict the following variables [cite: 46, 47, 48, 49, 50, 51, 52, 53, 54, 55][cite_start], which are weighted differently in the final score[cite: 88]:

* [cite_start]**High Impact (15%):** Colombian Inflation[cite: 48, 88].
* [cite_start]**Core Variables (10% each):** USD Exchange Rate (TRM), ISE (Economic Tracking Indicator), 10-year TES bonds, Colcap index, S&P500, Monetary Policy Rate, and the National Unemployment Rate[cite: 46, 47, 49, 50, 51, 54, 55, 88].
* [cite_start]**Commodities (5% each):** Brent Crude Oil and Gold prices[cite: 52, 53, 88].

---

## My Methodological Approach

*(Note: In this section, I will document my step-by-step process for building the forecasting models).*

### 1. Data Collection & Preprocessing
[cite_start]The challenge requires us to build our own databases from official sources like DANE, Banco de la República, and international exchanges[cite: 60, 61]. 
* **Tools used:** [Explain here if you are using Python (pandas, yfinance) or R to scrape/download the data via APIs].
* **Data Cleaning:** [Mention how you handle missing data, different frequencies (daily vs. monthly), or outliers].

### 2. Exploratory Data Analysis (EDA)
Before running any regressions, I analyzed the historical behavior of the series.
* [Insert a chart or graph here eventually showing the historical trend of the USD/COP or Inflation]
* **Key insights:** [e.g., "I noticed a strong seasonal component in the inflation data..."].

### 3. Model Selection & Strategy
[cite_start]Since the organizers allow any methodology[cite: 58], I am testing a combination of traditional econometrics and machine learning approaches:
* **Univariate Models:** For series with strong auto-correlated trends, I am testing [e.g., ARIMA / SARIMA models].
* **Multivariate Models:** To capture the relationship between the Monetary Policy Rate and Inflation, I am exploring [e.g., Vector Autoregression (VAR)].
* **Machine Learning:** As a benchmark, I am training a [e.g., Random Forest / XGBoost] model to see if it outperforms traditional econometrics on variables like the S&P500.

---

## Results & Iterations

[cite_start]The competition consists of three submission rounds (March, April, and May)[cite: 109, 111, 115]. I will be updating this section with my relative errors and adjustments as the official data is released.

### Round 1 (March Submission)
* **Status:** In progress.
* **Expected challenges:** Forecasting the exact closing price of the S&P500 and the exchange rate on the last business day of the month is highly susceptible to short-term market noise.

---

*I will continue to update this post as the competition progresses. Feel free to reach out if you want to discuss forecasting models or time-series econometrics!*
