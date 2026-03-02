# Obsidian Forex — Configuration Guide

This document describes every user-configurable input available in the
`obsidian_forex_overlay.pine` indicator, grouped by the section they appear
in inside TradingView's Settings panel.

---

## Signal Settings

| Input | Default | Range | Description |
|---|---|---|---|
| **BUY Threshold** | 75 | 51 – 99 | Fire a BUY arrow when the confluence score reaches or exceeds this value. Raise it (e.g., 80) for fewer, higher-quality signals. |
| **SELL Threshold** | 25 | 1 – 49 | Fire a SELL arrow when the confluence score falls to or below this value. Lower it (e.g., 20) for stricter SELL conditions. |
| **Min Bars Between Signals** | 5 | 1 – 50 | Anti-whipsaw gate. No new signal can fire until at least this many bars have closed since the last one. Increase on noisy pairs. |

---

## Timeframes

| Input | Default | Description |
|---|---|---|
| **TF 1 — Primary (5 m)** | `5` | Fastest scalping timeframe. Receives the lowest weight (15 %). |
| **TF 2 (10 m)** | `10` | Intermediate filter. Weight 20 %. |
| **TF 3 (30 m)** | `30` | Medium-term trend. Weight 30 %. |
| **TF 4 (1 H)** | `60` | Highest-weight timeframe (35 %). Drives the bulk of the signal. |

> **Note**: You can adjust these to any valid TradingView timeframe string
> (e.g., `"15"`, `"240"`, `"D"`).  Higher-timeframe bias is intentional —
> the 1 H view filters out low-quality scalp setups.

---

## EMA Ribbon

| Input | Default | Description |
|---|---|---|
| **EMA Fast** | 8 | Short-term momentum EMA. |
| **EMA Mid** | 21 | Medium-term trend EMA. |
| **EMA Slow** | 55 | Long-term trend EMA. Acts as dynamic support/resistance. |

A bullish ribbon requires: **EMA 8 > EMA 21 > EMA 55** and price above EMA 8.  
A bearish ribbon requires the inverse.

---

## Supertrend

| Input | Default | Description |
|---|---|---|
| **Multiplier** | 2.0 | ATR multiplier for band width. Lower = more sensitive, more signals. |
| **ATR Period** | 10 | Lookback period for the ATR component. |

---

## RSI

| Input | Default | Description |
|---|---|---|
| **Length** | 14 | RSI lookback period. |

Scoring bands: > 70 overbought (bearish penalty), < 30 oversold (bullish bonus), 50–70 bullish, 30–50 bearish.

---

## MACD

| Input | Default | Description |
|---|---|---|
| **Fast Length** | 12 | Fast EMA of MACD. |
| **Slow Length** | 26 | Slow EMA of MACD. |
| **Signal Length** | 9 | Signal line smoothing. |

---

## Stochastic RSI

| Input | Default | Description |
|---|---|---|
| **RSI Length** | 14 | RSI period fed into the Stochastic calculation. |
| **Stoch Length** | 14 | Stochastic lookback applied to the RSI values. |
| **K Smooth** | 3 | Smoothing for the %K line. |
| **D Smooth** | 3 | Smoothing for the %D line. |

---

## Bollinger Bands

| Input | Default | Description |
|---|---|---|
| **Length** | 20 | SMA period for the middle band. |
| **Multiplier** | 2.0 | Standard deviation multiplier for the upper/lower bands. |

---

## ATR / SL-TP

| Input | Default | Description |
|---|---|---|
| **ATR Length** | 14 | ATR lookback used for stop-loss and take-profit calculations. |
| **SL Multiplier** | 1.5 | Stop-loss = ATR × this multiplier (adjusted further for volatile instruments). |

**Instrument-specific multiplier overrides** (applied automatically):

| Instrument | Effective SL Multiplier |
|---|---|
| FOREX | `SL Multiplier` (as set) |
| XAUUSD / GOLD | `SL Multiplier × 1.5` |
| NAS100 / SPX (index) | `SL Multiplier × 1.3` |
| BTCUSD / crypto | `SL Multiplier × 1.4` |

Take-profit levels are always derived from the SL distance:

| Level | Distance | Risk:Reward |
|---|---|---|
| TP1 | 1 × SL | 1 : 1 |
| TP2 | 2 × SL | 1 : 2 |
| TP3 | 3 × SL | 1 : 3 |

---

## Visual

| Input | Default | Description |
|---|---|---|
| **Show EMA Ribbon** | ✅ | Draw the three EMA lines and the fill between them. |
| **Show Background Color** | ✅ | Subtle green/red/grey chart background tint based on current bias. |
| **Show SL / TP Lines** | ✅ | Draw dashed SL and dotted TP lines extending 20 bars from each signal. |
| **Show Dashboard** | ✅ | Top-right panel table with live signal data. |

---

## Tips for Different Markets

### XAUUSD (Gold)
- Widen **Min Bars Between Signals** to `8–10` to avoid over-trading during news
- Default 1 H timeframe weight handles gold's frequent intraday reversals well

### NAS100 / Indices
- Consider changing **TF 4** to `"240"` (4 H) for smoother index trends
- Raise **BUY Threshold** to `78` and lower **SELL Threshold** to `22`

### BTCUSD (Crypto)
- Use **SL Multiplier** of `2.0` to account for high volatility
- Keep **Min Bars Between Signals** at `5` — crypto moves fast

### Exotic Forex Pairs
- Standard defaults work well; raise **Min Bars Between Signals** to `7` for
  thin-liquidity pairs (e.g., USDTRY, USDZAR)
