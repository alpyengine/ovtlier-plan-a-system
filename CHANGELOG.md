# Changelog — Ovtlier Plan A System

All notable changes to this indicator are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Tags use the format `plan-a/vN` (e.g. `plan-a/v1`, `plan-a/v2`).

---

## [plan-a/v3] — 2026-04-30 (Current)

### Added
- **C6 - Asset Trend Filter**: new scored condition checking three simultaneous sub-conditions:
  - `close > SMA(50)` — price above mid-term trend
  - `close > SMA(200)` — price above primary trend
  - `SMA(50) > SMA(200)` — healthy structure (Golden Cross zone)
  All three must be true for C6 to pass. Prevents Plan A signals on
  structurally bearish assets even when the broad regime (C1) is bullish.
- `sma50Len` input (default 50): configurable length for the mid-term SMA.
- `sma200Len` input (default 200): configurable length for the primary SMA.
- `c6Required` boolean input (default OFF): when ON, C6 acts as a second
  mandatory gate alongside C1 — no signal fires if C6 fails, regardless of score.
  When OFF, C6 scores as a standard technical condition (0 or 1 point).
- `C_VALUE_TXT` color constant (`#58a6ff`, light blue): numeric value cells column.
- `C_C6_BG` color constant (`#1a1f2d`, deep navy): dedicated row background for C6.
- `distSma50` and `distSma200`: percentage distance of close from each SMA,
  displayed live in the table value column.
- `rvolRatio`: current volume / average volume ratio, shown live in C4 value cell.
- `slPct`: stop-loss distance as percentage of close, shown in score row.
- SMA 50 plotted on chart (purple, width 1).
- SMA 200 plotted on chart (yellow, width 2).
- Version footer row (row 10): merged across all 4 columns, indicator name + date.

### Changed
- Scoring system: max score raised from 4 (C2-C5) to 5 (C2-C6).
  READY threshold: >= 3/4 -> >= 4/5. WATCH threshold: == 2/4 -> == 3/5.
- Table dimensions: 3 columns x 9 rows (v2) -> 4 columns x 11 rows (v3).
  Column 2 (VALOR) added between description and status badge.
- All condition rows now display live numeric values in column 2:
  C2 shows Low vs EMA prices; C3 shows RSI14 and RSI2 readings;
  C4 shows RVOL ratio; C5 shows wick-to-body ratio; C6 shows D50% and D200%.
- Score row SL: absolute price only -> `SL XX.XX  (-X.X%)` with percentage.
- Alert message: added `C6: OK/FAIL` and `SL%` fields to alert string.
- `indicator()` title: `"Ovtlier Plan A - Swing v2"` -> `"Ovtlier Plan A - Swing v3"`.

### Fixed
- Unicode characters removed from `f_badge()` output and `alertcondition()`
  messages to prevent terminal and webhook encoding issues.

---

## [plan-a/v2] — 2026-04-30

### Added
- `tablePosInput`: 8-position table placement dropdown (Top Right, Top Left,
  Top Center, Bottom Right, Bottom Left, Bottom Center, Middle Right, Middle Left).
- `tablePos` resolver via `switch` statement.
- Full high-contrast color palette: 11 named `color` constants.
- Dedicated ticker sub-header row (row 1): ticker, timeframe, SPY/QQQ status.
- `f_badge()`, `f_badgeBg()`, `f_badgeTxt()` helper functions.
- Alternating row backgrounds for improved horizontal readability.
- Score row displays suggested SL price inline.
- Result row (row 8) merged across all 3 columns via `table.merge_cells()`.

### Changed
- Table dimensions: 3x7 (v1) -> 3x9 (v2).
- Table background: `color.new(color.gray, 85)` -> `#0d1117` (deep black).
- C1 row uses dedicated dark-green background (`#1a2d1a`).
- Status column: emoji-only -> solid PASS/FAIL badges with white text.
- `indicator()` title: `"Ovtlier Plan A - Swing v1"` -> `"Ovtlier Plan A - Swing v2"`.

### Fixed
- Text contrast: silver on light gray unreadable at `size.tiny` on high-DPI displays.

---

## [plan-a/v1] — 2026-04-29

### Added
- Initial release of the Plan A Swing Indicator.
- **C1 - Broad Market Regime**: SPY + QQQ both above EMA(20).
  Anti-repainting via `close[1]` + `barmerge.lookahead_on`.
- **C2 - Pullback to Value Zone**: Low <= EMA(9) and Close >= EMA(21) x 0.98.
- **C3 - Relative Weakness**: RSI(14) < 40 or RSI(2) < 10.
- **C4 - Relative Volume**: Volume > 1.5x SMA(20) of volume.
- **C5 - Reversal Candle**: Lower wick >= 2x body (hammer pattern).
- Scoring: C1 mandatory, C2-C5 scored 0-4. READY (>=3/4), WATCH (2/4).
- ATR-based Stop Loss line. Formula: SL = Low - (ATR(14) x 1.5).
- 3-column x 7-row scoring table (top-right, fixed).
- Regime background highlight (teal, 93% transparency).
- EMA 9 and EMA 21 plotted on chart.
- `plotshape()` signals: triangle-up teal (READY), yellow (WATCH).
- `alert()` dynamic webhook messages. `alertcondition()` static alerts.

---

## Roadmap

- [ ] v4 - C2 improved: pullback with prior uptrend context (avoid ranging markets)
- [ ] v4 - C1 dual timeframe: Daily regime confirmed on Weekly; Weekly on Monthly
- [ ] v5 - Multi-symbol scanner (scan watchlist for Plan A setups)
- [ ] Strategy - Plan A backtest with realistic commission and slippage
