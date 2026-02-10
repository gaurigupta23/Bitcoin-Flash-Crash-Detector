# Bitcoin Flash Crash Detector & Recovery Trade Simulator

A research notebook that detects Bitcoin crash events using multi-factor signals and simulates a simple mean-reversion recovery strategy on daily bars. It is intended for educational and research purposes.

## What This Project Does
- Downloads BTC-USD daily OHLCV data from Yahoo Finance via `yfinance`.
- Engineers features (returns, rolling volatility, rolling z-scores, drawdown, volume percentiles).
- Flags crash events using a multi-factor filter (price shock + statistical anomaly + volume spike + optional drawdown filter).
- Backtests a recovery trade (buy on crash, exit after a fixed hold period).
- Produces performance metrics, charts, and CSV exports.

## Project Files
- `Bitcoin_Crash_Detector.ipynb`: Main notebook.
- `crash_detection_chart.png`: Price chart with crash markers.
- `trading_performance_chart.png`: Return distribution and cumulative performance.
- `crash_severity_analysis.png`: Crash severity scatterplot.
- `crash_events.csv`: Detected crash rows.
- `crash_trades_results.csv`: Trade-level results.
- `full_data.csv`: Feature-engineered dataset.

## Requirements
- Python 3.9+
- Packages: `pandas`, `numpy`, `yfinance`, `matplotlib`, `seaborn`, `jupyter`

## Quickstart
1. Create and activate a virtual environment.
2. Install dependencies.
3. Launch Jupyter and run the notebook top-to-bottom.

Example:
```bash
python -m venv .venv
source .venv/bin/activate
pip install jupyter pandas numpy yfinance matplotlib seaborn
jupyter notebook Bitcoin_Crash_Detector.ipynb
```

## Key Parameters (Daily Bars)
These defaults are optimized for daily data and can be tuned:
```python
TICKER = 'BTC-USD'
START_DATE = '2023-02-08'
END_DATE = '2025-02-08'
INTERVAL = '1d'

CRASH_THRESHOLD = -6            # Daily return threshold
ZSCORE_THRESHOLD = -1.8         # Rolling z-score threshold
VOLUME_PERCENTILE = 0.90        # Rolling volume percentile threshold
DRAWDOWN_THRESHOLD = -10        # Drawdown threshold from rolling high
USE_DRAWDOWN_FILTER = True

ZSCORE_WINDOW = 90              # Rolling window (days)
VOLUME_WINDOW = 60
DRAWDOWN_WINDOW = 20

HOLD_PERIOD = 1                 # Days to hold after crash
STOP_LOSS = -15                 # Tracked (not enforced in exit logic)
```

## Crash Detection Logic
A crash is flagged when all core conditions are met:
- **Price shock:** daily return < `CRASH_THRESHOLD`
- **Statistical anomaly:** rolling z-score < `ZSCORE_THRESHOLD`
- **Volume spike:** volume > rolling percentile (`VOLUME_PERCENTILE`)
- **Drawdown filter (optional):** drawdown < `DRAWDOWN_THRESHOLD`

## Outputs
Running the notebook generates:
- Charts saved as PNGs.
- CSV exports for crash events, trade results, and full feature dataset.

## Notes and Limitations
- Results are sensitive to thresholds and windows; tune parameters for your use case.
- Daily data smooths intraday flash crashes. For true intraday events, use hourly data (note that free Yahoo Finance intraday history is limited).
- Transaction costs and slippage are not included.
- Stop-loss is recorded but not used to change the exit price in the current simulation.

## Data Source
Yahoo Finance via the `yfinance` Python package.

## Disclaimer
This project is for educational and research purposes only and does not constitute financial advice.
