# Turkish Stock Portfolio Analysis

I built this project in mid-2025, before starting my MSc Finance, to teach myself how to use Python for investment analysis. It looks at a portfolio of five Borsa Istanbul stocks and compares it with the BIST 100 index (XU100).

After finishing my MSc I came back to it, fixed some mistakes and added the one thing the original version was missing: a risk-free rate. In Turkey that changes the whole conclusion.

The five stocks were an exploratory pick, not chosen with a strategy. This is a learning project, not investment advice.

## What the notebook does

- Downloads daily prices for the five stocks and the XU100 (2 May 2024 to 12 June 2025)
- Calculates returns, volatility and correlations
- Compares an equal-weighted portfolio with the index and with a TL deposit
- Simulates 10,000 random portfolios to find the one with the best Sharpe ratio (Modern Portfolio Theory)
- Runs CAPM regressions (beta and alpha for each stock) and rolling betas
- Looks at drawdown and Value at Risk (VaR)

## Main finding: a TL deposit beat everything

![Cumulative returns](images/cumulative_returns.png)

Over the period, the equal-weighted portfolio returned about +1.8% and the BIST 100 about -7%. A TL deposit at the central bank policy rate (around 48% a year on average) would have returned about +53%.

So the portfolio slightly beat the index, but lost to a risk-free deposit by around 50 percentage points. With interest rates this high, stocks need very large returns just to break even against keeping the money in the bank.

## Other results

- **Sharpe ratios:** using the policy rate as the risk-free rate, the equal-weighted portfolio has a Sharpe ratio of -1.26. Even the "optimal" portfolio from the simulation, which is chosen with hindsight on the same data, only reaches 0.12.
- **Big differences between stocks:** KZBGY returned +84% and A1CAP +26%, but KTLEV fell 51% and ARTMS 48%. With equal weights, the losers cancelled out the winners.
- **Mostly stock-specific risk:** the stocks have low correlations with each other (0.11 to 0.32), betas below 1 (0.50 to 0.87), and the index explains only 3% to 20% of their moves (R-squared).
- **Downside risk:** the maximum drawdown was -34%, most of it in the first few weeks. The 1-day VaR was -3.15% at 95% and -5.89% at 99%.

![Efficient frontier](images/efficient_frontier.png)

## What I changed when I revisited it

- Added the risk-free rate to the Sharpe ratios and a TL deposit line to the returns chart (the original version assumed 0%)
- Fixed the total return calculation, which was adding daily returns instead of compounding them
- Fixed a bug where the CAPM loop overwrote the price data
- Used only the dates where all five stocks have prices, so everything is compared over the same period
- Fixed the end date so the results don't change every time the notebook runs
- Shortened the rolling beta window to 126 days, because the data covers only about 13 months

## What I would do differently now

- Choose the stocks with a clear reason, and test the optimised weights on a later period instead of the same data (the optimisation here is in-sample)
- Look at real returns after inflation, which was very high in Turkey over this period
- Compare with other ways of protecting savings, such as gold or foreign currency

## How to run it

```
pip install numpy pandas matplotlib seaborn statsmodels yfinance
```

Open `turkish_stock_portfolio_analysis.ipynb` and run all cells. Prices are downloaded from Yahoo Finance.

*Built with Python (pandas, NumPy, matplotlib, seaborn, statsmodels) and yfinance.*
