# Changelog — Ovtlier Plan A System

All notable changes to this indicator are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Tags use the format `plan-a/vN` (e.g. `plan-a/v1`, `plan-a/v2`).

---

## [plan-a/v4] — 2026-04-30 (Current)

### Added

**C2 Pullback mejorado — ventana de tendencia previa configurable:**
- `c2TrendBars` input (default 3, range 1-10): number of prior bars that must
  have closed above EMA fast before the current pullback qualifies.
  Prevents C2 from triggering in sideways/ranging markets with no clear
  prior uptrend. The table VALOR column shows `X/N barras OK` live,
  making it immediately visible how many bars passed the trend context check.
- `c2_priorTrend` bool: true when `barsAboveEma >= c2TrendBars`.
- `c2_touchEma` bool: unchanged from v3 (low <= EMA fast, close >= EMA slow x 0.98).
- C2 now requires BOTH conditions: prior trend context AND the actual dip touch.
  Condition renamed from "C2 Dip" to "C2 Dip+" in table to signal the upgrade.

**C1 doble timeframe — régimen confirmado en TF superior:**
- `useSuperTF` boolean input (default OFF): activates the higher-timeframe
  regime check. When ON, SPY+QQQ are also evaluated against their EMA
  on the timeframe above the current chart (Daily -> Weekly, Weekly -> Monthly).
- `superTFRequired` boolean input (default OFF): when ON, the higher-TF regime
  acts as a second mandatory gate alongside C1 — no signal fires if it fails.
  When OFF, it contributes +1 point to the score (puntuada mode).
- `f_superTF()` helper function: resolves the superior timeframe string
  from the current chart timeframe automatically (no manual entry needed).
- `spyBullSup`, `qqqBullSup`, `c1_superTF` bools: higher-TF regime sub-checks.
- `superTFGate` bool: combined gate that respects both `useSuperTF` and
  `superTFRequired` settings.
- C1b row added to table (row 3): shows TF label, SPY/QQQ sub-status,
  mode badge (OBLIGATORIA / PUNTUADA +1 / OFF), and PASS/FAIL badge.
  Row uses `C_STF_BG` (deep purple `#1f1a2d`) to visually distinguish it
  from C1 base row. Row is visually dimmed when `useSuperTF` is OFF.
- `C_STF_BG` color constant added (`#1f1a2d`).
- When `useSuperTF` ON and `superTFRequired` OFF: `scoreMax` increases from
  5 to 6; READY threshold becomes >= 5/6; WATCH threshold becomes == 4/6.
- Config summary row added (row 11): merged, shows active settings for C2, C6,
  and TF superior in a single compact line (`C2:3b  C6:punt  TFsup:OFF`).
- Alert string expanded: now includes `C2bars: X/N` and `TFsup: OK/FAIL/OFF`.

### Changed
- Table dimensions: 4 columns x 11 rows (v3) -> 4 columns x 13 rows (v4).
  Two new rows: C1b TF superior (row 3), config summary (row 11).
- `scoreMax` is now dynamic: 5 when TF superior is OFF or mandatory;
  6 when TF superior is ON and puntuada.
- READY/WATCH thresholds derived from `scoreMax` dynamically:
  READY >= scoreMax-1; WATCH == scoreMax-2.
- C2 table label: `"C2 Dip"` -> `"C2 Dip+"` to signal the logic upgrade.
- C2 VALOR cell: now shows trend context result (`X/N barras OK`) with
  color coding — blue when trend OK, red-tinted when trend context fails.
- `indicator()` title: `"Ovtlier Plan A - Swing v3"` -> `"Ovtlier Plan A - Swing v4"`.

---

## [plan-a/v3] — 2026-04-30

### Added
- **C6 - Asset Trend Filter**: three simultaneous sub-conditions:
  `close > SMA(50)`, `close > SMA(200)`, `SMA(50) > SMA(200)`.
- `sma50Len`, `sma200Len` configurable inputs.
- `c6Required` boolean: toggle C6 as mandatory gate (default OFF).
- VALOR column (col 2) added to table with live numeric readings per condition.
- `distSma50`, `distSma200`: % distance of close from each SMA, shown live.
- `rvolRatio`: volume/avg ratio shown live in C4.
- `slPct`: SL distance as % of close shown in score row.
- SMA 50 (purple) and SMA 200 (yellow) plotted on chart.
- Version footer row (row 10) merged across all 4 columns.

### Changed
- Max score: 4 -> 5. READY: >= 3/4 -> >= 4/5. WATCH: == 2/4 -> == 3/5.
- Table: 3x9 (v2) -> 4x11 (v3). Column 2 (VALOR) added.
- SL display: absolute price -> `SL XX.XX  (-X.X%)`.
- Alert string: added C6 status and SL% fields.

### Fixed
- Unicode removed from `f_badge()` and `alertcondition()` to prevent encoding issues.

---

## [plan-a/v2] — 2026-04-30

### Added
- `tablePosInput`: 8-position table placement dropdown.
- Full high-contrast color palette: 11 named `color` constants.
- Ticker sub-header row with SPY/QQQ individual status.
- `f_badge()`, `f_badgeBg()`, `f_badgeTxt()` helper functions.
- Alternating row backgrounds. Score row with SL. Merged result row.

### Changed
- Table: 3x7 -> 3x9. Background: gray -> `#0d1117`. C1 row: dark-green bg.
- Status: emoji-only -> solid PASS/FAIL badges with white text.

### Fixed
- Text contrast: silver on light gray unreadable at `size.tiny` on high-DPI.

---

## [plan-a/v1] — 2026-04-29

### Added
- Initial release. C1 (SPY+QQQ regime), C2 (pullback), C3 (RSI), C4 (RVOL),
  C5 (hammer). ATR stop loss. 3x7 table. EMA 9/21 on chart. Alerts.

---

## Roadmap

- [ ] v5 - Multi-symbol scanner (scan watchlist for Plan A setups)
- [ ] Strategy - Plan A backtest with realistic commission and slippage
