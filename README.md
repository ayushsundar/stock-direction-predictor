# 📈 stock-direction-predictor

This is a project where I'm using an LSTM-based model to predict the **direction** of stock prices, not the magnitude. The idea is to classify whether a stock will go up or down on the next day using a set of engineered time-series features.

## 🧠 What I'm Building

The model takes in a rolling sequence of 50 days worth of data and tries to predict whether the stock will rise the following day. It uses features like:

* Momentum indicators (10, 60, and 180-day returns)
* Rolling volatility over different time frames
* Trend metrics like moving average slope and daily returns

I'm training the model using a walk-forward validation approach with `TimeSeriesSplit` to keep the test sets honest across time.

## 💡 Signal Conversion

The model outputs a probability between 0 and 1. I convert that into a trading signal:

* If probability > 0.515 → **Buy**
* If probability < 0.485 → **Sell**
* Otherwise → **Hold**

This gives me a directional strategy I can backtest.

## 📊 Backtesting Results

The notebook includes a simple backtest that tracks:

* Cumulative strategy returns
* Sharpe ratio
* Win rate
* Exposure (how often the model makes a move)

I also visualize predicted probabilities and trade return distributions to see how well the signals align with actual price changes.

## 🚧 Next Steps

* Test on other tickers and longer time periods
* Compare to a baseline (e.g., moving average crossover)
* Try adding Fourier-transformed signals or other denoising techniques
* Eventually experiment with alternative model architectures

## 🛠 Built With

* `pandas`, `numpy`, `scikit-learn`
* `tensorflow.keras` for the LSTM
* `matplotlib` and `seaborn` for visualizations
* `yfinance` for historical stock data

## ⚠️ Known Issue

This notebook uses `yfinance` to pull historical stock data.
Occasionally, Yahoo Finance may rate-limit excessive requests and raise a `YFRateLimitError`.
If this happens, wait a few minutes and re-run the notebook.
