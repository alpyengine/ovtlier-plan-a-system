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

- Script: `plan_a_backtest_v2.pine` v2.2, input `useRegimeSizingInput`.
  - `false` → Variante (A), sizing fijo, sin cambios de comportamiento.
  - `true`  → Variante (B), sizing modulado por régimen.
- Universo: VGT, SMH, MSFT (universo confirmado de Plan A Config 2).
- Resto de inputs de Plan A sin tocar (misma Config 2 en A y B — una
  sola variable distinta entre variantes: `useRegimeSizingInput`).
- Costes reales de la cabecera: comisión 0,05% + slippage 0,05% (IB).
- Verificación con CSV completo (List of Trades), no con el dashboard
  en pantalla — lección de Plan A §28/§29.

## Criterio de éxito

La variante (B) debe mejorar el resultado ajustado a riesgo (menor
drawdown con retorno similar, o mejor Sharpe/Sortino aproximado) sobre
la variante (A), de forma consistente en los 3 tickers — no basta con
que gane en 1 de 3.

## Resultados por ticker

### VGT

| Métrica | (A) Fijo | (B) Modulado | Δ |
|---|---|---|---|
| Net Profit % | ⬜ | ⬜ | ⬜ |
| Max Drawdown % | ⬜ | ⬜ | ⬜ |
| Profit Factor | ⬜ | ⬜ | ⬜ |
| Win Rate % | ⬜ | ⬜ | ⬜ |
| Nº operaciones | ⬜ | ⬜ | (debe coincidir en A y B) |
| Sharpe/Sortino aprox. | ⬜ | ⬜ | ⬜ |

### SMH

| Métrica | (A) Fijo | (B) Modulado | Δ |
|---|---|---|---|
| Net Profit % | ⬜ | ⬜ | ⬜ |
| Max Drawdown % | ⬜ | ⬜ | ⬜ |
| Profit Factor | ⬜ | ⬜ | ⬜ |
| Win Rate % | ⬜ | ⬜ | ⬜ |
| Nº operaciones | ⬜ | ⬜ | (debe coincidir en A y B) |
| Sharpe/Sortino aprox. | ⬜ | ⬜ | ⬜ |

### MSFT

| Métrica | (A) Fijo | (B) Modulado | Δ |
|---|---|---|---|
| Net Profit % | ⬜ | ⬜ | ⬜ |
| Max Drawdown % | ⬜ | ⬜ | ⬜ |
| Profit Factor | ⬜ | ⬜ | ⬜ |
| Win Rate % | ⬜ | ⬜ | ⬜ |
| Nº operaciones | ⬜ | ⬜ | (debe coincidir en A y B) |
| Sharpe/Sortino aprox. | ⬜ | ⬜ | ⬜ |

## Auditoría de concentración

⬜ Pendiente — revisar si la mejora (si la hay) depende de 1-2 operaciones
puntuales en régimen ALTA, o si es un efecto distribuido en toda la
muestra.

## Veredicto

⬜ Pendiente de datos.

- Si mejora en los 3 tickers de forma consistente: **CONFIRMADA** — el
  indicador pasa de herramienta de lectura de contexto a sizing operativo.
- Si no: **REFUTADA** (misma categoría que Plan M o Liquidity Sweeps
  Long-Only) — el indicador queda como herramienta de lectura de
  contexto, no como sizing operativo.
