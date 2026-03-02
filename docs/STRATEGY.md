# Strategy Guide — Obsidian Forex

This document explains how the **Obsidian Forex** indicator makes its
decisions, so you can trade it with confidence and adapt it to your style.

---

## Philosophy

The indicator is built around a single principle:  
> **Only trade when the majority of evidence — across multiple timeframes and
> multiple indicator families — agrees.**

A single indicator crossing a threshold is noise. When RSI, MACD,
Stochastic RSI, EMA trend structure, volume, and Supertrend all lean the same
direction on the same timeframe *and* higher timeframes confirm it, the
probability of a successful trade increases substantially.

---

## Signal Engine: The Confluence Score

Every bar, the script calculates a **Confluence Score from 0 to 100**.

- **≥ 75** → BUY signal fires
- **≤ 25** → SELL signal fires
- **26 – 74** → NEUTRAL — no trade

### Step 1: Per-Timeframe Scoring

For each of the four timeframes (5 m, 10 m, 30 m, 1 H), the script evaluates
seven indicator families and assigns points:

| Indicator | Max Points | Bullish Condition |
|---|---|---|
| EMA Ribbon (8/21/55) | 20 | EMA8 > EMA21 > EMA55, price > EMA8 |
| Supertrend | 15 | Close above Supertrend line |
| RSI (14) | 15 | RSI 50–70 (momentum up, not overbought) |
| MACD (12/26/9) | 15 | MACD line > signal, histogram positive |
| Stochastic RSI | 15 | %K > %D, %K < 80 (rising, not overbought) |
| Volume (VWMA vs SMA) | 10 | VWMA > SMA20 (buying volume dominant) |
| Bollinger Bands | 10 | Price between midline and upper band |
| **Total** | **100** | |

Each timeframe therefore produces a score between 0 and 100.

### Step 2: Weighted Aggregation

The four timeframe scores are combined with weights that favour higher
timeframes (less noise):

| Timeframe | Weight |
|---|---|
| 5 m | 15 % |
| 10 m | 20 % |
| 30 m | 30 % |
| 1 H | 35 % |

`Weighted Score = (Score_5m × 0.15) + (Score_10m × 0.20) + (Score_30m × 0.30) + (Score_1H × 0.35)`

### Step 3: Market-Structure Micro-Adjustments

Up to ±5.5 extra points are added/subtracted based on price-action context:

| Pattern | Adjustment |
|---|---|
| Higher High detected | +2 |
| Lower Low detected | −2 |
| Bullish Fair Value Gap | +2 |
| Bearish Fair Value Gap | −2 |
| Price inside Bullish Order Block | +1.5 |
| Price inside Bearish Order Block | −1.5 |

This final value is clamped to **0 – 100** and displayed on the dashboard.

---

## Market Structure Detection

### Higher Highs / Lower Lows
The script looks back 10 bars. If the current high is above the 10-bar high
recorded one bar ago, a Higher High is confirmed — structural bullish bias.
The inverse gives a Lower Low — structural bearish bias.

### Fair Value Gaps (FVG)
A **Bullish FVG** exists when the current candle's low is above the high of
two bars ago (a gap left in the order flow that price may return to fill,
or that confirms a strong impulsive move).  
A **Bearish FVG** is the mirror image.

### Order Blocks
An **Order Block** is identified as a large-body candle (body > 1.5× ATR)
two bars in the past. If current price revisits that candle's body range, the
indicator treats it as an institutional zone with potential support or resistance.

---

## Anti-Whipsaw Filter

Even when the confluence score crosses a threshold, the signal will **not fire**
unless at least **N bars** have passed since the previous signal (default: 5).

This prevents the indicator from issuing rapid back-to-back signals during
choppy, low-conviction conditions. Adjust **Min Bars Between Signals** in
Settings to suit your trading speed.

---

## SL / TP Levels

Stop-loss and take-profit are **dynamic** — recalculated every bar from the
ATR at the moment of signal:

```
SL  = ATR(14) × SL_Multiplier
TP1 = SL × 1   →  1:1 RR
TP2 = SL × 2   →  1:2 RR
TP3 = SL × 3   →  1:3 RR
```

For BUY signals the SL sits **below** entry; for SELL signals it sits **above**.

The multiplier is automatically widened for volatile instruments:
- XAUUSD → ×1.5
- NAS100 / Indices → ×1.3
- BTCUSD / Crypto → ×1.4

---

## How to Use the Signals

### Entry
1. Wait for a **▲ BUY** or **▼ SELL** label to appear on a closed bar
2. Confirm the dashboard shows the same direction
3. Enter at the **next candle open** (signal always prints on bar close)

### Stop-Loss
- Place your stop at the **dashed red line** below (BUY) or above (SELL) entry

### Take-Profit
- **Conservative**: close at TP1 (1:1)
- **Standard**: close half at TP1, move stop to break-even, close remainder at TP2
- **Aggressive**: trail stop through TP1 → TP2, target TP3

### Position Sizing
- Risk no more than **1–2 %** of account equity per trade
- The SL distance in pips ÷ pip value gives your lot size at your chosen risk %

---

## Dashboard Panel Reference

| Row | Meaning |
|---|---|
| OBSIDIAN FOREX / Ticker | Header — pair currently viewed |
| Type | Auto-detected instrument: FOREX / GOLD / INDEX / CRYPTO |
| Signal | Current bias: ▲ BUY (green), ▼ SELL (red), ● NEUTRAL (grey) |
| Confluence | Score 0–100 with colour gradient (red → orange → yellow → green) |
| 5 m / 10 m | EMA-ribbon trend direction on each TF (▲ up / ▼ down / ● flat) |
| 30 m / 1 H | Same for the higher timeframes |
| RSI (14) | Current-chart RSI value (red if overbought, green if oversold) |
| MACD | 1 H MACD status: Bullish / Bearish / Neutral |
| Volatility | ATR-based: High / Normal / Low |
| ATR | Raw ATR(14) value in price units |
| SL | Stop-loss distance in price units |
| TP1 / TP2 / TP3 | Take-profit distances in price units |

---

## Best Timeframes for Scalping

| Chart TF | Style | Notes |
|---|---|---|
| 1 m | Ultra-scalp | Noisy; raise BUY/SELL thresholds to 80 / 20 |
| 5 m | Scalp (recommended) | Default settings optimised for this |
| 15 m | Short-term swing | Lower thresholds slightly for more signals |
| 1 H | Day-trading | Use for confirmation, not primary entry |

---

## Disclaimer

> **This indicator is for educational and informational purposes only.**  
> It does not constitute financial advice. Past performance of any signal
> or strategy is not indicative of future results. Always practise proper
> risk management and consult a qualified financial adviser before trading.
