# Dual-Signal Quantitative Framework for Bitcoin Trading

A quantitative trading research project that develops separate Sell-Risk and Buy-Opportunity models for Bitcoin and combines their probability outputs to determine portfolio allocation.

---

## 1. Research Motivation

The study examines the feasibility of quantitative trading in highly volatile markets; our research subject is based on Bitcoin, which has dramatic price volatility in this trading market and is used as the object of research.

Compared to traditional equities and bonds, Bitcoin exhibits relatively high price volatility; therefore, making it an ideal asset for this study to examine quantitative trading strategies in highly volatile markets.

---

## 2. Data Period and Evaluation Design

The time span which is used to build the model is from **2018-09-11 to 2026-06-03**, including **2,800 daily data points**.

Those data are segmented in chronological order.

- **Training Data:** First 80% of the total data
- **Evaluation Data:** Remaining 20% of the total data
- **Evaluation Period:** 2024-11-21 to 2026-06-03
- **Evaluation Observations:** 560 daily observations

The evaluation data from this period are then used to conduct the model's automated trading test.

---

## 3. Dual-Model Framework

Consider the fact that quantitative finance researchers typically formulate their models’ outputs as binary or three-class classification tasks, such as buy/sell or buy/sell/hold.

However, the use of two models’ outputs to jointly determine the final trading decision has received relatively less attention.

Accordingly, this study uses the buy signal and the sell signal to develop two models, respectively. The two models estimate the probabilities of an upside opportunity and downside risk, respectively.

### 3.1 Sell-Risk Model

In the Sell-Risk Model, it predicts whether Bitcoin will experience a decline of at least 10% within the next 10 days or not.

The probability output of the Sell-Risk Model is denoted as $P_{sell}$.

### 3.2 Buy-Opportunity Model

On the other hand, the Buy-Opportunity Model forecasts whether Bitcoin will achieve a gain of at least 10% within the next 10 days.

The probability output of the Buy-Opportunity Model is denoted as $P_{buy}$.

After both models separately evaluate downside risk and upside opportunity, their probability outputs are combined to calculate the target Bitcoin allocation.

---

## 4. Data Selection and Cross-Market Features

In terms of data selection, in addition to the Bitcoin market data and its technical indicators, this study also includes international gold price (USD/ounce) and the S&P 500 VIX as cross-market information.

Gold/USD is included to incorporate information from a safe-haven asset into the analysis.

The S&P VIX is included to reflect investors’ perceptions of market risk.

### Bitcoin Features

The Bitcoin information used by the models includes:

- Volatility over different horizons
- Daily price range
- Breakout strength
- Cumulative return
- Moving-average features

### VIX and Gold/USD Features

VIX and Gold/USD are further used to calculate:

- Daily log return
- 5-day rate of change
- 20-day rolling Z-score

These features are used to assess market conditions by the models.

---

## 5. Trading Decision and Portfolio Allocation

In terms of finally translating model predictions into portfolio positions, this study uses the probability output ($P_{sell}$) of the Sell-Risk Model and the probability output ($P_{buy}$) of the Buy-Opportunity Model to make the final trading decision.

### 5.1 Sell-Risk Override

First, the model determines whether a full exit is required. If $P_{sell} > 0.17$, the Sell-Risk Model has override priority, and the target Bitcoin allocation is set to 0%.

This means that the model determines that the predicted downside risk of Bitcoin exceeds the predefined risk threshold, and Bitcoin exposure is therefore reduced to zero.

### 5.2 Buy-Opportunity Position Allocation

If $P_{sell} \le 0.17$, the model then uses the probability output ($P_{buy}$) from the Buy-Opportunity Model to determine the target Bitcoin allocation.

The allocation is determined according to the following probability ranges:

| Buy-Opportunity Probability | Target Bitcoin Allocation |
|:---|---:|
| $P_{buy} < 0.30$ | 20% |
| $0.30 \le P_{buy} < 0.40$ | 40% |
| $0.40 \le P_{buy} < 0.50$ | 60% |
| $0.50 \le P_{buy} < 0.59$ | 80% |
| $P_{buy} \ge 0.59$ | 100% |

Through this approach, the two models do not simply generate buy or sell signals. Instead, they jointly determine the final proportion of Bitcoin held in the portfolio.

---

## 6. Preliminary Backtesting Results

The preliminary backtesting results show that, during the Evaluation Period, the Bitcoin Buy-and-Hold strategy declined by approximately **33%**, while the Dual-Signal Strategy developed in this study declined by approximately **5%**.

| Strategy | Approx. Return During Evaluation Period |
|:---|---:|
| Dual-Signal Strategy | **~-5%** |
| Bitcoin Buy-and-Hold | **~-33%** |
| Difference | **~+28 percentage points** |

Although the current strategy has not yet generated a positive absolute return, based on the model performance under the current conditions, its primary value is in reducing downside risk and limiting losses in asset value.

Whether the model can further achieve the objective of generating positive absolute returns still requires further investigation in future research.

---

## 7. Evaluation-Period Portfolio Performance

The following figure compares the portfolio value of the Dual-Signal Strategy with the Bitcoin Buy-and-Hold strategy during the Evaluation Period.

![Evaluation-Period Backtest](figures/bitcoin_dual_signal_backtest.png)

**Figure 1. Evaluation-period portfolio value of the Dual-Signal Strategy and Bitcoin Buy-and-Hold strategy.**

---

## 8. Trade Execution Records

The table below presents the first five executed trades generated during the Evaluation Period.

| Date | Action | BTC Price (USD) | Trade Quantity (BTC) | BTC Holdings | Cash (USD) | Total Assets (USD) | Days Since Previous Trade |
|:---|:---|---:|---:|---:|---:|---:|---:|
| 2024-11-29 | Establish 20% Position | 97,461.52 | 0.2050 | 0.2050 | 80,000.00 | 99,980.02 | — |
| 2024-12-04 | Increase to 40% Position | 98,768.53 | 0.2008 | 0.4058 | 60,148.78 | 100,228.13 | 5 |
| 2024-12-09 | Fully Exit Position | 97,432.72 | 0.4058 | 0.0000 | 99,646.53 | 99,646.53 | 5 |
| 2024-12-14 | Establish 100% Position | 101,372.97 | 0.9820 | 0.9820 | 0.00 | 99,546.99 | 5 |
| 2024-12-19 | Fully Exit Position | 97,490.95 | 0.9820 | 0.0000 | 95,639.16 | 95,639.16 | 5 |

The complete trade execution record is provided in **Appendix A**.

---

## 9. Current Limitations and Future Research

The current study still has several limitations.

At this stage, the model uses a time-series data split of:

- **80% Training**
- **20% Evaluation**

In future revisions, independent **Validation** and **Final Test** periods will be further established to provide a more complete evaluation of the model’s performance on previously unused data.

In addition, more international market indicators will be considered as features in future research to further examine whether information from different markets can improve the model’s ability to identify Bitcoin upside opportunities and downside risks.

---

# Appendix

## Appendix A. Complete Trade Execution Record

The complete simulated trade execution history from the Evaluation Period is available in the following CSV file:

[**View Complete Trade Execution Record (CSV)**](appendix/trade_execution_log.csv)

---

## Repository Structure

```text
Bitcoin-Dual-Signal-Quantitative-Framework/
│
├── README.md
│
├── figures/
│   └── bitcoin_dual_signal_backtest.png
│
└── appendix/
    └── trade_execution_log.csv
