# Multi-Asset Momentum Strategy (2000–2025)

This project implements and analyzes a **cross-sectional momentum trading strategy** across a diversified set of U.S. equities using Python.  
The analysis follows a complete quantitative research workflow: **data collection → preprocessing → signal generation → backtesting & performance evaluation**, including robustness checks and transaction cost modeling.

---

##Project Overview

Momentum is one of the most persistent and well-documented factors in finance.  
This project builds a **12–1 momentum model**, where assets are ranked by their cumulative returns over the previous 12 months (excluding the most recent month).  
The strategy is tested in three configurations:

- **Long-Only (1M):** invest equally in top 20% winners.  
- **Long-Short (1M):** go long winners and short losers each month.  
- **Long-Short (3M holding):** overlap three monthly portfolios for smoother performance.

All results are benchmarked against the **SPY**, representing the U.S. equity market.

---

##Notebooks

| Notebook | Description |
|-----------|--------------|
| **01_data_collection.ipynb** | Downloads daily adjusted prices (2000–2025) for 20 large-cap U.S. stocks and SPY using `yfinance`. Ensures data completeness and saves raw datasets for reproducibility. |
| **02_preprocess_returns.ipynb** | Aggregates daily data to monthly frequency, computes monthly returns, and performs exploratory data analysis (EDA). Includes descriptive statistics, return distributions, and correlation heatmaps (with and without outliers like AAPL and AMZN). |
| **03_signal_generation.ipynb** | Constructs the **12–1 cross-sectional momentum signal**, ranks assets into winners/losers, and creates portfolio weights for Long-Only and Long-Short strategies. Includes visualizations (signal heatmap, selection frequency, and exposure over time). |
| **04_backtesting_performance.ipynb** | Backtests each portfolio configuration, computes performance metrics (CAGR, Volatility, Sharpe, Sortino, Calmar, Max Drawdown), and compares to SPY. Adds rolling Sharpe, turnover analysis, and transaction cost impact (5 bps per side). |

---

##Key Results

| Strategy | CAGR | Ann. Vol | Sharpe | Sortino | Max DD | Calmar |
|-----------|------|-----------|---------|----------|----------|----------|
| **Long-Only (1M)** | 13.7% | 17.9% | 0.81 | 1.27 | −45.5% | 0.30 |
| **Long-Short (1M)** | −2.5% | 24.2% | 0.02 | 0.03 | −71.6% | −0.04 |
| **Long-Short (3M)** | −2.0% | 22.3% | 0.02 | 0.03 | −70.4% | −0.03 |
| **SPY Benchmark** | 8.7% | 15.0% | 0.63 | 0.87 | −50.8% | 0.17 |

**Takeaways**
- The **Long-Only 1M** strategy outperforms SPY with higher CAGR and Sharpe.  
- Market-neutral versions suffer from typical *momentum crashes* during reversals.  
- After accounting for **5 bps per-side trading costs**, performance remains robust.  
- Average turnover ≈ **0.8**, meaning ~80% of portfolio weights are reallocated each month.

---

##Visual Insights

The project includes professional visualizations for interpretability:

- **Price & return evolution:** cumulative growth of all assets (with and without outliers).  
- **Momentum heatmap:** temporal strength of the 12–1 signal across assets.  
- **Equity curves:** comparison of long-only, long-short, and SPY benchmark.  
- **Drawdown analysis:** historical loss depth visualization.  
- **Rolling Sharpe:** time-varying risk-adjusted performance.  
- **Turnover and cost impact:** portfolio activity and performance degradation after costs.

---

##Methodology

1. **Data Source:** Yahoo Finance (`yfinance` API)  
2. **Frequency:** Monthly (EOM resampling of daily adjusted close prices)  
3. **Universe:** 20 diversified U.S. large-caps + SPY benchmark  
4. **Signal Definition:**  

   $$
   \text{momentum}_{i,t} = \prod_{k=2}^{12}(1 + r_{i,t-k}) - 1
   $$

5. **Portfolio Construction:**  
   - Long-Only: equal-weighted top 20% winners  
   - Long-Short: equal-weighted long winners and short losers  
   - Holding period \( H = 3 \) months (overlapping portfolios)  

6. **Backtest Controls:**  
   - One-month lag on signals to avoid look-ahead bias  
   - Risk metrics annualized using monthly returns  
   - Transaction costs modeled via turnover × 2 × 0.0005  

---

##Performance Metrics

All metrics are implemented directly in Python:

| Function | Formula |
|-----------|----------|
| **CAGR** | \( (1 + R_\text{total})^{1/T} - 1 \) |
| **Annual Volatility** | \( \sigma_\text{monthly} \times \sqrt{12} \) |
| **Sharpe Ratio** | \( \frac{\bar{r}}{\sigma} \sqrt{12} \) |
| **Sortino Ratio** | \( \frac{\bar{r}}{\sigma_\text{downside}} \sqrt{12} \) |
| **Max Drawdown** | Minimum of cumulative return / peak − 1 |
| **Calmar Ratio** | \( \frac{\text{CAGR}}{|\text{Max DD}|} \) |

---

##Technologies used

- **Language:** Python 3.11+  
- **Core Libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `yfinance`, `scipy`  
- **Environment:** JupyterLab or VS Code notebooks  

---

##Author

**Ignacio Pinazo Orihuela**  
*Computer Engineering Student* — Universitat Politècnica de València    
[LinkedIn](https://www.linkedin.com/in/ignacio-pinazo-orihuela-143539288/) 



