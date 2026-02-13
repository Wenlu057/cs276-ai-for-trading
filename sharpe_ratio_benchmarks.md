# Sharpe Ratio: Realistic Ranges & Red Flags

## Context

This analysis applies to a long-short equity strategy over a 3-ticker universe (AAPL, GE, PSCT) with a maximum gross leverage of 1.5×, evaluated on out-of-sample (test) data using next-day open-to-open returns.

---

## Realistic Ranges

| Strategy Type | Expected Sharpe | Notes |
|---|---|---|
| SMA crossover baseline | 0.3 – 1.0 | Simple trend-following; no ML |
| Well-tuned linear model | 0.5 – 2.0 | Properly lagged features, no leakage |
| Top-tier quant fund (real world) | 1.5 – 3.0 | Decades of research, thousands of signals |

A Sharpe ratio of **1.0** on out-of-sample data is already considered strong for a simple model on a small universe. Achieving **2.0+** with a basic linear regression would be exceptional and warrants careful review of the methodology.

## Red Flags

Any backtest Sharpe above **~3.0** should be treated with suspicion. Common causes:

- **Look-ahead bias** — features computed from data that includes the target period (e.g., using today's close to predict today's return).
- **Survivorship bias** — only testing on assets that performed well historically.
- **Overfitting** — too many parameters relative to training samples, especially without regularization.
- **Incorrect return calculation** — e.g., using close-to-close returns when the strategy trades at the open, or misaligning weights with the return period they apply to.

