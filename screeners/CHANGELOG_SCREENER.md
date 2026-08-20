# Changelog — Ovtlier Plan A Screener

All notable changes to this screener are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Tags use the format `screener/vN` (e.g. `screener/v1`).

---

## [screener/v1] — 2026-04-30 (Current)

### Added

- Initial release of the Plan A multi-symbol screener.
- Evaluates **6 configurable symbols** simultaneously via `input.symbol()` —
  editable from the indicator settings panel without touching code.
- **Full 7-condition evaluation** (C1–C7) per symbol, identical logic to
  the Plan A indicator v5.2 — enforced by sharing the same `f_evalSymbol()`
  function that mirrors v5.2 calculations exactly.
- **C1 broad market regime** (SPY + QQQ above EMA) evaluated once globally —
  same result for all 6 symbols simultaneously (regime is market-wide, not
  symbol-specific).
- **C1b higher-TF regime** optional gate, also evaluated globally.
- `f_evalSymbol()` UDF: receives pre-fetched OHLCV + ATR data for each symbol
  and returns `[score, fullSignal, partialSignal, slPct, c2..c7 booleans]`.
  This single function guarantees parity with the indicator — not a
  reimplementation, a direct port of the v5.2 calculation block.
- **request.security() budget: 10 calls maximum** (2 SPY/QQQ global +
  2 TF superior global + 6 symbol tuples). Well within the 40-call limit.
  Each symbol uses one tuple call fetching `[close, open, high, low, volume, atr]`.
- **Table layout**: 7 columns × 8 rows.
  - Row 0: ticker headers per symbol
  - Row 1: C1 regime status (shared badge — same value per column)
  - Row 2: Score X/scoreMax
  - Row 3: Mini-check bar — digits for passing conditions, dash for failing
    (format: `2345--` means C2/C3/C4/C5 pass, C6/C7 fail)
  - Row 4: SL% (stop loss distance as % of close)
  - Row 5: Result badge — READY / WATCH / NO GO
  - Row 6: Current close price per symbol
  - Row 7: Version footer (merged, full width)
- `f_resultBadge()`, `f_resultBg()`, `f_resultTxt()`: helper functions for
  result badge rendering — consistent with indicator v5.2 badge colors.
- `f_condBar()`: compact condition summary string per symbol. Shows which
  of C2-C7 pass (digit) or fail (dash) in a single cell.
- `f_shortTicker()`: extracts short ticker from `EXCHANGE:TICKER` format
  for compact display in column headers.
- `scoreMax` dynamic: identical formula to indicator v5.2
  (5 + TF sup scored + C7 scored).
- Consolidated alert: fires once per bar close when ANY symbol reaches READY.
  Message lists all READY symbols with their score.
- `alertcondition()` for TradingView alert panel.
- Light palette (identical to indicator v5.2): `#eef0f6` header,
  `#ffffff`/`#f4f5f8` alternating rows, PASS/FAIL/WATCH/READY/NOTREADY badges.
- `tamPanelInput`: text size selector (tiny/small/normal/large).
- `tablePosInput`: 8-position table placement.
- `overlay = false`: screener renders in a separate pane, not overlaid on chart.
- `max_bars_back = 500`: consistent with indicator v5.2.

### Acceptance criteria (must verify before using in production)

The score shown by this screener for any given symbol must match **exactly**
the score shown by the Plan A indicator v5.2 on that same symbol and timeframe.
Verify on 2-3 known tickers by opening both side by side.

Key parity points to check:
- C2 `c2TrendBars` input must be set to the same value in both scripts.
- C7 `c7Lookback` must match.
- `useSuperTF` / `superTFRequired` must match.
- `useC7` / `c7Required` must match.
- The chart timeframe must match.

---

## Roadmap

- [ ] v2 - Sort symbols by score descending in table
- [ ] v2 - Color-code score cell by proximity to READY threshold
- [ ] backtest/v1 - Strategy backtest (strategies/ovtlier_plan_a_backtest.pine)
