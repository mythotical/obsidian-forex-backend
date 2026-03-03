# Changelog — Obsidian Forex

All notable changes to this project will be documented in this file.  
Format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.1.0] — 2026-03-03

### Changed
- **BUY threshold** lowered from 75 to **73** (mirrors the inverse of the SELL threshold at 25,
  giving both signals equal sensitivity to the confluence score).
- **Dead-cat-bounce guard added** — the only BUY-specific filter.  Price must NOT be more than
  3 % below its 20-bar swing high while both the 30 m and 1 H timeframes are still bearish.
  This prevents buying into brief relief rallies during sustained crashes while leaving the signal
  path fully open during genuine uptrends.
- **SELL logic is 100 % unchanged** — every SELL-related line is identical to v1.0.0.

### Fixed
- BUY signals now fire during strong uptrends (EMA ribbon green, price above all EMAs) that
  were previously suppressed by an overly strict filter chain.

### Dashboard
- Added **"Dead Cat"** row (row 13) showing `✓ OK` or `⚠ DEAD CAT` to make the active
  BUY filter state visible at a glance.

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
