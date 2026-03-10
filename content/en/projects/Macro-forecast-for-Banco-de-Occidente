---
title: "Macro Forecasting Challenge — Banco de Occidente"
date: 2026-03-09
draft: false
tags: ["python", "econometrics", "time-series", "finance", "colombia"]
summary: "A full forecasting pipeline to compete in a national macroeconomic tournament: data extraction from 6 official sources, SARIMA/AR models, and walk-forward backtesting."
---

## Overview

A end-to-end macroeconomic forecasting system built to compete in the **Macro Forecasting Challenge**, a national university tournament organized by Banco de Occidente and Occieconómicas. The pipeline forecasts 10 financial and macroeconomic variables for Colombia and international markets, evaluated against realized data across three monthly submission rounds.

**Repository:** [github.com/RedFoxRising/macro-forecasting-occidente](https://github.com/RedFoxRising/macro-forecasting-occidente)  
**Competition:** Banco de Occidente × Occieconómicas — March to May 2026

## The Problem

The challenge required forecasting 10 variables simultaneously, each from a different official source, at different frequencies, and with different statistical behaviors. A single modeling approach wouldn't work — a method suited for an equity index is wrong for inflation, and vice versa. The real problem was building a unified pipeline that could handle all of them cleanly while being fast enough to update and re-submit every month.

## Tech Stack

- **Language:** Python (Google Colab)
- **Data:** `yfinance`, `requests` (Datos Abiertos API), DANE Excel files, BanRep SUAMECA, Investing.com CSV
- **Modeling:** `statsmodels` (SARIMAX, AutoReg), `scikit-learn` (LinearRegression)
- **Processing:** `pandas`, `numpy`
- **Visualization:** `matplotlib`

## Variables Forecasted

| Variable | Weight | Model Used |
|---|---|---|
| Colombian Inflation (monthly %) | **15%** | SARIMA(1,1,1)(1,1,1,12) |
| USD/COP Exchange Rate (TRM) | 10% | Random Walk + drift |
| ISE Economic Tracking Index | 10% | AR(2) |
| 10-year TES bond yield | 10% | Linear regression vs UST10Y |
| Colcap equity index | 10% | Random Walk + drift |
| S&P 500 | 10% | Random Walk + drift |
| Monetary Policy Rate | 10% | Heuristic + manual adjustment |
| National Unemployment Rate | 10% | SARIMA(1,1,0)(0,1,1,12) |
| Brent Crude Oil | 5% | Random Walk + drift |
| Gold | 5% | Random Walk + drift |

## Key Features

- ✅ Automated data extraction from 6 official sources via API and Excel parsing
- ✅ Unified `df_master` — 10 variables aligned to monthly frequency from 2018
- ✅ Model per variable matched to its statistical behavior
- ✅ Walk-forward backtesting over 24 months (no data leakage)
- ✅ Simulated competition score to benchmark performance before submitting
- ✅ Manual adjustment layer to incorporate qualitative context

## What I Learned

### Parsing Real-World Official Data
Government data sources are not clean. The DANE inflation file uses a wide format (years as columns, months as rows) that needs to be melted before it becomes usable. The BanRep Excel has logos and footnotes above the actual data. The ISE file embeds multiple tables in a single sheet. Each source required its own extraction function with format-specific logic.

### Frequency Alignment
Merging daily series (TRM, S&P 500) with monthly series (Inflation, Unemployment) into a single DataFrame required a clear convention: `resample('BME').last()` to extract the last business day of each month for market variables, and first-of-month indexing for flow variables. Getting this wrong silently produces misaligned data that corrupts every downstream model.

### Honest Backtesting
The right way to validate a time-series model is walk-forward validation — training only on data available before each test month and never touching future observations. A standard train/test split would leak future information into the training window and produce artificially good results. The 24-month walk-forward loop revealed which models were genuinely predictive and which ones just looked good in-sample.

### When Simple Beats Complex
For the five market price variables, a drift-adjusted Random Walk consistently matched or outperformed more sophisticated models in the backtesting. This is the expected result in efficient markets at a 1-month horizon and an important reminder that model complexity is not the same as model quality.

## Challenges

**Challenge 1: Unstable Error Metric Near Zero**  
The competition's formula (`|P-O|/O × 100`) explodes when the observed value is close to zero. For the ISE (which was ~0.13% in April 2025), a 0.5 pp absolute error produced a 380% relative error — making the backtesting results unreadable. I solved this by using `max(|O|, ε)` as the denominator, where `ε` is the 25th percentile of the absolute historical values for each variable. This stabilizes the metric while preserving its behavior for normal observations.

**Challenge 2: Outliers in the TES Data**  
The Investing.com CSV for the 10-year TES bond contained a clear digitization error producing an implausible yield above 20%. I detected it programmatically (values above a threshold that is historically impossible for Colombia) and replaced it with a linear interpolation between adjacent observations.

**Challenge 3: ISE Extraction from a Complex Excel**  
The DANE ISE file embeds multiple indicator tables in a single sheet, with years spread across columns that include provisional labels like `2024p`. The extractor uses pattern-matching to find the ISE total row by indicator name, then dynamically detects year columns by checking if the first 4 characters are digits — making it robust to DANE's changing file formats.

## Results

- 🎯 Round 1 submitted (March 2026) — results pending
- 📊 Backtesting average weighted error: ~4.2% across 24 months
- 📈 Model outperformed pure Random Walk benchmark on 6 of 10 variables
- 💡 Inflation SARIMA reduced MAE by ~30% vs. naive seasonal average

*This section will be updated with realized errors after each round.*

## Future Improvements

- Add SIPSA food price index as external regressor for Inflation (SARIMAX)
- Add XM energy consumption data as proxy for ISE
- Build a scenario analysis module (bearish / base / bullish) with probability weighting
- Automate data refresh so the pipeline runs without manual file downloads

## Takeaways

The hardest part of this project was not the modeling — it was getting clean, aligned data. Every hour spent on a robust extraction function saved time in every subsequent month. The second key lesson: always backtest with walk-forward before trusting a model. Two variables that looked well-fit in-sample turned out to be no better than a random walk when tested honestly. Knowing that early meant not relying on them in the actual submission.
