# Changelog — Ovtlier Plan A System

All notable changes to this indicator are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Tags use the format `plan-a/vN` (e.g. `plan-a/v1`, `plan-a/v2`).

---

## [plan-a/v5] — 2026-04-30 (Current)

### Added

**C7 - Divergencia Alcista RSI (condición bonus):**
- `useC7` boolean input (default ON): activates C7. When OFF the row is
  visible in the table but visually dimmed and has no effect on score or gates.
- `c7Lookback` int input (default 5, range 2-20): the lookback window in bars
  used to find the prior price low and prior RSI low for the divergence check.
  A larger window detects longer-range divergences; a smaller window detects
  faster, tighter ones. The live value `LB:Nb` is shown in the table VALOR column.
- `c7Required` boolean input (default OFF): when ON, C7 acts as a mandatory gate
  — no signal if divergence is absent. When OFF, C7 scores +1 point (puntuada).
- `showDivLine` boolean input (default ON): when a divergence is detected,
  draws a dashed lime line on the chart connecting the prior price low to the
  current low, making the divergence visually obvious on the chart.
- `c7_priceLL` bool: price makes a lower low vs. the prior N-bar low.
- `c7_rsiHL`   bool: RSI makes a higher low vs. the prior N-bar RSI low.
- `c7_divRaw`  bool: both conditions true (raw divergence, ignoring useC7).
- `c7_div`     bool: final C7 pass/fail, respects useC7 toggle.
- `c7RsiDiff`  float: RSI current minus RSI prior low — positive = real divergence.
  Shown in table as `RSIdiff +X.X` for quick visual confirmation.
- `C_C7_BG` color constant (`#1a2020`, dark teal-green): dedicated row background
  for C7, visually distinct from C6 and TF superior rows.
- `scoreC7` int: contributes 1 to score when useC7=ON and c7Required=OFF and divergence detected.
- `scoreMax` is now fully dynamic: 5 base + optional TF sup (+1) + optional C7 (+1) = max 7.

### Changed
- `scoreMax` formula updated: `5 + (useSuperTF and not superTFRequired ? 1 : 0) + (useC7 and not c7Required ? 1 : 0)`.
- Table dimensions: 4x13 (v4) -> 4x14 (v5). New row 9 for C7.
- Config summary row (row 12) now includes C7 status: `C7:OFF`, `C7:punt Nb`, or `C7:OBLIG Nb`.
- Alert string: added `C7div: OK/FAIL/OFF` field.
- `indicator()` title: `"Ovtlier Plan A - Swing v4"` -> `"Ovtlier Plan A - Swing v5"`.

---

## [plan-a/v4] — 2026-04-30

### Added
- C2 improved: `c2TrendBars` input (default 3, range 1-10). N prior bars must
  close above EMA fast before the dip qualifies. Table shows `X/N barras OK` live.
- C1 dual timeframe: `useSuperTF` and `superTFRequired` inputs. Daily -> Weekly,
  Weekly -> Monthly. C1b row in table with individual SPY/QQQ status per TF.
- `f_superTF()` helper resolves the superior TF string automatically.
- `scoreMax` dynamic: 5 or 6 depending on TF sup mode.
- Config summary row (row 11) and version footer (row 12).

### Changed
- Table: 4x11 -> 4x13 rows. C2 label: "C2 Dip" -> "C2 Dip+".
- READY/WATCH thresholds derived from `scoreMax` dynamically.

---

## [plan-a/v3] — 2026-04-30

### Added
- C6: `close > SMA(50)`, `close > SMA(200)`, `SMA(50) > SMA(200)`.
- VALOR column (col 2) with live numeric readings per condition.
- `distSma50`, `distSma200`, `rvolRatio`, `slPct` live values.
- SMA 50/200 plotted. Version footer row.

### Changed
- Max score: 4->5. READY: >=3/4 -> >=4/5. Table: 3x9 -> 4x11.

---

## [plan-a/v2] — 2026-04-30

### Added
- 8-position table placement. High-contrast palette. PASS/FAIL badges.
- Ticker row with SPY/QQQ individual status. Merged result row.

### Changed
- Table: 3x7 -> 3x9. Background: gray -> `#0d1117`.

---

## [plan-a/v1] — 2026-04-29

### Added
- Initial release: C1 (regime), C2 (pullback), C3 (RSI), C4 (RVOL), C5 (hammer).
  ATR stop loss. 3x7 table. EMA 9/21 plotted. Dynamic and static alerts.

---

## Roadmap

- [ ] scanner/v1 - Multi-symbol scanner (new file: screeners/ovtlier_plan_a_scanner.pine)
- [ ] backtest/v1 - Strategy backtest with commission and slippage (new file: strategies/ovtlier_plan_a_backtest.pine)
