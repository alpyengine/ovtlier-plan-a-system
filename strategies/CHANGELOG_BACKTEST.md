# Changelog — Ovtlier Plan A Backtest

All notable changes to this strategy are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Tags use the format `backtest/vN` (e.g. `backtest/v1`).

---

## [backtest/v1] — 2026-04-30 (Current)

### Added

**Strategy declaration:**
- `pyramiding = 0`: one position at a time — no stacking.
- `default_qty_type = strategy.percent_of_equity`, `default_qty_value = 100`:
  full-equity position sizing, consistent with Plan ETF methodology.
- `commission_type = strategy.commission.percent`, `commission_value = 0.05`:
  Interactive Brokers rate, same as all prior campaigns (ATR, SAR, DOC,
  Liquidity Sweeps Long-Only).
- `slippage = 1`: 1 tick slippage per side, equivalent to ~0.05% on typical
  US equities. Same assumption as prior campaigns.

**Entry signal — READY only:**
- Identical 7-condition logic (C1–C7) and scoring system as indicator v5.2.
- `entrySignal`: fires when `score >= scoreMax - 1` (READY threshold).
  WATCH signals (score == scoreMax - 2) excluded from this campaign —
  starting with highest-quality signals to establish a clean baseline.
- Entry placed at bar close via `strategy.entry()`.

**Exit mode B — Trailing ATR:**
- `trailAtrMult` input (default 2.0): trailing distance in ATR units.
  Wider than the initial SL multiplier (1.5) to give winners room to run.
- `strategy.exit()` with `stop = slInitial` (initial hard stop) +
  `trail_offset = trailTicks` (trailing distance converted to ticks via
  `syminfo.mintick`). The trailing stop ratchets up as price rises,
  never moves down.
- `slInitial = low - atrSlMult × ATR(14)`: initial stop below the entry bar low.

**Exit mode C — EMA crossunder:**
- `exitEmaMode` input: select EMA Rapida (9) or EMA Lenta (21) as the exit trigger.
- `strategy.close()` fires when `close < exitEma` while in a long position.
- `strategy.exit()` with `stop = slInitial` remains active as the hard floor —
  protects against gaps that skip the EMA trigger.

**Max bars limit (anti-zombie, both modes):**
- `maxBarsHold` input (default 10, range 1–50): maximum bars to hold a position.
  Matches the 3–10 day holding period stated in Plan A README.
  If neither the trailing stop nor the EMA cross fires within N bars,
  `strategy.close()` exits at market with comment `"MaxBars N"`.
  Design decision validated by the Liquidity Sweeps Long-Only campaign
  where zombie positions materially degraded Profit Factor.

**Dashboard (top-right, updates every bar — no barstate gate):**
- 2 columns × 13 rows. Light palette (white background, dark text).
- Displays: ticker/TF, regime C1 status, current score, position state
  (LONG Nb / Flat), SL level, bars in trade vs max, closed trades count,
  win rate, Profit Factor (green if >= 1.2), net profit %, max drawdown %,
  cost summary.
- No `barstate` gate — table updates unconditionally every bar so Bar Replay
  works correctly (verified pattern from psar backtest campaigns).

**Visual plots:**
- EMA 9 (blue), EMA 21 (orange), SMA 50 (purple), SMA 200 (yellow).
- Entry shape: triangle-up teal at belowbar on entry signal.
- SL initial line: red dashed, visible only while in a position (plot.style_linebr).
- Exit EMA line: fuchsia, visible only in Mode C while in a position.

**Acceptance criteria (Profit Factor target):**
- PF > 1.2× Buy & Hold, consistent with all prior campaigns.
- Compare Mode B vs Mode C on the same symbol and timeframe before drawing
  conclusions — that comparison is the primary deliverable of this campaign.

### Campaign checklist before publishing results

- [ ] Verify that entry score matches Plan A indicator v5.2 on same symbol/TF
      (same parity check as the screener/v1 acceptance criteria).
- [ ] Run Mode B on at least 3 symbols (e.g. AAPL, MSFT, NVDA) — Daily TF.
- [ ] Run Mode C on the same 3 symbols with same parameters.
- [ ] Compare PF, Win Rate, Max DD, and trade count between B and C.
- [ ] Compare Net Profit % vs Buy & Hold (check "Buy & Hold Return" in
      TradingView Strategy Tester).
- [ ] If PF < 1.2 on both modes, review maxBarsHold and trailAtrMult before
      changing entry logic — exit optimization first.

---

## Roadmap

- [ ] backtest/v2 - Exit mode D: trailing + fixed target combination
      (only if v1 B or C does not meet PF > 1.2 threshold)
- [ ] backtest/v3 - WATCH signals campaign (score == scoreMax-2) for
      sample size comparison
