# Pre-Trading Automated Sim

A browser-based Monte Carlo simulator for exploring how a hypothetical, high-risk trading setup might perform under a synthetic stock-price model. It runs locally in the browser and does not connect to a broker, fetch market data, or execute a user-provided trading bot.

## Run it

Open `index.html` in a modern browser. No installation, build step, server, or external dependency is required.

## Features

- Configure starting capital, target profit per hour, simulated trading hours, seconds per trade, and number of simulations.
- Run multiple simulated paths with progress feedback and the option to stop a run.
- Review aggregate results:
  - **Success rate:** percentage of simulations whose balance reached the target balance at any point.
  - **Average ending balance** and **average realized profit per hour** across simulations.
  - **Ruin rate:** percentage of simulations whose balance reached zero or below.
  - **Average maximum drawdown:** average peak-to-trough balance decline.
- Explore the results in an ending-profit histogram, equity curves, and a per-trade profit scatter plot. Click chart points to inspect available details.
- Save and revisit recent run settings and summaries. Run details include simulation-level statistics, an equity curve, hourly P&L, and a trade ledger.
- Keep run history in browser `localStorage` (up to 50 runs), with controls to delete individual runs, clear history, or clear individual statistics and charts.
- Reproduce a saved run's results from its stored random seed and inputs.

## Simulation model

The simulator generates synthetic prices and applies a simple randomized trading rule; it does **not** simulate a strategy or bot with a predictive edge.

- Prices start at $100 and follow a geometric Brownian motion model with 15% annualized drift and 80% annualized volatility.
- Volatility varies stochastically over time and is adjusted for an approximate intraday open/midday/close pattern.
- Each trade has a 3% chance of an additional positive or negative gap move. The implemented gap size is approximately 6%–18%.
- The simulated position is 5% of the current balance by notional value. Each trade randomly chooses long or short with equal probability.
- Estimated trading costs are a 12-basis-point spread, 8-basis-point slippage on each side, and $2 commission per trade.
- The simulated stock price has a $0.50 floor. A run is counted as ruined if the account balance reaches zero or below.
- The number of trades per simulation is approximately `hours × 3,600 ÷ seconds per trade`, rounded down with a minimum of one.

These are simplified, fixed assumptions in the source code—not configurable market settings. The model omits real market data, order-book liquidity, actual execution behavior, taxes, and many other sources of risk. In particular, its intraday and gap mechanics are approximations, not a reconstruction of actual trading sessions or overnight events.

## Important disclaimer

This is an educational simulation, not financial advice or a reliable forecast. Synthetic results do not establish that a trading strategy is profitable and do not predict real trading performance. The simulator makes no real trades.
