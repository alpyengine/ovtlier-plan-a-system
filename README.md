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
| `indicators/ovtlier_plan_a.pine` | Main Plan A swing signal indicator | ✅ Active (v4) |

---

## Plan A — Core Conditions

The indicator evaluates **6 conditions** plus an optional higher-timeframe regime check.

| # | Condition | Type | Logic |
|---|-----------|------|-------|
| C1 | Broad Market Regime | **Mandatory** | SPY + QQQ both above EMA(20) |
| C1b | Higher-TF Regime | Optional (scored or mandatory) | SPY + QQQ in regime on Weekly (if Daily) or Monthly (if Weekly) |
| C2+ | Pullback with Trend Context | Scored | N prior bars closed above EMA fast + Low touches EMA |
| C3 | Relative Weakness (RSI) | Scored | RSI(14) < 40 **or** RSI(2) < 10 |
| C4 | Relative Volume (RVOL) | Scored | Volume > 1.5× average |
| C5 | Reversal Candle (Hammer) | Scored | Lower wick ≥ 2× body |
| C6 | Asset Trend Filter | Scored / Optional* | Close > SMA(50), Close > SMA(200), SMA(50) > SMA(200) |

**Signal levels:**
- 🟢 **READY** — C1 pass + score >= scoreMax−1
- 🟡 **WATCH** — C1 pass + score == scoreMax−2
- 🔴 **NOT READY** — C1 fail or score below threshold

*`scoreMax` is dynamic: 5 (higher TF OFF or mandatory) or 6 (higher TF ON and scored).*

---

## Configurable Inputs

### 📊 Broad Market Regime C1
| Input | Default | Description |
|-------|---------|-------------|
| EMA Régimen SPY | 20 | EMA length for SPY regime check |
| EMA Régimen QQQ | 20 | EMA length for QQQ regime check |
| Activar doble TF | OFF | Enables the higher-timeframe regime confirmation |
| TF superior obligatorio | OFF | ON = mandatory gate; OFF = adds +1 to score |

### 🎯 C2 Improved Pullback
| Input | Default | Description |
|-------|---------|-------------|
| C2 Barras tendencia previa | 3 | Number of prior bars that must have closed above the fast EMA before the current pullback qualifies. Range: 1–10. The table VALUE column shows `X/N barras OK` live. |

> **Why does this number matter?** At 1 bar the filter is minimal — catches dips in sideways ranges. At 5–10 bars the filter is strict — only signals with a clearly established prior uptrend. The default of 3 bars balances sensitivity and signal quality.

### 📈 Asset Trend Filter C6
| Input | Default | Description |
|-------|---------|-------------|
| SMA Media | 50 | Mid-term SMA length |
| SMA Larga | 200 | Primary SMA length |
| C6 obligatoria | OFF | ON = no signal without bullish structure; OFF = scores 0 or 1 |

### ⚠️ Risk Management
| Input | Default | Description |
|-------|---------|-------------|
| ATR Longitud | 14 | ATR length for stop loss calculation |
| ATR Stop-Loss × | 1.5 | ATR multiplier. SL = Low − (ATR × multiplier) |

---

## On-Chart Table — Columns (v4)

| Column | Content |
|--------|---------|
| CONDITION | Code and name (C1, C1b, C2+, C3–C6) |
| DESCRIPTION | Summarised logic + active configuration |
| VALUE | Live numeric reading |
| STATUS | PASS / FAIL badge |

**C2+ row:** the VALUE field shows `X/N barras OK` — the number of prior bars that closed above the fast EMA vs. those required. If the trend context fails, the text renders in red.

**C1b row:** always visible but visually dimmed when disabled. When active, shows `SPY OK/X  QQQ OK/X` for the higher TF and the active mode (OBLIGATORIA / PUNTUADA +1).

**Config summary row (row 11):** compact one-line summary of key active inputs, e.g. `C2:3b  C6:punt  TFsup:OFF`.

---

## How to Use

1. Open TradingView → Pine Editor → paste the content of `indicators/ovtlier_plan_a.pine`
2. Add to chart on a **Daily or Weekly** timeframe
3. Tune `C2 Barras tendencia previa` to match your conviction level:
   - **3 bars** (default): balanced sensitivity and quality
   - **5+ bars**: stricter — only well-established prior uptrends
4. Enable `Doble TF` to confirm the regime is also bullish on Weekly/Monthly
5. Read the on-chart table (position configurable from inputs)
6. Only trade READY signals — never force a trade when C1 fails

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
