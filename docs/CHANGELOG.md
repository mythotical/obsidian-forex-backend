# Changelog — Obsidian Forex

All notable changes to this project will be documented in this file.  
Format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [2.1.0] — 2026-03-03

### Fixed
- **SELL signals never firing** — the confluence scoring system was bullish-biased,
  preventing the score from dropping below the SELL threshold during genuine
  bearish moves.

### Changed
- **`src/obsidian_forex_overlay.pine`** — Rebranded to **OBF v2 SCALPER**
  - Indicator title updated to `"OBF v2 SCALPER"` / shorttitle `"OBF v2"`
  - Dashboard header updated to `"OBF v2 SCALPER"`
  - **Symmetric neutral scores** — reduced neutral fallback values so bearish
    conditions push the score toward 0 as aggressively as bullish pushes toward 100:
    - EMA ribbon neutral: `10.0` → `7.5`
    - RSI neutral: `7.5` → `5.0`
    - MACD neutral: `7.5` → `5.0`
    - Stochastic RSI neutral: `7.5` → `5.0`
    - Bollinger Bands neutral: `5.0` → `2.5`
  - **Directional volume bonus** — replaced VWMA-vs-SMA proxy with real
    volume-above-average detection; high volume on a bearish candle now scores
    `0` (helping score drop) instead of giving a neutral positive contribution:
    `volBonus = volAboveAvg ? (close < open ? 0.0 : 10.0) : 5.0`
  - **Directional PA bonus** — price-action candle direction now penalises
    bearish bars: `paBonus = bullPA ? 10.0 : bearPA ? -5.0 : 2.5`
  - **ADX bonus** — added direction-agnostic trend-strength bonus via `ta.dmi()`;
    strong trends (ADX > 15) add `5.0` regardless of direction:
    `adxBonus = isTrending ? 5.0 : 0.0`
  - **SELL threshold default raised** from `25` → `28` to make SELL signals
    slightly easier to trigger in mixed-but-bearish environments

---

## [1.0.0] — 2026-03-02

### Added
- **`src/obsidian_forex_overlay.pine`** — Full Pine Script v5 overlay indicator
  - Multi-Timeframe Analysis (MTF): 5 m, 10 m, 30 m, 1 H via `request.security()`
  - EMA Ribbon (8, 21, 55) with semi-transparent fill
  - Supertrend (ATR 10, mult 2.0) on all four timeframes
  - RSI (14) with overbought / oversold scoring bands
  - MACD (12, 26, 9) crossover + histogram direction scoring
  - Stochastic RSI (14, 14, 3, 3) for precise scalper entry timing
  - Volume (VWMA vs SMA-20) participation confirmation
  - Bollinger Bands (20, 2.0) squeeze and breakout detection
  - Higher High / Lower Low market-structure detection
  - Fair Value Gap (FVG) detection
  - Order Block / institutional-zone detection
  - Confluence scoring system (0–100) with weighted MTF aggregation
  - BUY / SELL / NEUTRAL signal thresholds (configurable)
  - Anti-whipsaw gate (configurable minimum bars between signals)
  - Dynamic SL / TP levels (ATR-based, instrument-adjusted)
  - Semi-transparent trend background color
  - Entry labels with confluence score overlay
  - SL / TP horizontal lines (20-bar projection)
  - Dashboard panel (top-right table) with live signal data
  - Instrument auto-detection: FOREX / GOLD / INDEX / CRYPTO
  - Instrument-specific SL multipliers (Gold ×1.5, Index ×1.3, Crypto ×1.4)
  - Dynamic `alert()` calls with full signal details (pair, score, SL, TP)
  - `alertcondition()` entries for BUY, SELL, and Trend Change
  - Hidden `plot()` for confluence score (queryable by alert messages)

- **`src/config.md`** — Detailed input reference and per-market tuning tips
- **`docs/INSTALL.md`** — Step-by-step TradingView installation and alert setup
- **`docs/STRATEGY.md`** — Full explanation of signal logic, scoring, and usage
- **`docs/CHANGELOG.md`** — This file
- **`README.md`** — Updated with project overview, features, install summary,
  usage guide, and disclaimer

---

## Future Roadmap

- [ ] Optional divergence detection (RSI and MACD)
- [ ] Session filter (London / New York / Tokyo) to skip low-liquidity windows
- [ ] Confluence score trend line (secondary non-overlay pane)
- [ ] Configurable TP1 / TP2 / TP3 RR ratios
- [ ] Visual back-test statistics (win rate estimate on historical signals)
- [ ] Webhook-ready alert message templates for trading-bot integration
