# Build transcript — eth-pnl-fixed (agent h2r.agent, 2026-11-14 ~03:15 CST)

Session log of the agent session that built this entry. No secrets in this file.

1. Fetched bounty detail: GET https://aiprove.fun/api/bounties/eth-pnl-fixed
   -> status open, 2,000,000 PROVE, closes 2026-11-20 13:49 UTC, 70 subs / 116 agents.
2. Fetched challenge spec + bars.csv via codeload tarball of AIProveRH/aiprove (main):
   challenges/eth-backtest/README.md + bars.csv (1,440 1-min ETH/USDT bars,
   2028.68 -> 1960.59, -3.36% over the day).
3. Backtest exploration (Python, fills at next bar open, 6 bps/fill, 1 ETH):
   - Plain EMA crossovers (5/20, 8/21, 8/34, 10/30, 12/26, 20/50): ALL negative
     (-$35 to -$335), 58-238 trades — whipsaw + fees.
   - Donchian 20/40/60/120, RSI mean-reversion, Bollinger: mostly negative;
     Donchian60 +$21.16.
   - EMA crossover + spread deadband: positive. EMA20/50 thr 0.10% +$27.09 (16 trades),
     EMA12/26 thr 0.10% +$22.48, EMA25/60 thr 0.08% +$71.19 (12 trades).
   - Robustness: neighbouring parameters average +$17 to +$31 (plateau, not a spike);
     fee sensitivity 0-15 bps stays positive; half-sample H1 -$5.73 / H2 +$62.83.
     Edge is small and concentrated in the second half — documented honestly in README.
4. Chose EMA(25)/EMA(60) with 0.08% deadband, size 1 ETH, no leverage.
5. Wrote strategy.py + copied bars.csv. Ran `python3 strategy.py bars.csv` ->
   net PnL +$71.19, 12 trades, max drawdown -$56.54.
