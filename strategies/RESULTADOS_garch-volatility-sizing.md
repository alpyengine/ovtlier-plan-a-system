# RESULTADOS — ¿El régimen GARCH mejora el sizing frente a fijo?

## Encargo

Ver encargo original (LA GRANJA, campaña `garch-volatility-sizing`).
Pregunta: usando Plan A Config 5 como sistema de entrada, ¿modular el
tamaño de posición por el régimen GARCH (100% equity en BAJA, 50% en
ALTA) mejora el resultado frente al sizing fijo actual (100% siempre)?

## Decisiones fijadas antes de codificar

1. Régimen leído siempre en Diario — coincide con calibración validada
   de `garch_volatility_regime` (TV-001).
2. Lectura con el cierre de la vela ANTERIOR a la señal de entrada
   (patrón `[1]` + `lookahead_on`, mismo que C1).
3. Una sola regla de modulación: 50% en ALTA, 100% en BAJA.

## Configuración exacta de la campaña

| Input | Valor |
|---|---|
| Modo salida | C — Cruce EMA |
| C4 RVOL obligatoria | OFF (Config 5) |
| Score mínimo entrada | 4 |
| useRegimeSizingInput | ON (Variante B) / OFF (Variante A) |
| ATR Stop-Loss | 1.5× |
| MaxBars | 10 |

## Universo

VGT, SMH, MSFT — Daily (1D). Historial máximo. Costes: 0,05% + 1 tick.

## Resultados — Variante A (GARCH OFF) vs Variante B (GARCH ON)

| Ticker | Métrica | A (OFF) | B (ON) | Delta |
|---|---|---|---|---|
| VGT | Trades | 10 | 10 | 0 |
| VGT | WR% | 50,0% | 50,0% | 0 |
| VGT | PF | 2,83 | 2,83 | 0 |
| VGT | Net% | +4,49% | +4,49% | 0 |
| VGT | DD% | -1,82% | -1,82% | 0 |
| SMH | Trades | 8 | 8 | 0 |
| SMH | WR% | 37,5% | 37,5% | 0 |
| SMH | PF | 1,07 | 1,07 | 0 |
| SMH | Net% | +0,25% | +0,25% | 0 |
| SMH | DD% | -2,81% | -2,81% | 0 |
| MSFT | Trades | 12 | 12 | 0 |
| MSFT | WR% | 33,3% | 33,3% | 0 |
| MSFT | PF | 0,38 | 0,38 | 0 |
| MSFT | Net% | -6,13% | -6,13% | 0 |
| MSFT | DD% | -3,70% | -3,70% | 0 |

**Los CSV de Variante A y Variante B son idénticos en los 3 tickers.**
Delta = 0 en todas las métricas.

## Diagnóstico

La modulación GARCH nunca se activó. Verificado en Bar Replay: en todas
las fechas de entrada el régimen mostraba "BAJA x1.0" — el multiplicador
fue siempre 1.0 y el sizing no cambió en ningún trade.

**Causa estructural:** el Plan A entra durante pullbacks en tendencias
alcistas, que son períodos de baja volatilidad por definición. El régimen
GARCH detecta alta volatilidad. Las dos condiciones son incompatibles —
nunca coinciden en la misma barra.

El Plan A ya actúa como filtro de volatilidad implícito por la naturaleza
de sus condiciones de entrada (C2 pullback ordenado, C3 RSI sobreventa
moderada, C8 cerca de máximos — todo esto excluye períodos de alta
volatilidad de forma natural).

## Veredicto

**HIPÓTESIS NO TESTEABLE con esta combinación de sistema.**

No es un fallo de código ni de configuración. Es un hallazgo metodológico:
el sizing GARCH modula en régimen ALTA, pero el Plan A con Config 5 nunca
genera señales en régimen ALTA. La modulación nunca tiene efecto.

El indicador `garch_volatility_regime` queda como herramienta de lectura
de contexto de mercado — no como modulador de sizing para el Plan A.

## Nota sobre campaña anterior (Config 2 + Modo B)

Se realizó una primera ronda con Config 2 (C4 ON, Modo B Trailing ATR).
En esa ronda los CSV sí difirieron entre Variante A y B porque el sizing
en porcentaje de equity alteró el capital disponible para entradas
posteriores — no porque el régimen GARCH se activara realmente. Ese
efecto era un artefacto del motor de backtesting, no una señal real de
la modulación GARCH.

## Próxima hipótesis si se quiere continuar

Para que el sizing GARCH tenga efecto real habría que probarlo con un
sistema que genere entradas también en períodos de alta volatilidad —
por ejemplo el Plan M (momentum intradiario), donde las señales no
requieren pullback ordenado y pueden ocurrir en cualquier régimen.
Eso es un encargo separado.

---

*Campaña cerrada — 01 sep 2026. No es asesoramiento financiero.*
