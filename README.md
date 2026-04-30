# Ovtlier Plan A — Swing Indicator System

> Pine Script v6 indicators for TradingView implementing Christopher Uhl's **Plan A** swing trading methodology.

📖 [Versión en español](./README_ES.md)

---

## What is Plan A?

Plan A is the swing trading framework taught at **Ovtlier University** by Christopher Uhl. It is designed for **Daily and Weekly timeframes** and focuses on buying high-quality pullbacks within confirmed bull-market regimes.

Unlike Plan M (intraday momentum anomalies), Plan A operates on larger timeframes and requires patience — waiting for the price to "breathe" back to institutional support zones before entering.

---

## Indicators in this Repository

| File | Description | Status |
|------|-------------|--------|
| `indicators/ovtlier_plan_a.pine` | Main Plan A swing signal indicator | ✅ Active (v3) |

---

## Plan A — Core Conditions

The indicator evaluates **6 conditions**: one fixed mandatory gate, one optional mandatory gate, and four/five scored conditions.

| # | Condition | Type | Logic |
|---|-----------|------|-------|
| C1 | Broad Market Regime | **Mandatory** | SPY + QQQ both above EMA(20) |
| C2 | Pullback to Value Zone | Scored | Low ≤ EMA(9), Close ≥ EMA(21)×0.98 |
| C3 | Relative Weakness (RSI) | Scored | RSI(14) < 40 **or** RSI(2) < 10 |
| C4 | Relative Volume (RVOL) | Scored | Volume > 1.5× average |
| C5 | Reversal Candle (Hammer) | Scored | Lower wick ≥ 2× body |
| C6 | Asset Trend Filter | Scored / Optional* | Close > SMA(50), Close > SMA(200), SMA(50) > SMA(200) |

*C6 can be toggled to mandatory via the `C6 obligatoria` input. Default: scored (0 or 1 point).

**Signal levels (0–5 scoring system):**
- 🟢 **READY** — C1 pass + 4 or 5 of C2–C6
- 🟡 **WATCH** — C1 pass + exactly 3 of C2–C6
- 🔴 **NOT READY** — C1 fail or score < 3

---

## On-Chart Table — Columns (v3)

| Column | Content |
|--------|---------|
| CONDITION | Code and name (C1–C6) |
| DESCRIPTION | Summarised check logic |
| VALUE | **Live numeric reading** (RSI, RVOL ratio, SMA distance %, etc.) |
| STATUS | PASS / FAIL badge |

---

## Repository Structure

```
ovtlier-plan-a-system/
├── indicators/
│   └── ovtlier_plan_a.pine       ← active version
├── strategies/                    ← future backtests
├── libs/                          ← future shared libraries
├── README.md
├── README_ES.md
├── CHANGELOG.md
└── LICENSE
```

---

## How to Use

1. Open TradingView → Pine Editor → paste the content of `indicators/ovtlier_plan_a.pine`
2. Add to chart on a **Daily or Weekly** timeframe
3. Read the on-chart table (position configurable from inputs)
4. Only trade READY signals — never force a trade when C1 (regime) fails
5. Check C6: if the asset trades below SMA 50/200, the signal is lower quality

---

## Risk Management

Every signal includes a suggested **Stop Loss** calculated as:
```
SL = Bar Low − (ATR(14) × multiplier)
```
Default multiplier: **1.5×**. The table shows both the absolute level and the percentage distance from the current close.

> ⚠️ This indicator is an educational tool. It does not constitute financial advice. Always apply your own risk management rules before entering any position.

---

## Plan A vs Plan M

| | Plan A | Plan M |
|---|---|---|
| Timeframe | Daily / Weekly | Intraday (15m / 1H) |
| Hold period | 3–10 days | Intraday / 1–2 days |
| Core logic | Buy the dip in bull regime | Momentum anomaly detection |
| Risk profile | Swing, defined risk | Intraday, faster stops |

---

## Versioning

- **Major versions** (`v1`, `v2`…) introduce significant new features or structural changes.
- **Minor versions** (`v1.1`, `v2.1`…) are refinements or bug fixes within the same feature set.

Full version history is in [CHANGELOG.md](./CHANGELOG.md).

---

## Author

Built on the teachings of **Christopher Uhl — Ovtlier University**.
Coded by [alpyengine](https://github.com/alpyengine).

---

## License

[Mozilla Public License 2.0](./LICENSE) · © Ovtlier Plan A System contributors.
Based on the Ovtlier University methodology by Christopher Uhl. For educational purposes.
