# RESULTADOS — ¿El régimen GARCH mejora el sizing frente a fijo?

## Encargo

Ver encargo original (LA GRANJA, campaña `garch-volatility-sizing`).
Pregunta: usando Plan A Config 2 como sistema de entrada ya validado,
¿modular el tamaño de posición por el régimen GARCH (100% equity en BAJA,
50% en ALTA) mejora el resultado ajustado a riesgo frente al sizing fijo
actual (100% equity siempre)?

## Decisiones fijadas antes de codificar

1. Régimen leído siempre en Diario (coincide con la calibración validada
   de `garch_volatility_regime`, TV-001).
2. Lectura con el cierre de la vela ANTERIOR a la señal de entrada de
   Plan A (patrón `[1]` + `lookahead_on`, igual que C1).
3. Una sola regla de modulación: reducir a la mitad el tamaño en ALTA,
   mantener el sizing fijo en BAJA. "Evitar entradas en ALTA" queda
   fuera de esta ronda (campaña posterior si esta da resultado).

## Método

- Script: `ovtlier_plan_a_backtest.pine` v3.0, input `useRegimeSizingInput`.
  - `false` → Variante (A), sizing fijo, sin cambios de comportamiento.
  - `true`  → Variante (B), sizing modulado por régimen GARCH.
- Configuración: Config 2 — C4 ON, minScore 4, Modo B Trailing ATR 2×,
  MaxBars 10. Todos los inputs idénticos entre A y B — una sola variable
  distinta: `useRegimeSizingInput`.
- Universo: VGT, SMH, MSFT.
- Costes: comisión 0,05% + slippage 1 tick (IB).
- Exportados con historial máximo — protocolo §28/§29.
- Fecha exportación: 01 sep 2026.

## Criterio de éxito

La variante (B) debe mejorar el resultado ajustado a riesgo (menor
drawdown con retorno similar, o PF superior) de forma consistente en
los 3 tickers. No basta con que gane en 1 de 3.

## Nota sobre número de trades

En la Variante B el número de trades PUEDE diferir de la Variante A
porque el sizing modulado cambia el capital disponible para cada
operación, lo que puede alterar el orden de ejecución en TradingView
cuando el capital restante es limitado. Esto es un comportamiento
esperado del motor de backtesting, no un error.

## Resultados por ticker

### VGT

| Métrica | (A) Fijo | (B) Modulado GARCH | Δ |
|---|---|---|---|
| Net Profit % | +7,67% | +8,43% | +0,76pp |
| Max Drawdown % (adversa máx.) | -2,12% | -2,12% | 0 |
| Profit Factor | 4,70 | 4,19 | -0,51 |
| Win Rate % | 57,1% | 60,0% | +2,9pp |
| Nº operaciones | 7 | 10 | +3 (ver nota) |
| Conc. top1 | 53,1% ⚠️ | 49,2% ✅ | mejora |

### SMH

| Métrica | (A) Fijo | (B) Modulado GARCH | Δ |
|---|---|---|---|
| Net Profit % | -1,63% | +0,56% | +2,19pp |
| Max Drawdown % (adversa máx.) | -3,29% | **-7,17%** | **-3,88pp** ⚠️ |
| Profit Factor | 0,68 | 0,92 | +0,24 |
| Win Rate % | 50,0% | 55,6% | +5,6pp |
| Nº operaciones | 4 | 9 | +5 (ver nota) |
| Conc. top1 | 95,2% ⚠️ | 48,4% ✅ | mejora concentración |

### MSFT

| Métrica | (A) Fijo | (B) Modulado GARCH | Δ |
|---|---|---|---|
| Net Profit % | +0,21% | -3,26% | **-3,47pp** ⚠️ |
| Max Drawdown % (adversa máx.) | -4,72% | **-10,71%** | **-5,99pp** ⚠️ |
| Profit Factor | 1,01 | 0,64 | -0,37 |
| Win Rate % | 33,3% | 33,3% | 0 |
| Nº operaciones | 6 | 6 | 0 |
| Conc. top1 | 82,6% ⚠️ | 90,4% ⚠️ | empeora |

## Auditoría de concentración

**VGT Variante A:** top1 53,1% — Trade 5 (2021-01-21, MaxBars) con
+43.187 USD. Justo sobre el umbral 50% — resultado frágil.

**SMH Variante A:** top1 95,2% — 1 trade explica casi todo el beneficio
bruto. Muestra de 4 trades estadísticamente inútil.

**MSFT Variante A:** top1 82,6% — Trade 3 (2018-09-21, MaxBars,
+51.649 USD) explica el 82,6% del bruto. Sin ese trade el sistema
pierde dinero.

**Nota sobre la Variante B:** el aumento de trades en VGT (+3) y SMH
(+5) en la Variante B respecto a A es un artefacto del motor de
backtesting — al reducir el tamaño de posición en régimen ALTA, el
capital restante permite entradas que en Variante A eran bloqueadas
por insuficiencia de margen. Esto hace que las Variantes A y B no sean
estrictamente comparables en estos dos tickers. Para una comparación
limpia habría que usar `default_qty_type = strategy.fixed` con unidades
fijas, no porcentaje de equity.

## Veredicto

**REFUTADA — el sizing GARCH no mejora el resultado de forma consistente.**

| Ticker | A mejor | B mejor | Criterio éxito |
|---|---|---|---|
| VGT | PF (+0,51) | Net (+0,76pp), DD igual | Empate — no concluyente |
| SMH | DD (-3,88pp peor en B) | Net (+2,19pp) | B empeora riesgo |
| MSFT | Todo — PF, Net, DD | — | A claramente mejor |

El criterio de éxito exigía mejora consistente en los 3 tickers. MSFT
falla claramente: la Variante B pierde más dinero (-3,26% vs +0,21%)
con el doble de drawdown (-10,71% vs -4,72%). En SMH el drawdown
empeora significativamente en B pese a la mejora en net profit.

**Causa probable:** con Modo B (Trailing ATR), reducir el tamaño al
50% en régimen ALTA no reduce suficientemente el riesgo absoluto —
el trailing stop amplio (2×ATR) sigue exponiendo capital suficiente
para generar pérdidas grandes. La modulación GARCH tiene más sentido
teórico con Modo C o D (salida más reactiva) que con trailing amplio.

**El indicador `garch_volatility_regime` queda como herramienta de
lectura de contexto, no como sizing operativo en esta configuración.**

## Próxima hipótesis si se quiere continuar

Antes de descartar definitivamente el sizing GARCH, habría que probar
con **Config 5 (C4 OFF, Modo C Cruce EMA)** — la configuración ganadora
de la campaña §30 — porque:
1. Modo C tiene salidas más reactivas que el Trailing ATR, lo que
   amplifica el beneficio de reducir el tamaño en momentos de alta
   volatilidad.
2. Config 5 tiene más trades por ticker (10-18 vs 4-7), lo que da
   muestra estadística más robusta.
3. La comparación sería con la config que ya sabemos que funciona —
   no con Config 2 que falla por sí sola en SMH y MSFT.

Esto es un encargo separado — no parte de esta campaña.

