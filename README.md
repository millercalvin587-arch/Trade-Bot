# Trade Prediction Bot

Predicts next-day direction (UP/DOWN) for a stock, forex pair, crypto or commodity, using
price-chart indicators plus sentiment from government and central-bank news feeds.

## Setup
    pip install -r requirements.txt

## Run
    python bot.py --symbol AAPL              # US stock
    python bot.py --symbol EURUSD=X          # forex
    python bot.py --symbol BTC-USD           # crypto
    python bot.py --symbol GC=F              # gold
    python bot.py --symbol AAPL --loop 900   # refresh every 15 minutes
    python bot.py --symbol AAPL --demo       # offline test, synthetic data

Each run prints the prediction, held-out backtest accuracy, the news items that moved
sentiment most, and saves `<SYMBOL>_chart.png` (price, SMA10/50, RSI).
Every prediction is appended to `history.csv`.

## How it works
- Prices: Yahoo Finance daily bars via `yfinance`.
- News: RSS feeds listed in `GOV_FEEDS` (Fed, ECB, BoE, US Treasury, USTR, IMF, World Bank,
  Central Bank of Kenya). Edit that dict to add or remove sources; dead feeds are skipped.
- Sentiment: keyword scoring (`POSITIVE` / `NEGATIVE` sets). Replace `score_text` with a
  proper NLP model for better results.
- Model: random forest on returns, moving-average gaps, volatility and RSI, with a
  time-ordered train/test split. The news score adjusts the probability slightly.

## Limits
- Short-term price direction is very hard to predict. Check the backtest accuracy; near 50%
  means the model has no edge. Do not trade real money on this without much more testing.
- Live news is only used for the latest prediction. Past news isn't in the training data, so
  the model can't yet learn how news actually moves prices. `history.csv` starts collecting that.
- This is a learning tool, not financial advice.
