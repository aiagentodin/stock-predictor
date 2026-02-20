# Stock Predictor

ML-powered stock screening system using LightGBM ensemble models to identify top S&P 500 stocks for 1-week and 1-month horizons.

## Quick Start

```bash
# First-time setup (creates venv, installs deps, trains models)
chmod +x setup.sh run.sh
./setup.sh

# Daily scans
./run.sh

# Open the dashboard
open dashboard.html
```

## Commands

| Command | Description |
|---------|-------------|
| `python predictor.py --train` | Train models on 3 years of historical data |
| `python predictor.py --scan` | Run full scan, output to `data/predictions.json` |
| `python predictor.py --backtest` | Walk-forward backtest with accuracy report |

## Architecture

### ML Pipeline
- **Data:** Daily OHLCV for ~500 S&P 500 stocks via yfinance
- **Features:** 28 technical indicators (returns, momentum, RSI, MACD, Bollinger, OBV, MFI, ATR, ADX, volume, volatility, SMA crossovers, stochastic, price position)
- **Normalization:** Cross-sectional z-score per date
- **Model:** 5-model LightGBM ensemble with different random seeds
- **Training:** Walk-forward on rolling 2-year window
- **Target:** Binary — will stock outperform median in next 5/20 trading days?

### Scoring (0-100)
- ML prediction probability: 50%
- Technical momentum: 20%
- Volume/accumulation: 15%
- Volatility-adjusted: 15%

### Dashboard
- Single-file HTML with embedded CSS/JS
- Dark fintech theme with glassmorphism
- Loads from `data/predictions.json` (falls back to sample data)
- Responsive, animated, interactive

## File Structure

```
stock-predictor/
├── predictor.py          # ML engine
├── dashboard.html        # Frontend dashboard
├── requirements.txt      # Python dependencies
├── setup.sh              # First-time setup
├── run.sh                # Daily runner
├── README.md
├── data/
│   ├── predictions.json       # Live scan results
│   └── sample_predictions.json # Demo data
└── models/
    ├── models_5d.pkl     # Trained 5-day models
    ├── models_20d.pkl    # Trained 20-day models
    └── accuracy.json     # Validation accuracy
```

## Disclaimer

This is for educational/research purposes only. Not financial advice.
