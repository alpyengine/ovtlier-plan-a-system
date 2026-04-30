# Changelog — Ovtlier Plan A System

All notable changes to this indicator are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Tags use the format `plan-a/vN` (e.g. `plan-a/v1`, `plan-a/v2`).

---

## [plan-a/v2] — 2026-04-30 (Current)

### Added
- `tablePosInput`: 8-position table placement dropdown (Top Right, Top Left,
  Top Center, Bottom Right, Bottom Left, Bottom Center, Middle Right, Middle Left).
- `tablePos` resolver via `switch` statement — mirrors the pattern used in
  the Trend Template and Position Sizer indicators.
- Full high-contrast color palette: 11 named `color` constants replacing
  inline `color.new()` calls throughout the table rendering block.
- Dedicated ticker sub-header row (row 1): shows `syminfo.ticker`,
  `timeframe.period`, and individual SPY/QQQ status (`checkmark`/`x`) inline.
- `f_badge()`, `f_badgeBg()`, `f_badgeTxt()` helper functions for
  consistent PASS/FAIL badge rendering across all condition rows.
- Alternating row backgrounds (`C_ROW_BG` / `C_ROW_ALT_BG`) for improved
  horizontal readability across 5 condition rows.
- Score row (row 7) now displays suggested SL price inline:
  `SL: XX.XX` in light red text alongside the score count.
- Result row (row 8) merged across all 3 columns via `table.merge_cells()`,
  displaying READY / WATCH / NOT READY in `size.normal` text.

### Changed
- Table dimensions: 3 columns x 7 rows (v1) -> 3 columns x 9 rows (v2).
- Table background: `color.new(color.gray, 85)` -> `#0d1117` (deep black).
- C1 row now uses a dedicated dark-green background (`#1a2d1a`) to
  visually mark it as the mandatory gate condition.
- Condition status column: emoji-only (`checkmark`/`dash`) -> solid
  PASS (green badge) / FAIL (red badge) with white text, eliminating
  readability issues at small sizes.
- Score row result: inline `LONG`/`WATCH`/`NO GO` text -> dedicated
  full-width merged result row with color-coded background.
- `indicator()` title string updated: `"Ovtlier Plan A - Swing v1"` ->
  `"Ovtlier Plan A - Swing v2"`.

### Fixed
- Text contrast issue from v1: silver text on light gray background
  was unreadable at `size.tiny` on high-DPI displays. All text now
  renders on dark opaque backgrounds with white or light-colored text.

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
- Scoring system: C1 mandatory, C2-C5 scored 0-4.
  READY (>=3/4), WATCH (2/4), NOT READY (<2 or C1 fail).
- ATR-based Stop Loss line on chart for full signals.
  Formula: SL = Bar Low - (ATR(14) x 1.5).
- 3-column x 7-row scoring table (top-right, fixed position).
- Regime background highlight (teal, 93% transparency).
- EMA fast (9) and slow (21) plotted on chart.
- `plotshape()` signals: triangle-up teal (READY), triangle-up yellow (WATCH).
- `alert()` for dynamic webhook messages on READY and WATCH signals.
- `alertcondition()` for TradingView alert panel (static messages).

---

## Roadmap

- [ ] v3 - Weekly-specific hammer relaxation (optional C5 on Weekly)
- [ ] v3 - RSI divergence detection as bonus condition
- [ ] v4 - Multi-symbol scanner (scan watchlist for Plan A setups)
- [ ] Strategy - Plan A backtest with realistic commission and slippage
