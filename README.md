# Project: Enhanced Value and Momentum Index

## Overview
This project aims to enhance value and momentum investment strategies through index investments in the India Stock Exchange (NSE). It utilizes historical data analysis and predictive modeling to optimize investment decisions based on risk appetite and market conditions.

- **Git Repository:** [Enhanced Value and Momentum Index](https://github.com/dilip-k-m/enhance-value-and-momentum-index.git)

### Concept
Historical data analysis indicates that Nifty 50 (N50), Mid cap 150 (Mid150), and Small cap 250 (Sml250) indices have provided Compound Annual Growth Rates (CAGR) of 12%, 20%, and 27% respectively over the last 30 years. These indices are considered alongside Gold ETF for investment strategies.

Indices considered and their corresponding CAGR:
- Nifty 50 (N50) (2-3 yr): 12%
- Mid cap 150 (Mid150) (3-5 yrs): 20%
- Small cap 250 (Sml250) (>7 yr): 30%

The project's core logic involves entering the market at low points and exiting at high points based on value and momentum investment principles. Portfolio rebalancing occurs periodically according to the investor's risk appetite.

### Product Lifecycle
The project is structured into three phases of deployment:

1. **Initial Load (Start of Investment):** 
   - Identify the optimal entry points into the market based on market conditions.
   - Utilize a risk score analyzer to determine the allocation of capital across indices.

2. **Portfolio Rebalance:**
   - Monthly and daily predictions of market conditions are made to adjust portfolio allocations.
   - Capital movement actions are taken in real-time based on predicted market conditions.

3. **Partial Withdrawal (SWP) and Delta Load (SIP):**
   - Withdrawals and investments are made based on market conditions, following predefined rules.

### Code-base
The project's predictive modeling utilizes ARIMA and LSTM models to forecast future price movements of indices. These models are trained on historical data, including:
- Top 5 global economy major indices
- Gold prices
- Bond indices
- India's GDP and trade data

The choice between ARIMA and LSTM models depends on factors such as data characteristics, stationarity, interpretability, data size, and forecasting horizon.

#### Why ARIMA and LSTM?
Determining whether to use ARIMA or LSTM models depends on several factors:
- Linear patterns and stationarity favor ARIMA models.
- ARIMA provides interpretability and is suitable for short-term forecasting with smaller datasets.
- LSTM excels in capturing non-linear patterns, handling large-scale data, and long-term forecasting.

