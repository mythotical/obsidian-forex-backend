# Installation Guide — Obsidian Forex

Follow these steps to load the **Obsidian Forex** indicator into TradingView.

---

## Prerequisites

- A [TradingView](https://www.tradingview.com) account (free or paid)
- A chart open for any forex pair, XAUUSD, NAS100, BTCUSD, or any instrument

---

## Step-by-Step Installation

### 1. Open the Pine Script Editor

1. Open TradingView in your browser
2. Click **Pine Editor** in the bottom toolbar  
   *(If you don't see it, click **More** → **Pine Script editor**)*

### 2. Copy the Script

1. Open [`src/obsidian_forex_overlay.pine`](../src/obsidian_forex_overlay.pine) in this repository
2. Select all the code (`Ctrl + A` / `Cmd + A`) and copy it (`Ctrl + C` / `Cmd + C`)

### 3. Paste into the Editor

1. Inside the Pine Editor, click the **default code** and select all (`Ctrl + A`)
2. Paste the copied script (`Ctrl + V`)

### 4. Save the Script

1. Click **Save** (floppy-disk icon or `Ctrl + S`)
2. Give it a name, e.g., **Obsidian Forex**
3. Click **Save**

### 5. Add to Chart

1. Click **Add to chart** (play button `▷`)
2. The indicator will appear as an overlay on your current chart
3. The dashboard table will appear in the top-right corner

---

## Setting Up Alerts

### BUY / SELL Alerts

1. Right-click any **BUY ▲** or **SELL ▼** label on the chart
2. Select **Add Alert on Obsidian Forex**  
   *— or —*  
   Click the **Alarm clock** icon in the top toolbar → **Create Alert**

3. In the alert dialog:
   - **Condition**: `Obsidian Forex` → choose one of:
     - `Obsidian Forex — BUY Signal`
     - `Obsidian Forex — SELL Signal`
     - `Obsidian Forex — Trend Change`
   - **Trigger**: `Once Per Bar Close` (recommended)
   - Set your notification preferences (email, popup, mobile)
4. Click **Create**

> The dynamic `alert()` calls inside the script fire automatically on bar close
> and include the pair name, confluence score, and SL/TP values in the message.

---

## Recommended Chart Settings

| Setting | Recommended Value |
|---|---|
| Chart type | Candlestick |
| Theme | Dark |
| Timeframe | 5 m (primary scalping) |
| Auto-scale | On |

---

## Multi-Timeframe Note

The script always analyses 5 m, 10 m, 30 m, and 1 H data regardless of the
timeframe you are currently viewing. This means:

- On a **1-minute chart** you will still see the full MTF analysis
- On a **4-hour chart** the signal is still driven by the same four fixed
  timeframes (not the 4 H)

To customise the four timeframes, go to **Settings → Timeframes** in the
indicator settings panel.

---

## Troubleshooting

| Issue | Solution |
|---|---|
| "Script has too many calls to `request.security()`" | This should not occur; the indicator uses exactly 4 `request.security()` calls. If you see this, ensure you are running **Pine Script v5**. |
| Signals not appearing | Check that the chart has enough historical data (at least 200 bars). |
| Dashboard not visible | Enable **Show Dashboard** in indicator settings. |
| Lines cluttering the chart | Disable **Show SL / TP Lines** in indicator settings. |
| Too many signals | Increase **Min Bars Between Signals** (default 5). |
| No signals on low-liquidity instruments | Lower **BUY Threshold** to 70 / raise **SELL Threshold** to 30. |
