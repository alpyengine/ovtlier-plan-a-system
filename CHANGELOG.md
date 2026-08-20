# Changelog — Ovtlier Plan A System

All notable changes to this indicator are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Tags use the format `plan-a/vN` (e.g. `plan-a/v1`, `plan-a/v2`).

---

## [plan-a/v5.2] — 2026-04-30 (Current)

### Changed — Visual only. No logic changes.

- `C_HEADER_TXT`: `#374151` -> `#111827` (casi negro)
- `C_LABEL_TXT`: `#1a1f2e` -> `#0f1117` (negro puro)
- `C_VALUE_TXT`: `#4b5563` -> `#1f2937` (gris muy oscuro)

Insufficient contrast on light backgrounds at small text sizes.

---

## [plan-a/v5.1] — 2026-04-30

### Changed — Visual only. No logic changes.

- **Palette**: full table color palette replaced with the light theme
  already used in Market Dashboard v5.2. Exact mapping:

  | Constant | v5 (dark) | v5.1 (light) |
  |---|---|---|
  | C_HEADER_BG | #1a2433 | #eef0f6 |
  | C_HEADER_TXT | white | #374151 |
  | C_ROW_BG | #0d1117 | #ffffff |
  | C_ROW_ALT_BG | #141c26 | #f4f5f8 |
  | C_LABEL_TXT | #c9d1d9 | #1a1f2e |
  | C_VALUE_TXT | #58a6ff | #4b5563 |
  | C_PASS_BG | #1a6b3c | #dcfce7 |
  | C_PASS_TXT | white | #14532d |
  | C_FAIL_BG | #8b1a1a | #fee2e2 |
  | C_FAIL_TXT | white | #991b1b |
  | C_WARN_BG | #7a5c00 | #fef3c7 |
  | C_WARN_TXT | (none) | #b45309 |
  | C_READY_BG | #0d5c2e | #bbf7d0 |
  | C_READY_TXT | (none) | #14532d |
  | C_NOTREADY_BG | #6b0f0f | #fecaca |
  | C_NOTREADY_TXT | (none) | #991b1b |
  | C_TICKER_BG | #0f1923 | #eef0f6 |
  | C_TICKER_TXT | #58a6ff | #1d4ed8 |
  | C_OBLIG_BG | #1a2d1a | #f0fdf4 |
  | C_C6_BG | #1a1f2d | #eff6ff |
  | C_STF_BG | #1f1a2d | #f5f3ff |
  | C_C7_BG | #1a2020 | #f0fdfa |

- **C_DIM_TXT** (`#9ca3af`) added: unified color for dimmed text in disabled
  rows (C1b OFF, C7 OFF) replacing multiple inline `color.new(#c9d1d9, 70)` calls.
- **C_READY_TXT** and **C_NOTREADY_TXT**: explicit text colors added for the
  result row and score row — previously hardcoded `color.white` which had poor
  contrast on the new light backgrounds.
- **`tamPanelInput`** string input added (default `"small"`, options:
  `tiny / small / normal / large`), placed in group "🎨 Visual".
  All table cells now use `TS` (primary) or `TS_SM` (secondary/value cells)
  variables resolved from this input, replacing all hardcoded `size.small` /
  `size.tiny` calls. `TS_SM` is one step smaller than `TS`.
- Table border and frame colors updated from black to light gray
  (`#d1d5db` border, `#9ca3af` frame) for visual consistency with the light palette.
- Score row: `color.white` text replaced with `scoreTxt` variable
  (green/amber/dark depending on signal state).
- Result row: `color.white` text replaced with `resultTxtC` variable.
- SL value in score row: `color.new(#ffcdd2, 0)` -> `color.new(#991b1b, 0)`.
- Sub-checks color in C6 and C7 rows: `#4caf50`/`#f44336` ->
  `#15803d`/`#dc2626` (better contrast on light backgrounds).
- SPY/QQQ inline status in ticker row: same color adjustment (`#15803d`/`#dc2626`).
- `indicator()` title: `"Ovtlier Plan A - Swing v5"` ->
  `"Ovtlier Plan A - Swing v5.1"`.
- Version footer text updated to `"Ovtlier Plan A v5.1"`.

### Not changed
- All conditions C1–C7: logic, thresholds, booleans — unchanged.
- All `request.security()` calls — unchanged.
- All chart plots (EMA, SMA, divergence line, SL line) — unchanged.
- All alert strings and `alertcondition()` calls — unchanged.
- Scoring system and `scoreMax` formula — unchanged.

---

## [plan-a/v5] — 2026-04-30

### Added
- C7 Bullish RSI Divergence: `useC7`, `c7Lookback` (2-20), `c7Required` inputs.
- `showDivLine`: dashed lime line connecting the two price lows on divergence.
- `scoreMax` fully dynamic: 5 + TF sup scored (+1) + C7 scored (+1) = max 7.
- Table: 4x13 -> 4x14 rows (new row 9 for C7).
- Config summary row updated with C7 status.

---

## [plan-a/v4] — 2026-04-30

### Added
- C2+: `c2TrendBars` input (default 3, range 1-10). Prior trend context check.
- C1b dual TF: `useSuperTF` + `superTFRequired`. Daily->Weekly, Weekly->Monthly.
- Config summary row and version footer row.

---

## [plan-a/v3] — 2026-04-30

### Added
- C6: SMA 50/200 asset trend filter. VALOR column with live numeric readings.

---

## [plan-a/v2] — 2026-04-30

### Added
- 8-position table placement. High-contrast dark palette. PASS/FAIL badges.

---

## [plan-a/v1] — 2026-04-29

### Added
- Initial release: C1–C5, ATR stop loss, 3x7 table, EMA 9/21, alerts.

---

## Roadmap

- [ ] scanner/v1 — Multi-symbol scanner (screeners/ovtlier_plan_a_scanner.pine)
- [ ] backtest/v1 — Strategy backtest (strategies/ovtlier_plan_a_backtest.pine)
