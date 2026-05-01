# Ovtlier Plan A — Swing Indicator System

> Pine Script v6 indicators for TradingView implementing Christopher Uhl's **Plan A** swing trading methodology.

📖 [Versión en español](./README_ES.md)

---

## What is Plan A?

Plan A is the swing trading framework taught at **Ovtlier University** by Christopher Uhl. It is designed for **Daily and Weekly timeframes** and focuses on buying high-quality pullbacks within confirmed bull-market regimes.

---

## Indicators in this Repository

| File | Description | Status |
|------|-------------|--------|
| `indicators/ovtlier_plan_a.pine` | Main Plan A swing signal indicator | ✅ Active (v5) |

---

## Plan A — Core Conditions

The indicator evaluates **7 conditions** plus an optional higher-timeframe regime check.

| # | Condition | Type | Logic |
|---|-----------|------|-------|
| C1 | Broad Market Regime | **Mandatory** | SPY + QQQ both above EMA(20) |
| C1b | Higher-TF Regime | Optional (scored or mandatory) | SPY + QQQ in regime on Weekly (Daily) or Monthly (Weekly) |
| C2+ | Pullback with Trend Context | Scored | N prior bars closed above EMA fast + Low touches EMA |
| C3 | Relative Weakness (RSI) | Scored | RSI(14) < 40 **or** RSI(2) < 10 |
| C4 | Relative Volume (RVOL) | Scored | Volume > 1.5× average |
| C5 | Reversal Candle (Hammer) | Scored | Lower wick ≥ 2× body |
| C6 | Asset Trend Filter | Scored / Optional | Close > SMA(50), Close > SMA(200), SMA(50) > SMA(200) |
| C7 | Bullish RSI Divergence | Scored / Optional | Price: lower low (LL) + RSI: higher low (HL) |

**Dynamic scoreMax:**
```
scoreMax = 5 (base C2-C6)
         + 1 if higher TF active in scored mode
         + 1 if C7 active in scored mode
         = maximum 7
```

**Signal levels:**
- 🟢 **READY** — C1 pass + score >= scoreMax−1
- 🟡 **WATCH** — C1 pass + score == scoreMax−2
- 🔴 **NOT READY** — C1 fail or score below threshold

---

## Configurable Inputs

### 📊 Broad Market Regime C1
| Input | Default | Description |
|-------|---------|-------------|
| EMA Régimen SPY | 20 | EMA length for SPY regime check |
| EMA Régimen QQQ | 20 | EMA length for QQQ regime check |
| Activar doble TF | OFF | Enables higher-timeframe regime confirmation |
| TF superior obligatorio | OFF | ON = mandatory gate; OFF = +1 to score |

### 🎯 C2 Improved Pullback
| Input | Default | Description |
|-------|---------|-------------|
| C2 Barras tendencia previa | 3 | Prior bars that must close above fast EMA before dip qualifies. Range: 1–10. Table shows `X/N barras OK` live. |

### 📉 RSI Divergence — C7
| Input | Default | Description |
|-------|---------|-------------|
| Activar C7 Divergencia RSI | ON | Activates the bullish RSI divergence condition |
| C7 Ventana barras (lookback) | 5 | Bars back to find the prior price and RSI low. Larger = longer divergences. Range: 2–20. |
| C7 obligatoria | OFF | ON = no signal without divergence; OFF = +1 to score |
| Mostrar línea divergencia | ON | Draws a dashed lime line connecting the two price lows when divergence is detected |

> **What is bullish RSI divergence?** When price makes a lower low vs. N bars ago but RSI makes a higher low — bearish momentum is exhausting even as price continues falling. One of the most reliable reversal signals in the Uhl methodology, especially when combined with C2 (pullback to support) and C3 (RSI oversold).

> **How to read the C7 VALUE field:** The table shows `RSIdiff +X.X  LB:Nb`. A positive `RSIdiff` confirms the current RSI is above the window's RSI minimum — a real divergence. A negative value means no divergence even if price dropped.

### 📈 Asset Trend Filter C6
| Input | Default | Description |
|-------|---------|-------------|
| SMA Media | 50 | Mid-term SMA length |
| SMA Larga | 200 | Primary SMA length |
| C6 obligatoria | OFF | ON = no signal without bullish structure |

### ⚠️ Risk Management
| Input | Default | Description |
|-------|---------|-------------|
| ATR Longitud | 14 | ATR length |
| ATR Stop-Loss × | 1.5 | SL = Low − (ATR × multiplier) |

---

## On-Chart Table — Structure (v5)

| Row | Content |
|-----|---------|
| 0 | Header (CONDITION / VALUE / STATUS) |
| 1 | Ticker + TF + SPY/QQQ status |
| 2 | C1 Regime — mandatory |
| 3 | C1b Higher TF — optional |
| 4 | C2+ Pullback + trend context |
| 5 | C3 RSI |
| 6 | C4 RVOL |
| 7 | C5 Hammer |
| 8 | C6 Trend SMA |
| 9 | **C7 RSI Divergence** ← new in v5 |
| 10 | Score + SL price and % |
| 11 | Final result (READY / WATCH / NOT READY) |
| 12 | Active config summary |
| 13 | Version + date |

---

## How to Use

1. Open TradingView → Pine Editor → paste `indicators/ovtlier_plan_a.pine`
2. Add to chart on **Daily or Weekly** timeframe
3. Tune inputs to your style:
   - **Conservative:** C6 mandatory + C7 mandatory + TF sup mandatory
   - **Balanced (default):** everything in scored mode
   - **Sensitive:** reduce C2 to 1–2 bars, disable C7
4. Read the table — pay special attention to C7 when C3 also passes (double RSI confirmation)
5. The dashed lime line on the chart marks the detected divergence visually
6. Only trade READY signals

---

## Risk Management

```
SL = Bar Low − (ATR(14) × multiplier)
```
Default multiplier: **1.5×**. Table shows absolute level and percentage distance.

> ⚠️ Educational tool only. Not financial advice.

---

## Repository Structure

```
ovtlier-plan-a-system/
├── indicators/
│   └── ovtlier_plan_a.pine          ← v5 (active)
├── screeners/                        ← scanner v1 (next)
├── strategies/                       ← backtest v1 (next)
├── README.md
├── README_ES.md
├── CHANGELOG.md
└── LICENSE
```

> The `scanner` and `backtest` files are independent projects with their own version cycles (`scanner/v1`, `backtest/v1`), separate from the main indicator.

---

## Plan A vs Plan M

| | Plan A | Plan M |
|---|---|---|
| Timeframe | Daily / Weekly | Intraday (15m / 1H) |
| Hold period | 3–10 days | Intraday / 1–2 days |
| Core logic | Buy the dip in bull regime | Momentum anomaly detection |
| Risk profile | Swing, defined risk | Intraday, faster stops |

---

## Author

Built on the teachings of **Christopher Uhl — Ovtlier University**.
Coded by [alpyengine](https://github.com/alpyengine).

## License

[Mozilla Public License 2.0](./LICENSE) · © Ovtlier Plan A System contributors.
