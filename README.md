# Obsidian Forex

> **A high-accuracy, multi-timeframe scalping indicator for TradingView**  
> Works on all forex pairs · XAUUSD (Gold) · NAS100 · BTCUSD · indices

---

## Features

- **Multi-Timeframe Analysis (MTF)** — simultaneously analyses 5 m, 10 m, 30 m, and 1 H
- **Confluence Scoring (0–100)** — signal fires only when most indicators on most timeframes agree
- **7-Layer Signal Engine**:
  - EMA Ribbon (8 / 21 / 55) — trend direction and ribbon structure
  - Supertrend (ATR 10, ×2.0) — dynamic trend confirmation
  - RSI (14) — momentum with overbought / oversold bands
  - MACD (12, 26, 9) — crossover + histogram direction
  - Stochastic RSI (14, 14, 3, 3) — precise scalper entry timing
  - Volume (VWMA vs SMA) — confirms institutional participation
  - Bollinger Bands (20, 2.0) — volatility and squeeze detection
- **Market-Structure Layer** — Higher Highs / Lower Lows, Fair Value Gaps, Order Blocks
- **Dynamic SL / TP** — ATR-based, auto-adjusted per instrument (Forex / Gold / Index / Crypto)
- **Anti-Whipsaw Filter** — configurable minimum bars between signals
- **Premium Dashboard** — live signal panel in the top-right corner of the chart
- **Instrument Auto-Detection** — automatically widens SL for Gold, crypto, and indices
- **Full Alert Support** — dynamic alerts with score + SL/TP, plus `alertcondition()` entries

---

## File Structure

```
/
├── README.md                          ← This file
├── src/
│   ├── obsidian_forex_overlay.pine    ← Main Pine Script v5 indicator
│   └── config.md                      ← Full input reference & tuning guide
└── docs/
    ├── INSTALL.md                     ← Step-by-step TradingView installation
    ├── STRATEGY.md                    ← Signal logic explained
    └── CHANGELOG.md                   ← Version history
```

---

## Quick Installation

1. Open [TradingView](https://www.tradingview.com) and open any chart
2. Click **Pine Editor** in the bottom toolbar
3. Select all default code and delete it
4. Open [`src/obsidian_forex_overlay.pine`](src/obsidian_forex_overlay.pine),
   copy the entire file, and paste it into the editor
5. Click **Save**, then **Add to chart**

For detailed instructions (including alert setup) see [docs/INSTALL.md](docs/INSTALL.md).

---

## How to Use the Signals

| Signal | Action |
|---|---|
| **▲ BUY** (green label) | Enter long on the next candle open. SL below the dashed red line; target TP1/TP2/TP3 dotted lines. |
| **▼ SELL** (red label) | Enter short on the next candle open. SL above the dashed red line; target TP1/TP2/TP3. |
| **● NEUTRAL** (grey bg) | No trade — confluence score is between 25 and 75. Stand aside. |

**Dashboard quick-reference:**

| Colour | Meaning |
|---|---|
| 🟢 Green signal cell | Strong BUY bias (score ≥ 75) |
| 🔴 Red signal cell | Strong SELL bias (score ≤ 25) |
| ⚫ Grey signal cell | Neutral / ranging market |
| ▲ Timeframe arrow | That TF's EMA ribbon is bullish |
| ▼ Timeframe arrow | That TF's EMA ribbon is bearish |

For the full strategy explanation and position-sizing guide see
[docs/STRATEGY.md](docs/STRATEGY.md).

---

## Supported Instruments

| Category | Examples |
|---|---|
| Forex majors | EURUSD, GBPUSD, USDJPY, AUDUSD, USDCHF, USDCAD, NZDUSD |
| Forex minors | EURGBP, EURJPY, GBPJPY, AUDJPY … |
| Forex exotics | USDTRY, USDZAR, USDMXN … |
| Gold | XAUUSD |
| Indices | NAS100, US500, SPX |
| Crypto | BTCUSD, ETHUSD |

---

## Configuration

All inputs are available under the indicator's **Settings** panel.
See [`src/config.md`](src/config.md) for a full description of every parameter
and per-market tuning tips.

---

## Alerts

Three alert conditions are available in TradingView's alert dialog:

- **Obsidian Forex — BUY Signal**
- **Obsidian Forex — SELL Signal**
- **Obsidian Forex — Trend Change**

Dynamic alerts (fired at bar close) include the pair name, confluence score,
and SL / TP levels in the notification message.

---

## Pine Script Version

- **Version**: Pine Script v5 (`//@version=5`)
- **Type**: `indicator()` with `overlay=true`
- **Lookahead**: all `request.security()` calls use `barmerge.lookahead_off`
  to prevent repainting

---

## Disclaimer

> This indicator is provided for **educational and informational purposes only**.
> It does not constitute financial advice, investment advice, trading advice,
> or any other sort of advice. You should not treat any of the indicator's
> output as such.  
>
> Trading forex, gold, indices, and cryptocurrency involves substantial risk
> of loss. Past performance of any signal, strategy, or indicator is **not**
> indicative of future results. Always use proper risk management and consult
> a qualified financial adviser before making trading decisions.
