---
title: "Forecasting the Future: My Strategy for the Banco de Occidente Macro Challenge"
date: 2026-02-21
draft: false
summary: "How I built a full macroeconomic forecasting pipeline — from raw API data to walk-forward backtesting — to compete in a national university tournament."
tags: ["Econometrics", "Forecasting", "Data Analysis", "Python", "Finance", "Colombia"]
---

## The Challenge

I am currently competing in the **Macro Forecasting Challenge**, a national tournament organized by Banco de Occidente and their research division, Occieconómicas. The goal is straightforward in concept but demanding in practice: forecast 10 key macroeconomic and financial variables for Colombia and international markets, then compare your predictions against the realized data.

The competition runs over three submission rounds — March, April, and May — and the winning team is determined by the lowest average weighted relative error across all three. The prize for first place is a 6-week summer internship at the bank's Trading Desk (Mesa de Dinero) in Bogotá.

### The 10 Variables

The variables are weighted differently in the scoring formula, which directly shaped how I prioritized my modeling effort:

| Variable | Weight | Type |
|---|---|---|
| Colombian Inflation (monthly) | **15%** | Domestic macro |
| USD/COP Exchange Rate (TRM) | 10% | FX |
| ISE Economic Tracking Index | 10% | Domestic macro |
| 10-year TES bond yield (2036) | 10% | Fixed income |
| Colcap equity index | 10% | Equities |
| S&P 500 | 10% | Equities |
| Monetary Policy Rate | 10% | Domestic macro |
| National Unemployment Rate | 10% | Domestic macro |
| Brent Crude Oil | 5% | Commodities |
| Gold | 5% | Commodities |

The scoring formula is: `Error = |Forecast − Observed| / Observed × 100%`, averaged across variables weighted by their assigned importance.

---

## Part 1: Building the Data Pipeline

The first challenge was constructing a clean, unified dataset from scratch. Each variable lives in a different source, with a different format, frequency, and level of messiness. I built extraction functions for each one.

**International market variables** (S&P 500, Brent, Gold, Colcap) came from Yahoo Finance via `yfinance` — the cleanest source in the pipeline. I pulled daily closing prices from 2018 onward to capture both the pre-pandemic cycle and the post-2020 normalization.

**The TRM** (USD/COP exchange rate) was downloaded from Colombia's official open data API (`datos.gov.co`), which provides the daily rate certified by the Superintendencia Financiera — the same source the competition uses for the observed values.

**Banco de la República data** (Monetary Policy Rate) required parsing a non-standard Excel file from their SUAMECA portal, with logos and footnotes in the header rows. The extraction function handles this automatically with `skiprows` and a date-validation pass to strip out non-date rows.

**DANE data** (Inflation, ISE, Unemployment) presented the most complex parsing challenges. The inflation table uses a "wide" format where years are columns and months are rows — the function melts it into a standard long-format time series. The ISE file has multiple tables embedded in a single sheet, so the extractor searches for the specific row containing the ISE total by pattern-matching on the indicator name. The unemployment data (GEIH) has pandemic-era asterisks and mixed data types that required a robust cleaning pass before the series was usable.

**TES 10-year bond yields** came from Investing.com as a manually downloaded CSV. The raw data contained an outlier (likely a digitization error producing an implausible yield) that was detected programmatically and interpolated linearly.

---

## Part 2: Unifying into `df_master`

With 10 clean series in hand, the next step was aligning them into a single monthly DataFrame — `df_master`. This is less trivial than it sounds because the sources operate at different frequencies.

My solution: for daily series (TRM, S&P 500, etc.), I use `resample('BME').last()` to extract the value at the last business day of each month — which is exactly what the competition measures. For monthly flow variables (Inflation, ISE, Unemployment), I normalize the index to the first day of each month as a merge key.

The result is a 10-column DataFrame with a monthly DatetimeIndex from 2018 to the present, which serves as the base for all models.

---

## Part 3: Forecasting Models

I deliberately chose different model types for each variable, matching the statistical behavior of the series rather than applying one method uniformly.

**Random Walk with drift** for the five market price variables (TRM, S&P 500, Brent, Gold, Colcap). In efficient markets, the current price contains all available information about the future. For a 1-month horizon, a drift-adjusted random walk — where the drift is the average monthly change over the last 12 months — is a benchmark that is extremely hard to beat systematically. I tested this claim in the backtesting phase.

**Linear regression against the US Treasury 10-year yield** for the TES bond. Colombian sovereign yields move closely with US rates plus a time-varying spread. The model regresses the historical TES yield on the UST 10Y and uses the current US rate as the predictor for next month. The spread diagnostics are shown explicitly so I can adjust them manually if the credit risk picture changes.

**SARIMA(1,1,1)(1,1,1,12)** for Inflation. The monthly inflation series shows both a downward trend (Colombia's disinflation cycle since 2023) and strong seasonal patterns — January and December tend to be high-inflation months due to utility tariff adjustments and the holiday season. The seasonal ARIMA captures both components.

**AR(2)** for the ISE. The annual growth rate of economic activity has high month-to-month inertia — knowing the last two months is sufficient to get a reasonable one-step-ahead forecast. A parsimonious model is preferred here because the ISE series is shorter than the others (published with a 2-month lag).

**SARIMA(1,1,0)(0,1,1,12)** for Unemployment. Colombian unemployment has a very pronounced annual seasonal pattern, with peaks in January and July. The seasonal differencing and moving average term handle this well without overfitting.

**Heuristic rule** for the Monetary Policy Rate. The BanRep announces meeting dates well in advance, and the market prices in expected decisions through OIS rates. Rather than fitting a statistical model to an inherently discrete and calendar-driven variable, I use the last known rate and apply a manual adjustment based on the latest central bank communications before each submission.

---

## Part 4: Walk-Forward Backtesting

Before trusting any model with a real submission, I validated each one using **walk-forward validation** over the last 24 months of available data.

The key principle: for each test month `t`, the model is trained exclusively on data from `[0, t-1]`. It never sees the future. This is the only statistically honest way to evaluate a time-series forecasting model — any split that randomly assigns observations to train and test sets would leak future information into the training window.

For each variable, I computed two sets of errors: the model's error and the error of a pure random walk used as a benchmark. The comparison answers the question: *does my model actually add value, or am I overcomplicating things?*

One technical correction was necessary: the competition's error formula `|P-O|/O × 100` becomes unstable when the observed value is close to zero. For Inflation (~0.3%) and ISE (which can be near 0% during economic slowdowns), a perfectly reasonable absolute error of 0.15 percentage points produces a relative error that looks catastrophically large. I addressed this by using `max(|O|, ε)` as the denominator, where `ε` is the 25th percentile of the absolute historical values for each variable — this stabilizes the metric while preserving its behavior for normal-sized observations.

---

## Results and Next Steps

The backtesting revealed that the Random Walk is genuinely hard to beat for market prices at a 1-month horizon — which is the expected result and validates the model selection. The models with most room for improvement are Inflation and ISE, where incorporating external regressors (food price indices from DANE's SIPSA survey, energy consumption data from XM) should reduce the mean absolute error.

The most impactful next step before each submission is the **manual adjustment layer** — reviewing the BanRep's latest minutes, the SIPSA weekly food price data, and forward curves for oil before finalizing any number. The statistical models provide an unbiased anchor; the judgment layer calibrates it against information the models cannot see.

I will update this post with the actual errors from each round as the official data is released.

---

*The full pipeline — data extraction, `df_master` construction, models, and backtesting — is documented in the companion notebook available in the project repository.*
