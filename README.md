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
| `indicators/ovtlier_plan_a.pine` | Main Plan A swing signal indicator | ✅ Active (v2) |

---

## Plan A — Core Conditions

The indicator evaluates **5 conditions**, one mandatory and four scored:

| # | Condition | Type | Logic |
|---|-----------|------|-------|
| C1 | Broad Market Regime | **Mandatory** | SPY + QQQ both above EMA(20) |
| C2 | Pullback to Value Zone | Scored | Low ≤ EMA(9), Close ≥ EMA(21)×0.98 |
| C3 | Relative Weakness (RSI) | Scored | RSI(14) < 40 **or** RSI(2) < 10 |
| C4 | Relative Volume (RVOL) | Scored | Volume > 1.5× average |
| C5 | Reversal Candle (Hammer) | Scored | Lower wick ≥ 2× body |

**Signal levels:**
- 🟢 **READY** — C1 pass + 3 or 4 of C2–C5
- 🟡 **WATCH** — C1 pass + exactly 2 of C2–C5
- 🔴 **NOT READY** — C1 fail or score < 2

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

Version history is tracked entirely through Git commits and tags — no archive folder needed.

---

## How to Use

1. Open TradingView → Pine Editor → paste the content of `indicators/ovtlier_plan_a.pine`
2. Add to chart on a **Daily or Weekly** timeframe
3. Read the on-chart table (top-right by default, configurable)
4. Only trade READY signals — never force a trade when C1 (regime) fails

---

## Risk Management

Every signal includes a suggested **Stop Loss** calculated as:
```
SL = Bar Low − (ATR(14) × multiplier)
```
Default multiplier: **1.5×**. Adjustable in indicator inputs.

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
