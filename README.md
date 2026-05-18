# VWAP Mean-Reversion Research Tool
### ES Futures | Tick Data | NY Regular Trading Hours

![VWAP Analysis Chart](output(vwap-mean-reversion).png)

---

## What This Project Does

This tool analyses whether price reverts back to VWAP after touching 
the ±2 standard deviation bands during a trading session on ES futures 
(S&P 500 index futures). It uses real tick data and answers the question:

**"When price stretches too far from VWAP, does it reliably snap back — 
and is that statistically real or just luck?"**

---

## Background

I'm a trader with hands-on experience using VWAP and tick data on ES 
futures. I built this tool to quantify observations I had made manually 
while trading — turning trading intuition into something statistically 
testable and measurable.

---

## What It Analyses

- **VWAP bands** at ±1σ and ±2σ computed from volume-weighted price data
- **Touch events** — every time price crosses the ±2σ band
- **Outcomes** — did price revert to VWAP within the forward window,
  continue, or reverse?
- **3 session segments** — Open (9:30–10:30), Midday (10:30–14:00),
  Close (14:00–16:00)
- **Statistical significance** — bootstrap hypothesis testing with
  Bonferroni correction across 6 test conditions (2 directions × 3 segments)
- **Execution realism** — bid/ask spread modeling, 1-bar latency,
  fallback spread when quotes are invalid
- **Regime conditioning** — VIX level, day classification
  (trending vs balanced), opening drive score at 9:45
- **Sensitivity analysis** — results tested across σ levels
  (1.5, 2.0, 2.5) and forward windows (5min, 10min, 30min)

---

## Key Metrics Produced

| Metric | What It Tells You |
|--------|------------------|
| Reversion rate | % of band touches that returned to VWAP |
| Bootstrap 95% CI | How reliable that rate is statistically |
| Baseline comparison | Is it better than random timing? |
| Bonferroni p-value | Is it significant after multiple comparison correction? |
| Net Sharpe | Risk-adjusted return after spread costs |
| MAE / MFE | How far price moved against / for the trade |
| Time to reversion | How long it typically takes to snap back |

---

## Sample Results

Running on a single ES session (2026-05-12):

- **22,500** second bars processed
- **80 touch events** detected (21 upper, 59 lower)
- Strong reversion behaviour on lower band during Open segment
- Day classified as **Trend_Bear** with **Strong_Bull** opening drive
- Sensitivity analysis confirms results hold across multiple σ and window settings

---

## Data

- **Instrument:** ES Futures (S&P 500)
- **Data type:** Tick data → aggregated to 1-second OHLCV bars
- **Session:** NY Regular Trading Hours (9:30–16:00 ET)
- **External data:** VIX and SPY ATR fetched via yfinance (cached locally)

---

## Tech Stack

- Python 3
- pandas, numpy, scipy
- matplotlib
- yfinance

---

## How to Run

1. Install dependencies:
!pip install pandas numpy scipy matplotlib yfinance

2. Add your tick data CSV and run the last cell, or call directly:
```python
from pathlib import Path
main(RunConfig(
    csv_path=Path("your_tick_data.csv"),
))
```

3. Output: a full analysis chart saved as `vwap_analysis.png`
   plus a detailed printed report in the notebook

---

## What I Learned Building This

- Bootstrap hypothesis testing and confidence intervals
- Bonferroni correction for multiple comparisons
- Execution modeling — spread costs, latency simulation
- Regime classification using VIX and realized volatility quartiles
- MAE/MFE analysis for path risk measurement
- Building a full research pipeline from raw tick data to
  statistical conclusions

---

## Status

Single-session prototype. Next version will extend to 20+ sessions
with walk-forward validation.

---

*Built as a personal research project combining live trading experience 
on ES futures with a data analysis course. Domain knowledge from 
real trading, statistical framework built and learned through the project.*

---

**GitHub:** [github.com/WhiteRabbit-TB](https://github.com/WhiteRabbit-TB)
