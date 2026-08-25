# RESULTADOS CAMPAÑA — Ovtlier Plan A Backtest v1
### Entregable de campaña — en curso
*Última actualización: 21 ago 2026 · Config base: Modo C Cruce EMA · C2=3 barras · C7 OFF · scoreMax=5*
*Timeframes probados: Daily (1D) · Weekly (1W) · Capital: 1M USD · Comisión: 0,05% · Slippage: 1 tick (~0,05%)*

> **Documentos relacionados:** `ovtlier_plan_a_backtest.pine` (script), `ovtlier_plan_a.pine` (indicador v5.2), `plan_a_construccion_indicador.md` (racional de diseño — de dónde salen C1-C7), `GUIA_outlier_referencia.md` (filosofía/disciplina del método base), `RESULTADOS_SAR_trailing.md` (precedente de protocolo), `RESULTADOS_H1_ATR_trailing.md` (precedente de umbral de éxito).

---

## §1 · OBJETO DE LA CAMPAÑA

| Campo | Valor |
|---|---|
| Sistema a probar | Ovtlier Plan A — 7 condiciones (C1-C7), señales READY únicamente |
| Script | `ovtlier_plan_a_backtest.pine` v1 |
| Pregunta principal | ¿El sistema Plan A genera alfa sobre Buy & Hold, en qué activos y con qué configuración de salida? |
| Universo probado | 10 tickers: SPY, VGT, VWO, GLDM, SMH, IGV, ROP, VDE, AMZN, ILMN |
| Configuraciones probadas | 4 (ver §2) |
| Umbral de éxito | Profit Factor > 1,2 — mismo umbral que campañas ATR y SAR |
| Precedente | Campaña SAR (33 casos): ninguno superó 1,2x. Campaña ATR (28 casos): ninguno superó 1,2x |

---

## §2 · CONFIGURACIONES PROBADAS

| Config | Modo salida | C7 Div RSI | C2 Barras | scoreMax | Estado |
|---|---|---|---|---|---|
| **Config 1** | B — Trailing ATR (mult 2,0×) | OFF | 3 | 5 | Probada en SPY únicamente |
| **Config 2** | C — Cruce EMA (EMA rápida 9) | OFF | 3 | 5 | **Configuración principal** — 10 tickers |
| **Config 3** | C — Cruce EMA | **ON** | 3 | 6 | Descartada — sin trades suficientes |
| **Config 4** | C — Cruce EMA | OFF | **1** | 5 | Probada — 10 tickers |

---

## §3 · TABLA DE REGISTRO — CONFIG 1 (Trailing ATR, SPY únicamente)

*Referencia inicial antes de ampliar el universo.*

| Ticker | TF | PF | %Gan | Ops | DD% | Net% | vs criterio |
|---|---|---|---|---|---|---|---|
| SPY | 1D | 2,30 | 50,0% | 16 | 7,33% | +13,29% | ✅ PASS |

---

## §4 · TABLA DE REGISTRO — CONFIG 2 (Cruce EMA, C2=3, C7 OFF)

*Configuración principal. 10 tickers, Daily.*

| Ticker | Tipo | PF | %Gan | Ops | DD% | Net% | vs criterio | Notas |
|---|---|---|---|---|---|---|---|---|
| **SPY** | ETF broad | **3,77** | 58,3% | 12 | 4,60% | +16,51% | ✅ **PASS** | Mejor resultado ETF |
| **VGT** | ETF tech | **3,53** | 57,1% | 14 | 2,02% | +7,46% | ✅ **PASS** | Menor DD del conjunto |
| VWO | ETF emergentes | 1,18 | 60,0% | 5 | 4,02% | +0,62% | ❌ FAIL | PF < 1,2; muestra pequeña |
| GLDM | ETF oro | 0,07 | 50,0% | 2 | 2,61% | -2,18% | ❌ FAIL | Solo 2 trades — oro no apto |
| **SMH** | ETF semis | **1,33** | 50,0% | 8 | 4,83% | +1,72% | ✅ **PASS** | Justo por encima del umbral |
| **IGV** | ETF software | **1,65** | 50,0% | 18 | 6,05% | +7,91% | ✅ **PASS** | Mayor volumen de trades tech |
| ROP | Acción | 0,10 | 14,3% | 7 | 13,01% | -12,96% | ❌ FAIL | Win rate 14% — inadecuado |
| VDE | ETF energía | 0,11 | 40,0% | 5 | 10,27% | -8,87% | ❌ FAIL | Energía no apta para Plan A |
| **AMZN** | Acción tech | **7,26** | 83,3% | 6 | 2,42% | +6,13% | ✅ **PASS** | PF más alto del conjunto |
| ILMN | Acción biotech | 0,81 | 33,3% | 3 | 2,50% | -0,44% | ❌ FAIL | Biotech no apta |

**Resumen Config 2:** 5 PASS / 5 FAIL · Patrón claro: tech y broad market superan el umbral; energía, commodities, biotech y emergentes fallan.

---

## §5 · TABLA DE REGISTRO — CONFIG 3 (Cruce EMA, C2=3, C7 ON)

*C7 activo eleva scoreMax a 6 y requiere divergencia alcista RSI simultánea con las demás condiciones.*

| Ticker | Ops | PF | Net% | vs criterio | Notas |
|---|---|---|---|---|---|
| SPY | 0 | — | 0% | ❌ Sin trades | |
| VGT | 0 | — | 0% | ❌ Sin trades | |
| VWO | 1 | 0 | -2,42% | ❌ 1 trade perdedor | Muestra inútil |
| GLDM | 0 | — | 0% | ❌ Sin trades | |
| SMH | 1 | — | +2,76% | ⚠️ 1 trade | Muestra inútil |
| IGV | 1 | — | +0,19% | ⚠️ 1 trade | Muestra inútil |
| ROP | 0 | — | 0% | ❌ Sin trades | |
| VDE | 0 | — | 0% | ❌ Sin trades | |
| AMZN | 1 | — | +0,98% | ⚠️ 1 trade | Muestra inútil |
| ILMN | 0 | — | 0% | ❌ Sin trades | |

**Config 3 descartada.** C7 en Daily elimina prácticamente todas las señales — la divergencia alcista RSI no coincide con los pullbacks con suficiente frecuencia en este timeframe. C7 ON no es viable para backtesting en Daily con este universo.

---

## §6 · TABLA DE REGISTRO — CONFIG 4 (Cruce EMA, C2=1, C7 OFF)

*C2 reducida a 1 barra — sin requisito de tendencia previa.*

| Ticker | PF | %Gan | Ops | DD% | Net% | vs criterio | Config 2 PF | Δ PF |
|---|---|---|---|---|---|---|---|---|
| SPY | 1,01 | 50,0% | 14 | 4,53% | +0,06% | ❌ FAIL | 3,77 | -2,76 |
| VWO | 0,80 | 42,9% | 7 | 5,64% | -1,00% | ❌ FAIL | 1,18 | -0,38 |
| VGT | 3,53 | 57,1% | 14 | 2,02% | +7,46% | ✅ PASS | 3,53 | = |
| GLDM | 0,08 | 66,7% | 3 | 2,61% | -2,16% | ❌ FAIL | 0,07 | +0,01 |
| IGV | 1,67 | 52,4% | 21 | 6,05% | +8,45% | ✅ PASS | 1,65 | +0,02 |
| ROP | 0,09 | 12,5% | 8 | 14,45% | -14,40% | ❌ FAIL | 0,10 | -0,01 |
| VDE | 0,12 | 50,0% | 6 | 10,20% | -8,80% | ❌ FAIL | 0,11 | +0,01 |
| AMZN | 7,49 | 85,7% | 7 | 2,42% | +6,36% | ✅ PASS | 7,26 | +0,23 |
| ILMN | 0,81 | 33,3% | 3 | 2,50% | -0,44% | ❌ FAIL | 0,81 | = |
| SMH | n/d | n/d | n/d | n/d | n/d | — | 1,33 | — |

**Resumen Config 4:** 3 PASS / 6 FAIL (SMH no disponible) · Reducir C2 a 1 barra empeora el resultado en los activos clave (SPY: PF 3,77 → 1,01) sin mejorar los que ya fallaban. El requisito de tendencia previa de 3 barras es un filtro de calidad real, no ruido.

---

## §7 · COMPARATIVA CONSOLIDADA — CONFIG 2 VS CONFIG 4

| Ticker | C2 PF | C4 PF | Ganador | Δ |
|---|---|---|---|---|
| SPY | **3,77** | 1,01 | **Config 2** | -2,76 |
| VGT | **3,53** | 3,53 | Empate | = |
| VWO | 1,18 | 0,80 | **Config 2** | -0,38 |
| GLDM | 0,07 | 0,08 | Empate (ambos fallan) | +0,01 |
| SMH | **1,33** | n/d | Config 2 por defecto | — |
| IGV | 1,65 | **1,67** | Config 4 (+0,02) | +0,02 |
| ROP | 0,10 | 0,09 | Empate (ambos fallan) | -0,01 |
| VDE | 0,11 | 0,12 | Empate (ambos fallan) | +0,01 |
| AMZN | 7,26 | **7,49** | Config 4 (+0,23) | +0,23 |
| ILMN | 0,81 | 0,81 | Empate | = |

**Config 2 (C2=3) gana o empata en 8 de 9 tickers comparables.** Config 4 mejora marginalmente IGV (+0,02) y AMZN (+0,23) pero destruye SPY (-2,76). Config 2 es la configuración ganadora.

---

## §8 · CASOS ANÓMALOS Y VERIFICACIONES

### AMZN — PF 7,26-7,49 con solo 6-7 trades
Resultado llamativo. Con tan pocos trades el PF es matemáticamente inestable — un trade distinto puede multiplicarlo o destruirlo. No se verifica como ventaja sistemática hasta tener más muestra (mínimo 20-30 trades). **Tratar con cautela.**

### Config 3 — C7 elimina señales por diseño
No es un bug del script — es comportamiento esperado. La divergencia RSI en Daily es un evento poco frecuente que no coincide con los pullbacks a la EMA con suficiente regularidad. C7 es un filtro de calidad útil para el indicador visual (donde el usuario evalúa trade a trade) pero excesivamente restrictivo para un sistema automático de backtesting en Daily.

### ROP y VDE — PF < 0,2 en ambas configs
Estos activos no son inadecuados por el diseño del backtest — son inadecuados para la metodología Plan A en Daily. ROP (industrial compounding) y VDE (energía cíclica) no siguen los patrones de pullback institucional + volumen que el Plan A detecta en tech y broad market.

---

## §9 · VEREDICTO POR HIPÓTESIS

### H1 — ¿El Plan A genera alfa sobre Buy & Hold? — CONFIRMADA CON CONDICIONES

El sistema supera el umbral PF > 1,2 en **5 de 10 tickers** con Config 2. El alfa es real y consistente en el universo tecnológico y de broad market. No es universal.

### H2 — ¿C7 (Divergencia RSI) mejora los resultados? — REFUTADA en Daily

C7 ON elimina prácticamente todas las señales en Daily. No es viable como filtro automático en este timeframe. Puede ser útil como filtro de convicción manual (indicador visual) pero no para estrategia sistemática.

### H3 — ¿Reducir C2 a 1 barra genera más señales y mejores resultados? — REFUTADA

Más señales pero peores. El filtro de tendencia previa de 3 barras es un componente de calidad real del sistema. Reducirlo degrada el PF en los activos donde el sistema ya funcionaba.

### H4 — ¿El Modo C (Cruce EMA) supera al Modo B (Trailing ATR)? — CONFIRMADA en SPY

SPY: Modo B PF 2,30 vs Modo C PF 3,77. El cruce de EMA actúa como salida natural del Plan A — usa la misma zona de soporte que generó la entrada como señal de salida.

---

## §10 · UNIVERSO APTO PARA PLAN A DAILY (Config 2 confirmada)

| Ticker | PF | Win% | Ops | DD% | Calificación |
|---|---|---|---|---|---|
| AMZN | 7,26 | 83,3% | 6 | 2,42% | ⭐ Mejor resultado — verificar muestra |
| SPY | 3,77 | 58,3% | 12 | 4,60% | ✅ Resultado robusto |
| VGT | 3,53 | 57,1% | 14 | 2,02% | ✅ Resultado robusto — menor DD |
| IGV | 1,65 | 50,0% | 18 | 6,05% | ✅ Mayor muestra tech |
| SMH | 1,33 | 50,0% | 8 | 4,83% | ✅ Justo en umbral — ampliar muestra |

---

## §11 · UNIVERSO NO APTO PARA PLAN A DAILY

| Ticker | Tipo | PF | Motivo de descarte |
|---|---|---|---|
| ROP | Industrial | 0,10 | Win rate 14% — señales falsas sistemáticas |
| VDE | Energía | 0,11 | Sector cíclico incompatible con señales Plan A |
| GLDM | Oro | 0,07 | Commodity — sin patrón de pullback institucional |
| ILMN | Biotech | 0,81 | Alta volatilidad asimétrica — stops prematuros |
| VWO | Emergentes | 1,18 | PF marginal — muestra insuficiente para confirmar |

---

## §12 · RECOMENDACIÓN OPERATIVA

- **Configuración recomendada:** Config 2 — Modo C Cruce EMA, C2=3 barras, C7 OFF, scoreMax=5.
- **Universo operativo:** SPY, VGT, IGV en Daily como núcleo robusto. AMZN con cautela por muestra pequeña.
- **No operar:** ROP, VDE, GLDM, ILMN con Plan A Daily — el sistema no genera alfa en estos activos.
- **VWO:** resultado marginal (PF 1,18) — no operar hasta tener más muestra histórica.
- **C7:** útil como filtro de convicción manual en el indicador visual. No activar en backtest automático Daily.
- **Max barras = 10:** mantener como protección anti-zombi. Coincide con holding period 3-10 días del Plan A.

---

## §13 · QUÉ NO SE PROBÓ

- **Weekly timeframe** en el universo completo — Plan A está diseñado también para Weekly. Queda pendiente como próxima campaña prioritaria.
- **NVDA, MU, MSFT, AAPL, QQQ, VOO** — tickers del plan original no ejecutados por priorizar el análisis de configuraciones. Pendiente ampliar el universo tech.
- **Config con C2=5 barras** — filtro más estricto. Podría mejorar calidad en activos marginales (VWO, SMH) a costa de menos trades.
- **Señales WATCH** (score == scoreMax-2) — excluidas por diseño en v1. Pueden ampliar la muestra sin bajar umbral de calidad. Candidatas para backtest v2.
- **Comisión 0%** — todas las simulaciones corrieron con comisión 0,05% y slippage. No se probó la diferencia en coste cero (como referencia de resultado bruto).
- **Verificación operación por operación** en AMZN (PF 7,26 con 6 trades) — pendiente revisar si la concentración en 1-2 trades explica el resultado, igual que en los casos COPPER-M y VGT-M de la campaña SAR.

---

## CIERRE DEFINITIVO DE LA CAMPAÑA — PLAN A BACKTEST v1

Con 4 configuraciones y 10 tickers probados, la conclusión es clara:

- **El Plan A genera alfa sistemático en activos tech y broad market en Daily** (SPY, VGT, IGV superan PF 1,2 con robustez). El criterio de éxito se cumple.
- **La configuración óptima es Config 2** — Modo C Cruce EMA, C2=3 barras, C7 OFF.
- **El sistema no es universal** — energía, commodities, biotech y emergentes no superan el umbral en ninguna configuración probada.
- **Próxima campaña recomendada:** Plan A Weekly en el universo tech confirmado (SPY, VGT, IGV) + ampliar a NVDA, QQQ, MSFT.

---

*Documento de proyecto — Ovtlier Plan A System. No es asesoramiento financiero.*

---

## §14 · TABLA DE REGISTRO — WEEKLY (Config 2: Cruce EMA, C2=3, C7 OFF)

*Misma configuración ganadora de Daily. Max barras ajustado a 5 semanas. 10 tickers.*
*Nota: PF vacío (—) indica que todos los trades ganaron — gross loss = 0, división por cero matemáticamente correcta.*

| Ticker | Tipo | PF | %Gan | Ops | DD% | Net% | vs criterio | Notas |
|---|---|---|---|---|---|---|---|---|
| **SPY** | ETF broad | — | 100% | 3 | 2,43% | +2,98% | ⚠️ Muestra mínima | 3 trades, todos ganadores — no concluyente |
| VWO | ETF emergentes | — | 0% | 0 | 0% | 0% | ❌ Sin trades | Emergentes sin señales en Weekly |
| **VGT** | ETF tech | — | 100% | 4 | 2,83% | +16,03% | ⚠️ Muestra mínima | 4 trades, todos ganadores — resultado llamativo |
| GLDM | ETF oro | 0 | 0% | 1 | 1,80% | -1,49% | ❌ FAIL | 1 trade perdedor |
| SMH | ETF semis | — | 100% | 1 | 2,62% | +4,33% | ⚠️ Muestra mínima | 1 trade solo |
| **IGV** | ETF software | 2,64 | 50% | 4 | 5,00% | +8,32% | ✅ **PASS** | Único PF calculable — supera umbral |
| ROP | Acción | 0,74 | 50% | 2 | 5,07% | -1,04% | ❌ FAIL | Sigue sin funcionar |
| VDE | ETF energía | 0 | 0% | 1 | 1,85% | -0,15% | ❌ FAIL | 1 trade perdedor |
| AMZN | Acción tech | — | 100% | 1 | 4,00% | +0,18% | ⚠️ Muestra mínima | 1 trade solo |
| ILMN | Acción biotech | 1,70 | 33,3% | 3 | 10,57% | +6,87% | ✅ **PASS** | Sorpresa — supera umbral en Weekly |

**Resumen Weekly Config 2:** 2 PASS calculables (IGV, ILMN) + 4 con muestra insuficiente (SPY, VGT, SMH, AMZN) + 4 FAIL/sin trades.

---

## §15 · ANÁLISIS SEMANAL — HALLAZGOS CLAVE

### Muestra insuficiente como patrón dominante
El problema central del Weekly no es que el sistema falle — es que **genera muy pocas señales**. 7 de 10 tickers tienen 0-4 trades en toda la historia disponible. Con ese volumen ningún resultado es estadísticamente significativo, ni los positivos ni los negativos.

| Situación | Tickers | Interpretación |
|---|---|---|
| 0 trades | VWO | Sistema no genera señal en este activo en Weekly |
| 1-3 trades con 100% win | SPY, SMH, AMZN | Muestra mínima — no concluyente |
| 4 trades con 100% win | VGT (+16%) | Resultado llamativo, verificar concentración |
| PF calculable y PASS | IGV (2,64), ILMN (1,70) | Únicos resultados estadísticamente utilizables |
| FAIL confirmado | GLDM, ROP, VDE | Consistente con Daily |

### VGT Weekly — caso a verificar
4 trades con 100% win rate y +16,03% net profit. Resultado llamativo siguiendo el criterio del protocolo (§8 Daily): verificar si 1-2 trades concentran el beneficio antes de considerar este resultado como válido. DD de solo 2,83% sugiere que las salidas funcionaron bien.

### ILMN — sorpresa en Weekly
En Daily: PF 0,81, FAIL. En Weekly: PF 1,70, PASS. Con solo 3 trades el dato no es robusto, pero apunta a que ILMN puede tener señales válidas en temporalidades más largas donde el ruido diario se filtra. A confirmar con más historia.

### IGV — el único resultado robusto de Weekly
4 trades con PF 2,64 y +8,32% — consistente con su resultado en Daily (PF 1,65). IGV se confirma como el activo más estable del sistema en ambos timeframes.

---

## §16 · COMPARATIVA DAILY vs WEEKLY — CONFIG 2

| Ticker | PF Daily | Trades D | PF Weekly | Trades W | Tendencia |
|---|---|---|---|---|---|
| SPY | 3,77 | 12 | — (100%) | 3 | Daily más robusto por muestra |
| VGT | 3,53 | 14 | — (100%) | 4 | Daily más robusto por muestra |
| VWO | 1,18 | 5 | — | 0 | Sin señales en Weekly |
| GLDM | 0,07 | 2 | 0 | 1 | Falla en ambos |
| SMH | 1,33 | 8 | — (100%) | 1 | Muestra insuficiente en Weekly |
| IGV | 1,65 | 18 | **2,64** | 4 | Weekly mejora PF — muestra pequeña |
| ROP | 0,10 | 7 | 0,74 | 2 | Falla en ambos |
| VDE | 0,11 | 5 | 0 | 1 | Falla en ambos |
| AMZN | 7,26 | 6 | — (100%) | 1 | Daily más robusto por muestra |
| ILMN | 0,81 | 3 | **1,70** | 3 | Weekly invierte el resultado — verificar |

**Conclusión comparativa:** Daily tiene más muestra y más resultados calculables. Weekly confirma la dirección (lo que falla en Daily falla en Weekly) pero no añade evidencia suficiente para cambiar conclusiones dado el volumen de trades.

---

## §17 · QUÉ NO SE PROBÓ (actualizado con Weekly)

- **NVDA, MU, MSFT, AAPL, QQQ, VOO** — pendiente ampliar el universo tech en ambos timeframes.
- **Config 4 (C2=1) en Weekly** — no ejecutada. Con Daily ya mostró degradación, no prioritaria.
- **Config 3 (C7 ON) en Weekly** — descartada por los resultados en Daily (elimina señales).
- **4H timeframe** — pendiente como campaña separada si se decide explorar el Plan A en temporalidades intraday extendidas.
- **Verificación operación por operación VGT Weekly** — 4 trades 100% win, pendiente confirmar si hay concentración.
- **Señales WATCH** — excluidas en v1, candidatas para backtest v2 para ampliar muestra.

---

## CIERRE DE CAMPAÑA WEEKLY — PLAN A BACKTEST v1

Weekly confirma el patrón de Daily pero con muestra insuficiente para conclusiones independientes:

- **El sistema no genera suficientes señales en Weekly** para ser estadísticamente significativo con el universo actual y el historial disponible.
- **Lo que falla en Daily falla en Weekly** (GLDM, ROP, VDE) — consistencia metodológica confirmada.
- **IGV es el activo más estable** del sistema en ambos timeframes — PF > 1,6 en Daily y > 2,6 en Weekly.
- **Daily sigue siendo el timeframe operativo recomendado** para el Plan A con este sistema, por volumen de señales y robustez estadística.
- **Próxima campaña recomendada:** ampliar el universo tech en Daily (NVDA, QQQ, MSFT, AAPL) antes de explorar 4H.

---

*Documento de proyecto — Ovtlier Plan A System. No es asesoramiento financiero.*

---

## §18 · TABLA DE REGISTRO — LOTE 2 DAILY (NVDA, QQQ, MSFT, AAPL)

*Config 2: Modo C Cruce EMA · C2=3 · C7 OFF · scoreMax=5 · Daily (1D)*

| Ticker | Tipo | PF | %Gan | Ops | DD% | Net% | vs criterio | Notas |
|---|---|---|---|---|---|---|---|---|
| NVDA | Acción semis | 0,01 | 10,0% | 10 | 60,82% | -59,77% | ❌ FAIL | Peor resultado de toda la campaña — ver §19 |
| QQQ | ETF Nasdaq | 0,65 | 50,0% | 16 | 7,69% | -3,91% | ❌ FAIL | Nasdaq como índice falla donde SPY pasa |
| **MSFT** | Acción tech | **2,86** | **75,0%** | 8 | **2,50%** | +3,71% | ✅ **PASS** | Mejor win rate de toda la campaña |
| AAPL | Acción tech | 0,45 | 50,0% | 4 | 9,97% | -5,11% | ❌ FAIL | Muestra pequeña — resultado no robusto |

**Resumen lote 2:** 1 PASS / 3 FAIL.

---

## §19 · CASO ANÓMALO — NVDA Daily (PF 0,01 · DD 60,82%)

NVDA es el resultado más destructivo de toda la campaña. Win rate del 10% sobre 10 trades indica que el sistema genera señales falsas sistemáticamente en este activo. Posibles causas:

- **Alta volatilidad intrínseca de NVDA** — los pullbacks a la EMA9 son frecuentes y profundos, activando el SL inicial antes de que el trade tenga oportunidad de desarrollarse.
- **Movimientos gap frecuentes** — NVDA tiene gaps de apertura habituales que saltan el SL calculado sobre el cierre anterior.
- **Incompatibilidad con C5 (Hammer)** — las velas de NVDA en zona de pullback raramente forman hammers limpios; cuando lo hacen en condiciones adversas el sistema entra y pierde.

**Conclusión:** NVDA queda descartada definitivamente para el Plan A Daily. El patrón de señal Plan A no es compatible con la estructura de volatilidad de semiconductores individuales de alta beta.

---

## §20 · QQQ vs SPY — MISMO RÉGIMEN, RESULTADOS OPUESTOS

SPY (PF 3,77) y QQQ (PF 0,65) usan el mismo régimen C1 (SPY+QQQ > EMA20) pero producen resultados radicalmente distintos cuando son el activo escaneado:

| | SPY | QQQ |
|---|---|---|
| PF | 3,77 | 0,65 |
| Win Rate | 58,3% | 50,0% |
| Trades | 12 | 16 |
| DD% | 4,60% | 7,69% |
| Net% | +16,51% | -3,91% |

**Hipótesis:** QQQ tiene mayor concentración en mega-cap tech de alta beta (NVDA, META, TSLA pesan más que en SPY). Los pullbacks de QQQ son más bruscos y menos ordenados que los de SPY, generando más señales falsas. El Plan A funciona mejor en el índice más diversificado y menos volátil.

---

## §21 · MSFT — RESULTADO NOTABLE

75% win rate con PF 2,86 y DD 2,5% — el mejor perfil riesgo/recompensa de todas las acciones individuales probadas. MSFT tiene características que alinean bien con el Plan A:

- Pullbacks ordenados a la EMA en tendencias establecidas
- Baja volatilidad relativa para ser una mega-cap tech
- Volumen institucional estable — C4 (RVOL) se activa en condiciones reales, no en ruido

Con solo 8 trades la muestra es limitada pero el perfil es el más prometedor de las acciones individuales. A confirmar con más historia si está disponible.

---

## §22 · TABLA DE REGISTRO — LOTE 2 WEEKLY (NVDA, QQQ, MSFT, AAPL)

*Config 2: Modo C Cruce EMA · C2=3 · C7 OFF · scoreMax=5 · Weekly (1W) · Max barras=5*

| Ticker | Tipo | PF | %Gan | Ops | DD% | Net% | vs criterio | Notas |
|---|---|---|---|---|---|---|---|---|
| NVDA | Acción semis | 0 | 0% | 1 | 2,77% | -2,77% | ❌ FAIL | Consistente con Daily — inadecuado |
| QQQ | ETF Nasdaq | 0,73 | 66,7% | 6 | 7,69% | -1,48% | ❌ FAIL | Falla en ambos TF |
| MSFT | Acción tech | — | 100% | 1 | 1,84% | +0,26% | ⚠️ Muestra mínima | 1 trade — no concluyente |
| AAPL | Acción tech | 0 | 0% | 1 | 3,17% | -0,75% | ❌ FAIL | 1 trade perdedor |

---

## §23 · TABLA MAESTRA — TODOS LOS RESULTADOS (Daily + Weekly, Config 2)

*Visión consolidada de toda la campaña. PF "—" indica todos los trades ganadores (gross loss = 0).*

| Ticker | Tipo | PF Daily | Trades D | PF Weekly | Trades W | Veredicto final |
|---|---|---|---|---|---|---|
| **SPY** | ETF broad | **3,77** | 12 | — (100%) | 3 | ✅ **APTO** — núcleo robusto |
| **VGT** | ETF tech | **3,53** | 14 | — (100%) | 4 | ✅ **APTO** — núcleo robusto |
| **IGV** | ETF software | **1,65** | 18 | **2,64** | 4 | ✅ **APTO** — más estable en ambos TF |
| **SMH** | ETF semis | **1,33** | 8 | — (100%) | 1 | ✅ **APTO** — ampliar muestra |
| **MSFT** | Acción tech | **2,86** | 8 | — (100%) | 1 | ✅ **APTO** — mejor win rate acciones |
| **AMZN** | Acción tech | **7,26** | 6 | — (100%) | 1 | ⚠️ **VIGILAR** — PF alto, muestra mínima |
| **ILMN** | Acción biotech | 0,81 | 3 | **1,70** | 3 | ⚠️ **VIGILAR** — Weekly invierte Daily |
| VWO | ETF emergentes | 1,18 | 5 | — | 0 | ❌ **DESCARTAR** — PF marginal, sin señales Weekly |
| QQQ | ETF Nasdaq | 0,65 | 16 | 0,73 | 6 | ❌ **DESCARTAR** — falla en ambos TF |
| AAPL | Acción tech | 0,45 | 4 | 0 | 1 | ❌ **DESCARTAR** — falla en ambos TF |
| GLDM | ETF oro | 0,07 | 2 | 0 | 1 | ❌ **DESCARTAR** — commodity incompatible |
| ROP | Acción industrial | 0,10 | 7 | 0,74 | 2 | ❌ **DESCARTAR** — win rate 14% en Daily |
| VDE | ETF energía | 0,11 | 5 | 0 | 1 | ❌ **DESCARTAR** — sector incompatible |
| NVDA | Acción semis | 0,01 | 10 | 0 | 1 | ❌ **DESCARTAR** — peor resultado campaña |

---

## §24 · UNIVERSO OPERATIVO FINAL — PLAN A CONFIG 2 DAILY

| Tier | Tickers | Criterio | Acción |
|---|---|---|---|
| **Tier 1 — Núcleo** | SPY, VGT, IGV | PF > 1,5 en Daily, confirmado | Operar con Config 2 |
| **Tier 2 — Secundario** | SMH, MSFT | PF > 1,2 en Daily, muestra limitada | Operar con cautela |
| **Tier 3 — Vigilar** | AMZN, ILMN | PF alto pero muestra mínima o inconsistente entre TF | No operar hasta ampliar muestra |
| **Descartados** | QQQ, VWO, AAPL, GLDM, ROP, VDE, NVDA | Fallan en ambos TF o muestra insuficiente | No usar con Plan A |

---

## §25 · VEREDICTO FINAL POR HIPÓTESIS

### H1 — ¿El Plan A genera alfa sobre Buy & Hold? — **CONFIRMADA CON CONDICIONES**
Sí, en activos tech y broad market diversificado. SPY (PF 3,77), VGT (PF 3,53), IGV (PF 1,65), MSFT (PF 2,86) superan el umbral PF > 1,2 con robustez en Daily. El criterio de éxito se cumple en 5 de 14 tickers probados.

### H2 — ¿C7 mejora los resultados? — **REFUTADA en Daily automático**
C7 ON (Config 3) elimina prácticamente todas las señales en Daily. No viable para backtesting automático. Útil únicamente como filtro de convicción manual en el indicador visual.

### H3 — ¿C2=1 barra genera mejores resultados que C2=3? — **REFUTADA**
Degradación consistente. SPY: PF 3,77 → 1,01 sin el filtro de tendencia previa. C2=3 es componente de calidad real, no ruido.

### H4 — ¿Modo C (Cruce EMA) supera al Modo B (Trailing ATR)? — **CONFIRMADA**
SPY: Modo B PF 2,30 vs Modo C PF 3,77. Salida más natural dentro de la metodología Uhl.

### H5 — ¿Weekly añade valor sobre Daily? — **INDETERMINADA por muestra insuficiente**
Weekly confirma la dirección (lo que falla en Daily falla en Weekly) pero genera señales insuficientes para conclusiones independientes. Daily sigue siendo el timeframe operativo recomendado.

### H6 — ¿NVDA y activos de alta beta son aptos? — **REFUTADA**
NVDA: PF 0,01, DD 60,82%, win rate 10% en Daily. Alta volatilidad y gaps frecuentes destruyen el sistema. Activos de alta beta con movimientos no ordenados son incompatibles con el Plan A.

---

## §26 · PATRÓN ESTRUCTURAL DE LA CAMPAÑA

El sistema Plan A funciona en activos que comparten estas características:

| Característica | Presente en aptos | Ausente en descartados |
|---|---|---|
| Tendencia alcista sostenida y ordenada | SPY, VGT, IGV, MSFT | NVDA (volátil), VDE (cíclico), GLDM (commodity) |
| Pullbacks a EMA limpios y recuperables | SPY, VGT, MSFT | QQQ (pullbacks bruscos), NVDA (gaps) |
| Volumen institucional estable | SPY, VGT, IGV | ROP (baja liquidez relativa), VWO (emergentes) |
| Baja volatilidad relativa | VGT (DD 2%), MSFT (DD 2,5%) | NVDA (DD 60%), ROP (DD 13%) |

**Conclusión estructural:** el Plan A es un sistema de pullback institucional en tendencia. Funciona cuando el activo tiene tendencia ordenada, pullbacks a la EMA limpios y volumen institucional real. Falla cuando la volatilidad es tan alta que los stops se activan antes de que el trade se desarrolle.

---

## §27 · QUÉ NO SE PROBÓ

- **4H timeframe** — descartado en favor de Weekly; pendiente como campaña separada si se plantea uso del Plan A en intraday extendido.
- **Señales WATCH** (score == scoreMax-2) — excluidas en v1. Pueden ampliar la muestra en Tier 2 y Tier 3 sin bajar calidad. Candidatas para backtest v2.
- **Verificación operación por operación en VGT Weekly** (4 trades 100% win, +16%) y **AMZN Daily** (6 trades PF 7,26) — pendiente confirmar si la concentración en 1-2 trades explica el resultado, siguiendo el criterio §8 de esta campaña y el precedente de COPPER-M y VGT-M en la campaña SAR.
- **Config con C2=5 barras** — filtro más estricto, podría mejorar calidad en SMH y VWO a costa de menos trades.
- **Universo ampliado** — META, GOOGL, AMD en Daily podrían añadir evidencia al patrón tech confirmado.
- **Comisión 0%** como referencia de resultado bruto — no ejecutado.

---

## CIERRE DEFINITIVO DE LA CAMPAÑA — PLAN A BACKTEST v1

*14 tickers × 2 timeframes × 4 configuraciones = 68 combinaciones evaluadas.*

**Conclusiones que no se reabren salvo evidencia nueva:**

1. **El Plan A genera alfa real en activos tech y broad market diversificado en Daily.** SPY, VGT, IGV superan PF 1,2 con robustez. El criterio de éxito de la campaña se cumple.

2. **La configuración óptima es Config 2** — Modo C Cruce EMA, C2=3 barras, C7 OFF. Ninguna de las otras tres configuraciones probadas la supera de forma consistente.

3. **El sistema no es universal.** Falla sistemáticamente en energía (VDE), commodities (GLDM), industriales de baja beta (ROP), biotech individual (ILMN Daily), emergentes (VWO) y alta beta de semis (NVDA). El patrón de fallo es coherente y explicable por la estructura de cada activo.

4. **Daily supera a Weekly** por volumen de señales y robustez estadística. Weekly confirma la dirección pero no genera muestra suficiente para operar de forma sistemática.

5. **C7 no es viable en backtesting automático Daily.** Es un filtro de convicción útil en el indicador visual pero elimina señales en exceso cuando se usa como gate automático.

6. **Universo operativo recomendado:** SPY, VGT, IGV como núcleo (Tier 1). MSFT y SMH como secundarios (Tier 2). Operar únicamente en Daily con Config 2.

---

*Documento de proyecto — Ovtlier Plan A System. No es asesoramiento financiero.*

---

## §28 · VERIFICACIÓN DE CONCENTRACIÓN — CIERRE DE LOS 3 CASOS PENDIENTES

*Auditoría operación por operación. Mismo protocolo que campaña SAR (COPPER-M, VGT-M).*
*Umbral: si 1 operación explica > 50% del beneficio bruto → DESCARTADO POR CONCENTRACIÓN.*
*Fuente: CSV exportados del Strategy Tester de TradingView, 21 ago 2026.*

---

### Caso 1 — AMZN Daily — DESCARTADO + CORRECCIÓN DE DATOS

**Dato corregido:** el dashboard del backtest mostraba 6 trades y PF 7,26. El CSV exportado revela **18 trades cerrados** con net profit **-$28.928 (-2,89%)**.

| Trade | Fecha salida | PnL USD | Resultado |
|---|---|---|---|
| 1 | 1999-04-30 | +$19.708 | Ganador |
| 2 | 2002-08-27 | -$64.674 | **Perdedor** |
| 3 | 2002-12-09 | -$921 | Perdedor |
| 4 | 2003-10-24 | +$20.167 | Ganador |
| 5 | 2006-12-06 | -$20.736 | Perdedor |
| 6 | 2007-10-15 | +$20.310 | Ganador |
| 7 | 2009-06-16 | +$3.662 | Ganador |
| 8 | 2016-04-27 | -$9.150 | Perdedor |
| 9 | 2016-09-13 | +$13.030 | Ganador |
| 10 | 2017-02-01 | +$4.929 | Ganador |
| 11 | 2017-03-22 | -$14.391 | Perdedor |
| 12 | 2017-06-13 | +$9.557 | Ganador |
| 13 | 2019-07-31 | +$2.413 | Ganador |
| 14 | 2020-02-25 | +$9.750 | Ganador |
| 15 | 2024-02-21 | +$65 | Ganador |
| 16 | 2024-07-15 | -$2.315 | Perdedor |
| 17 | 2024-10-01 | -$12.893 | Perdedor |
| 18 | 2026-02-02 | -$7.439 | Perdedor |

**Análisis de concentración (ganadores):**
- Beneficio bruto total: $103.591
- Pérdida bruta total: -$132.520
- Operación mayor: Trade 6 (2007), $20.310 = **19,6% del bruto**
- Top 2 operaciones: $40.477 = **39,1% del bruto**

**¿Por qué el dashboard mostraba 6 trades?** El período visible en el gráfico estaba limitado al historial reciente. El CSV exporta el historial completo desde 1999 — 18 trades reales.

**Veredicto: DESCARTADO — net negativo confirmado**
Sin concentración excesiva (ninguna operación supera el 20% del bruto — bien distribuido), pero la pérdida bruta supera al beneficio bruto. AMZN no es un activo apto para el Plan A Daily con Config 2. El PF 7,26 del dashboard era un artefacto del período de visualización limitado, no un resultado real del sistema completo.

---

### Caso 2 — VGT Weekly — DESCARTADO POR RESULTADO REAL NEGATIVO

**Dato corregido:** el dashboard mostraba 4 trades con 100% win y +16,03%. El CSV exportado revela **39 trades cerrados** con net profit **+$5.682 (+0,57%)** — prácticamente break-even.

| Métrica | Dashboard (parcial) | CSV completo |
|---|---|---|
| Trades | 4 | **39** |
| Win Rate | 100% | **46,2% (18/39)** |
| Net Profit | +16,03% | **+0,57%** |
| Profit Factor | — | **~1,02** |

**Análisis de concentración (ganadores):**
- Beneficio bruto total: $252.767
- Pérdida bruta total: -$247.084
- Operación mayor: Trade 26 (jul 2021), $48.058 = **19,0% del bruto**
- Top 2 operaciones: $95.846 = **37,9% del bruto**

Sin concentración excesiva — el beneficio está bien distribuido entre 18 operaciones ganadoras. El problema no es concentración sino que beneficio bruto y pérdida bruta son prácticamente iguales.

**Veredicto: DESCARTADO — break-even después de costes**
El resultado real es +0,57% en toda la historia disponible. PF efectivo ≈ 1,02 — no supera el umbral de 1,2. El +16,03% del dashboard era artefacto del período de visualización. VGT Weekly no añade valor sobre Buy & Hold.

---

### Caso 3 — ILMN Weekly — DESCARTADO POR CONCENTRACIÓN + RESULTADO NEGATIVO

**Dato corregido:** el dashboard mostraba 3 trades con PF 1,70 y +6,87%. El CSV exportado revela **13 trades cerrados** con net profit **-$38.140 (-3,81%)**.

| Trade | Fecha salida | PnL USD | % del bruto ganador |
|---|---|---|---|
| 1 | 2003-10-10 | +$26.020 | 16,1% |
| 2 | 2003-12-11 | -$90.888 | — |
| 3 | 2009-10-26 | -$16.363 | — |
| 4 | 2011-02-23 | +$16.626 | 10,3% |
| 5 | 2011-07-13 | +$9.884 | 6,1% |
| 6 | 2013-09-20 | -$16.812 | — |
| **7** | **2013-12-27** | **+$90.144** | **55,7%** ⚠️ |
| 8 | 2017-05-10 | -$8.728 | — |
| 9 | 2017-12-06 | -$7.377 | — |
| 10 | 2019-07-15 | -$14.909 | — |
| 11 | 2021-07-16 | +$3.131 | 1,9% |
| 12 | 2026-02-02 | -$44.949 | — |
| 13 | 2026-04-27 | +$16.081 | 9,9% |

**Análisis de concentración:**
- Operación mayor (Trade 7, dic 2013): $90.144 = **55,7% del beneficio bruto** ⚠️ SUPERA UMBRAL
- Top 2 operaciones: **71,8% del beneficio bruto**
- Sin Trade 7: net profit = -$128.284 — el sistema destruye capital

**Veredicto: DESCARTADO POR CONCENTRACIÓN + RESULTADO NEGATIVO**
Una sola operación (dic 2013, rally post-earnings de ILMN) explica el 55,7% del beneficio bruto total — supera el umbral del 50%. Sin esa operación, el sistema pierde -$128.284. El PF 1,70 del dashboard era artefacto del período de visualización limitado a los 3 trades más recientes. Igual tratamiento que COPPER-M en la campaña SAR.

---

### Resumen de la auditoría de concentración

| Caso | Dashboard | CSV real | Causa del error | Veredicto |
|---|---|---|---|---|
| AMZN Daily | PF 7,26 · 6 trades · +6,13% | PF < 1 · 18 trades · -2,89% | Período de visualización limitado | ❌ DESCARTADO — net negativo |
| VGT Weekly | 4 trades · 100% win · +16,03% | 39 trades · 46% win · +0,57% | Período de visualización limitado | ❌ DESCARTADO — break-even |
| ILMN Weekly | PF 1,70 · 3 trades · +6,87% | PF < 1 · 13 trades · -3,81% | Período limitado + concentración 55,7% | ❌ DESCARTADO POR CONCENTRACIÓN |

**Los 3 casos fallan la auditoría.** Ninguno se promueve. La causa común es que el dashboard del backtest mostraba resultados sobre el historial visible en pantalla, no sobre el historial completo exportado en el CSV.

---

### Impacto en el universo operativo (tabla maestra actualizada)

| Tier anterior | Ticker | Veredicto auditoría | Tier final |
|---|---|---|---|
| Tier 3 Vigilar | AMZN | Net negativo en historial completo | ❌ **DESCARTADO** |
| Tier 1 Núcleo | VGT Weekly | Break-even en historial completo | ❌ **Weekly descartado** — Daily mantiene Tier 1 |
| Tier 3 Vigilar | ILMN | Concentración 55,7% + net negativo | ❌ **DESCARTADO** |

**Lección metodológica:** verificar siempre el CSV completo antes de clasificar un resultado como válido. El dashboard del Strategy Tester muestra resultados sobre el período visible en pantalla — si el gráfico no está en vista máxima de historial, los datos son parciales. Esta verificación pasa a ser paso obligatorio del protocolo de campaña para todas las pruebas futuras.


---

## §29 · AUDITORÍA CSV COMPLETA — TIER 1/2

*Mismo protocolo que §28. Historial máximo exportado del Strategy Tester.*
*Umbral de concentración: > 50% del beneficio bruto en 1 operación → DESCARTADO.*
*Fuente: CSV exportados 21 ago 2026.*

---

### SPY Daily — ⚠️ ALERTA DE CONCENTRACIÓN

| Trade | Fecha salida | PnL USD | PnL % |
|---|---|---|---|
| 1 | 2018-06-20 | +$1.734 | +0,17% |
| 2 | 2018-09-10 | +$5.043 | +0,51% |
| **3** | **2021-08-20** | **+$8.016** | **+0,80%** |
| 4 | 2025-08-04 | -$2.059 | -0,20% |

| Métrica | Dashboard previo | CSV completo |
|---|---|---|
| Trades | 12 | **4** |
| Win Rate | 58,3% | **75,0%** |
| PF | 3,77 | **7,19** |
| Net | +16,51% | **+1,28%** |

**Discrepancia explicada:** el historial disponible de SPY en este feed de TradingView solo cubre desde 2018 aproximadamente. Los 12 trades del dashboard previo procedían de un período de visualización distinto o de un feed con más historial. El CSV exportado ahora recoge solo 4 trades desde 2018.

**Concentración:** Trade 3 (ago 2021) = **54,2% del beneficio bruto** — supera el umbral del 50%.

**Veredicto: INDETERMINADO — muestra insuficiente**
4 trades no permiten ninguna conclusión estadística. El sistema no ha generado suficientes señales en el historial disponible en este feed para validar o descartar SPY. No se promueve ni se descarta — se marca como pendiente de verificación con feed de mayor historial (ej. datos ajustados desde 1993 si están disponibles en la cuenta de TradingView).

---

### VGT Daily — CONFIRMADO TIER 1

| Trade | Fecha salida | PnL USD | PnL % |
|---|---|---|---|
| 1 | 2016-06-14 | -$9.018 | -0,91% |
| **2** | **2016-09-13** | **+$15.453** | **+1,57%** |
| 3 | 2017-06-13 | +$7.484 | +0,75% |
| 4 | 2017-12-28 | +$2.672 | +0,26% |
| 5 | 2019-05-03 | -$3.438 | -0,34% |
| 6 | 2021-01-06 | -$4.388 | -0,43% |
| 7 | 2021-08-18 | +$1.015 | +0,10% |
| 8 | 2023-04-12 | +$8.785 | +0,88% |

| Métrica | Dashboard previo | CSV completo |
|---|---|---|
| Trades | 14 | **8** |
| Win Rate | 57,1% | **62,5%** |
| PF | 3,53 | **2,10** |
| Net | +7,46% | **+1,88%** |

**Concentración:** Trade 2 (sep 2016) = **43,6% del bruto** — por debajo del umbral del 50%.

**Veredicto: CONFIRMADO — baja concentración, resultado positivo**
PF 2,10 real, sin concentración excesiva, beneficio bien distribuido entre 5 operaciones ganadoras. La reducción respecto al dashboard (PF 3,53 → 2,10) es material pero el resultado sigue siendo positivo y superando el umbral 1,2. **VGT Daily se mantiene en Tier 1** con PF corregido a 2,10.

---

### IGV Daily — DESCARTADO POR CONCENTRACIÓN + RESULTADO NEGATIVO

| Trade | Fecha salida | PnL USD | PnL % |
|---|---|---|---|
| 1 | 2006-10-23 | -$6.780 | -0,68% |
| 2 | 2013-10-01 | -$3.653 | -0,37% |
| 3 | 2017-06-13 | +$9.628 | +0,98% |
| 4 | 2017-08-11 | -$18.183 | -1,83% |
| **5** | **2017-11-03** | **+$42.892** | **+4,37%** |
| 6 | 2018-03-21 | +$2.177 | +0,21% |
| 7 | 2018-09-07 | -$1.552 | -0,15% |
| 8 | 2020-02-24 | -$59.778 | **-5,86%** |
| 9 | 2020-09-09 | +$15.237 | +1,63% |
| 10 | 2024-01-04 | -$6.182 | -0,64% |

| Métrica | Dashboard previo | CSV completo |
|---|---|---|
| Trades | 18 | **10** |
| Win Rate | 50,0% | **40,0%** |
| PF | 1,65 | **0,73** |
| Net | +7,91% | **-2,34%** |

**Concentración:** Trade 5 (nov 2017) = **61,3% del bruto ganador** — supera el umbral del 50%.
**Trade 8 (feb 2020):** -$59.778 en una sola operación — el crash COVID destruye el sistema.

**Veredicto: DESCARTADO — net negativo + concentración 61,3%**
El beneficio real es -2,34%. Una sola operación (nov 2017, rally post-earnings sector software) explica el 61,3% del beneficio bruto, y el crash de febrero 2020 genera una pérdida individual de -$59.778 que el sistema no puede compensar. IGV falla la auditoría en ambos criterios. **Eliminado del universo operativo.**

---

### MSFT Daily — CONFIRMADO TIER 2

| Trade | Fecha salida | PnL USD | PnL % |
|---|---|---|---|
| 1 | 2004-11-24 | +$1.742 | +0,17% |
| **2** | **2009-10-05** | **+$19.129** | **+1,94%** |
| 3 | 2012-08-31 | +$10.019 | +0,98% |
| 4 | 2017-06-13 | +$9.992 | +0,98% |
| 5 | 2021-05-03 | +$13.963 | +1,36% |
| 6 | 2021-09-09 | +$2.531 | +0,24% |

| Métrica | Dashboard previo | CSV completo |
|---|---|---|
| Trades | 8 | **6** |
| Win Rate | 75,0% | **100,0%** |
| PF | 2,86 | **∞ (sin perdedores)** |
| Net | +3,71% | **+5,67%** |

**Concentración:** Trade 2 (oct 2009) = **33,3% del bruto** — bien por debajo del umbral.
Top 2 trades = 57,6% — aceptable con 6 operaciones.

**Veredicto: CONFIRMADO — resultado mejor que el dashboard**
100% win rate sobre 6 trades con historial completo, sin perdedores registrados, beneficio bien distribuido. La operación mayor (33,3%) no genera concentración excesiva. **MSFT Daily se mantiene en Tier 2** con resultado reforzado. Caveat: 6 trades en ~22 años sigue siendo muestra estadísticamente limitada.

---

### SMH Daily — CONFIRMADO TIER 2

| Trade | Fecha salida | PnL USD | PnL % |
|---|---|---|---|
| **1** | **2005-12-19** | **+$24.220** | **+2,43%** |
| **2** | **2006-01-19** | **+$28.303** | **+2,84%** |
| 3 | 2012-04-09 | -$9.587 | -0,91% |
| **4** | **2014-07-11** | **+$36.186** | **+3,47%** |
| 5 | 2018-09-10 | +$5.458 | +0,51% |
| 6 | 2019-04-30 | -$3.249 | -0,30% |
| 7 | 2025-07-24 | +$5.943 | +0,55% |

| Métrica | Dashboard previo | CSV completo |
|---|---|---|
| Trades | 8 | **7** |
| Win Rate | 50,0% | **71,4%** |
| PF | 1,33 | **7,80** |
| Net | +1,72% | **+8,59%** |

**Concentración:** Trade 4 (jul 2014) = **36,1% del bruto** — por debajo del umbral.
Top 2 trades = 64,4% — dentro de rango aceptable para 5 ganadores.

**Veredicto: CONFIRMADO — resultado significativamente mejor que el dashboard**
PF real 7,80 vs 1,33 en dashboard. Beneficio distribuido entre 5 operaciones ganadoras, ninguna supera el 40% del bruto. **SMH Daily asciende a Tier 1** por PF real 7,80 y win rate 71,4%.

---

### Resumen auditoría §29 — Tier 1/2

| Ticker | PF Dashboard | PF CSV real | Concentración | Veredicto | Tier final |
|---|---|---|---|---|---|
| SPY | 3,77 | 7,19 (4 trades) | 54,2% ⚠️ | INDETERMINADO — muestra insuficiente | ⏳ Pendiente |
| VGT | 3,53 | **2,10** | 43,6% ✅ | CONFIRMADO | ✅ **Tier 1** |
| IGV | 1,65 | **−0,73** | 61,3% ❌ | DESCARTADO | ❌ Eliminado |
| MSFT | 2,86 | **∞ (5,67%)** | 33,3% ✅ | CONFIRMADO | ✅ **Tier 2** |
| SMH | 1,33 | **7,80** | 36,1% ✅ | CONFIRMADO — asciende | ✅ **Tier 1** |

---

### Universo operativo final — post auditoría completa (§28 + §29)

| Tier | Ticker | PF real CSV | Win% real | Trades | Estado |
|---|---|---|---|---|---|
| **Tier 1** | VGT Daily | 2,10 | 62,5% | 8 | ✅ Confirmado |
| **Tier 1** | SMH Daily | 7,80 | 71,4% | 7 | ✅ Confirmado — ascendido |
| **Tier 2** | MSFT Daily | ∞ | 100,0% | 6 | ✅ Confirmado — muestra pequeña |
| **⏳ Pendiente** | SPY Daily | 7,19 | 75,0% | 4 | Indeterminado — historial insuficiente |
| ❌ Eliminado | IGV Daily | 0,73 | 40,0% | 10 | Descartado — net negativo + concentración |
| ❌ Eliminado | AMZN Daily | <1 | — | 18 | Descartado §28 |
| ❌ Eliminado | VGT Weekly | ~1,02 | 46,2% | 39 | Descartado §28 |
| ❌ Eliminado | ILMN Weekly | <1 | — | 13 | Descartado §28 — concentración 55,7% |

**Nota SPY:** para resolver el estado Pendiente se necesita un feed con historial desde 1993 en TradingView (datos ajustados por dividendos disponibles en plan Premium+). Con 4 trades desde 2018 no hay base estadística suficiente para ninguna conclusión.


---

## §30 · CAMPAÑA v2.1 — CONFIGS 5, 6 y 7 (C4 opcional + minScore)

*Script: ovtlier_plan_a_backtest.pine v2.1 · Fecha: 23 ago 2026*
*Tickers: SPY, VGT, IGV, SMH, MSFT · Timeframe: Daily (1D)*
*Todos los CSV exportados con historial máximo — protocolo §28/§29 aplicado.*
*Concentración auditada en todos los casos — umbral > 50% = alerta.*

| Config | C4 RVOL | minScore | Descripción |
|---|---|---|---|
| C2 | ON (obligatoria) | 4 | Base auditada §29 — referencia |
| **C5** | **OFF (puntuada)** | 4 | Sin gate RVOL, exigencia READY |
| C6 | ON (obligatoria) | **3** | RVOL obligatorio, incluye WATCH |
| **C7** | **OFF (puntuada)** | **3** | Sin gate RVOL, incluye WATCH |

*Nota: C7 en esta sección es la Config 7 del backtest (C4 OFF + minScore 3) — no confundir con la condición C7 (Divergencia RSI) del indicador.*

---

### SPY Daily

| Config | Trades | WR% | PF | Net% | Conc. top1 | Criterio |
|---|---|---|---|---|---|---|
| C2 (base auditada) | 4 | 75,0% | 7,19 | +1,28% | — | ✅ PASS |
| C5 (C4 OFF · 4) | 10 | 60,0% | 3,48 | +13,09% | OK 40,9% | ✅ PASS |
| C6 (C4 ON · 3) | 38 | 50,0% | 1,33 | +10,01% | OK 19,2% | ✅ PASS |
| C7-cfg (C4 OFF · 3) | — | — | — | — | — | No disponible |

**Hallazgo SPY:** C5 (sin gate RVOL, READY) es la mejor configuración verificada — PF 3,48 con 10 trades y sin concentración. Multiplica la muestra de C2 por 2,5 manteniendo calidad. C6 (WATCH + RVOL) genera 38 trades con PF 1,33 — útil para muestra estadística pero con menor calidad por trade.

---

### VGT Daily

| Config | Trades | WR% | PF | Net% | Conc. top1 | Criterio |
|---|---|---|---|---|---|---|
| C2 (base auditada) | 8 | 62,5% | 2,10 | +1,88% | — | ✅ PASS |
| C5 (C4 OFF · 4) | 13 | 61,5% | **4,74** | **+21,04%** | OK 27,8% | ✅ **PASS** |
| C6 (C4 ON · 3) | 45 | 53,3% | 1,66 | +29,32% | OK 13,7% | ✅ PASS |
| C7-cfg (C4 OFF · 3) | 88 | 50,0% | 1,18 | +21,79% | OK 5,6% | ❌ FAIL |

**Hallazgo VGT:** C5 es el mejor resultado de toda la campaña v2.1 — PF 4,74, WR 61,5%, sin concentración excesiva (27,8%). Eliminar el gate RVOL mejora el PF de 2,10 a 4,74 manteniendo la exigencia READY. C6 genera 45 trades con PF 1,66 y net +29,32% — mayor beneficio absoluto pero menor PF. C7-cfg (88 trades) cae justo por debajo del umbral (PF 1,18) — demasiadas señales de baja calidad.

---

### IGV Daily

| Config | Trades | WR% | PF | Net% | Conc. top1 | Criterio |
|---|---|---|---|---|---|---|
| C2 (base auditada) | 10 | 40,0% | 0,73 | -2,34% | — | ❌ FAIL |
| C5 (C4 OFF · 4) | 18 | 61,1% | **1,53** | **+13,80%** | OK 17,7% | ✅ **PASS** |
| C6 (C4 ON · 3) | 52 | 44,2% | 0,91 | -3,09% | OK 9,9% | ❌ FAIL |
| C7-cfg (C4 OFF · 3) | 93 | 44,1% | 1,08 | +13,06% | OK 5,6% | ❌ FAIL |

**Hallazgo IGV — reversión de veredicto:** IGV fue descartado en §29 con C2 (PF 0,73). Con C5 (sin gate RVOL) pasa a PF 1,53 con 18 trades y WR 61,1% — sin concentración. El gate RVOL era el responsable de eliminar las mejores entradas de IGV. C6 y C7-cfg fallan — añadir WATCH o mantener RVOL degradan el resultado. **IGV se readmite condicionalmente con Config 5.**

---

### SMH Daily

| Config | Trades | WR% | PF | Net% | Conc. top1 | Criterio |
|---|---|---|---|---|---|---|
| C2 (base auditada) | 7 | 71,4% | 7,80 | +8,59% | — | ✅ PASS |
| C5 (C4 OFF · 4) | 8 | 50,0% | 0,59 | -3,61% | ⚠️ 62,6% | ❌ FAIL |
| C6 (C4 ON · 3) | 38 | 50,0% | 1,04 | +5,13% | OK 15,5% | ❌ FAIL |
| C7-cfg (C4 OFF · 3) | 83 | 44,6% | 0,99 | +7,92% | OK 10,3% | ❌ FAIL |

**Hallazgo SMH:** SMH funciona bien únicamente con C2 (gate RVOL ON + READY estricto). Eliminar el gate RVOL o bajar el minScore degrada el resultado. El RVOL es un filtro de calidad real para SMH — las entradas válidas de semiconductores sí coinciden con picos de volumen. C2 sigue siendo la única configuración válida. C5 además tiene concentración 62,6% en una operación.

---

### MSFT Daily

| Config | Trades | WR% | PF | Net% | Conc. top1 | Criterio |
|---|---|---|---|---|---|---|
| C2 (base auditada) | 6 | 100,0% | inf | +5,67% | — | ✅ PASS |
| C5 (C4 OFF · 4) | 8 | 62,5% | **2,55** | **+11,39%** | OK 43,8% | ✅ **PASS** |
| C6 (C4 ON · 3) | 42 | 40,5% | 1,11 | +8,54% | OK 14,9% | ❌ FAIL |
| C7-cfg (C4 OFF · 3) | 82 | 45,1% | 1,19 | +23,27% | OK 6,9% | ❌ FAIL |

**Hallazgo MSFT:** C5 es la mejor configuración verificable (C2 con PF infinito no es estadísticamente interpretable). PF 2,55 con 8 trades y sin concentración. C6 y C7-cfg justo por debajo del umbral — MSFT también funciona mejor con READY estricto y sin gate RVOL.

---

### Nota — SPT (ticker no planificado)

El CSV `NASDAQ_SPT_2026-08-23` corresponde a un ticker no incluido en el plan de campaña. Resultado: 12 trades, WR 25%, PF 0,32, net -37,71%, concentración 62,8%. **Descartado** — no forma parte del universo operativo.

---

### Tabla maestra comparativa — mejor config por ticker

| Ticker | Config ganadora | PF | WR% | Trades | Net% | vs C2 base |
|---|---|---|---|---|---|---|
| SPY | **C5** (C4 OFF · 4) | **3,48** | 60,0% | 10 | +13,09% | ↑ PF, ↑ muestra |
| VGT | **C5** (C4 OFF · 4) | **4,74** | 61,5% | 13 | +21,04% | ↑↑ PF, ↑ muestra |
| IGV | **C5** (C4 OFF · 4) | **1,53** | 61,1% | 18 | +13,80% | ↑↑ readmitido |
| SMH | **C2** (C4 ON · 4) | **7,80** | 71,4% | 7 | +8,59% | C2 sigue siendo mejor |
| MSFT | **C5** (C4 OFF · 4) | **2,55** | 62,5% | 8 | +11,39% | ↑ PF, ↑ muestra |

**Config 5 (C4 OFF + minScore 4) gana en 4 de 5 tickers.** SMH es la excepción — el gate RVOL es un filtro de calidad real para semiconductores.

---

### Veredicto §30

**Hallazgo principal:** el gate RVOL obligatorio (C4 Required ON) era el mayor responsable de las pocas señales y de eliminar entradas de calidad en SPY, VGT, IGV y MSFT. Quitarlo manteniendo el threshold READY (minScore=4) mejora el PF en todos los casos excepto SMH.

**Bajar el minScore a 3 (WATCH)** genera muchos más trades pero degrada el PF de forma consistente — la exigencia READY es un filtro de calidad real que conviene mantener.

**Configuración recomendada actualizada:**

| Ticker | Config | C4 | minScore |
|---|---|---|---|
| SPY, VGT, IGV, MSFT | **Config 5** | OFF | 4 |
| SMH | **Config 2** | ON | 4 |

**Universo operativo actualizado post §30:**

| Tier | Ticker | Config | PF real |
|---|---|---|---|
| **Tier 1** | VGT | C5 | 4,74 |
| **Tier 1** | SMH | C2 | 7,80 |
| **Tier 1** | IGV | C5 | 1,53 (readmitido) |
| **Tier 2** | SPY | C5 | 3,48 |
| **Tier 2** | MSFT | C5 | 2,55 |


---

## §31 · CAMPAÑA 4H — CIERRE DEFINITIVO

*Script: ovtlier_plan_a_backtest.pine v2.1 · Fecha: 23 ago 2026*
*Config 5: Modo C Cruce EMA · C4 OFF · minScore 4 · MaxBars 6*
*Tickers: SPY, QQQ, AAPL, MSFT, NVDA · Timeframe: 4H*
*CSV exportados con historial máximo — protocolo §28/§29 aplicado.*

---

### Resultados

| Ticker | Trades | WR% | PF | Net% | Conc. top1 | Criterio |
|---|---|---|---|---|---|---|
| SPY | 55 | 34,5% | 0,47 | -8,81% | OK 14,5% | ❌ FAIL |
| QQQ | 91 | 34,1% | 0,70 | -10,40% | OK 13,6% | ❌ FAIL |
| AAPL | 47 | 34,0% | 0,48 | -21,42% | OK 25,0% | ❌ FAIL |
| MSFT | 37 | 29,7% | 0,87 | -2,71% | OK 31,7% | ❌ FAIL |
| NVDA | 34 | 41,2% | 0,49 | -22,18% | OK 17,2% | ❌ FAIL |

**0 PASS / 5 FAIL. Sin excepción.**

---

### Diagnóstico — por qué falla en 4H

**1. Win rate estructuralmente bajo (~34%)**
En Daily el sistema tenía WR 60-75%. En 4H cae a 30-41%. El ruido de las barras de 4H activa C2 (pullback a EMA) con mucha más frecuencia pero en condiciones que no son pullbacks reales — son simplemente oscilaciones intradía dentro de una barra mayor.

**2. La EMA9 en 4H no es soporte institucional**
En Daily la EMA9 representa ~2 semanas de precio — una zona donde los institucionales realmente defienden posiciones. En 4H la EMA9 son 36 horas — demasiado corta para tener significado como soporte. El precio la cruza y la recupera en la misma sesión sin que haya ningún cambio estructural.

**3. La salida por Cruce EMA es prematura en 4H**
Lo que observas en el gráfico — entradas tardías y salidas inmediatas — tiene una explicación técnica directa: en 4H el precio puede tocar la EMA, activar la entrada en el cierre de esa barra, y en la siguiente barra de 4H ya estar de vuelta bajo la EMA. El sistema entra y sale en 1-2 barras, capturando exactamente el peor momento del movimiento.

**Señal de salida dominante en 4H:**
- SPY: 46 de 55 salidas por Cruce EMA (83,6%)
- QQQ: 61 de 91 salidas por Cruce EMA (67,0%)
- AAPL: 35 de 47 salidas por Cruce EMA (74,5%)

El Cruce EMA en 4H actúa como salida inmediata porque la EMA9 de 4H es tan reactiva que cualquier corrección mínima la cruza.

**4. Margin calls frecuentes en QQQ y AAPL**
QQQ tiene 15 margin calls de 91 trades (16,5%). Son entradas donde el precio cae inmediatamente después de la entrada y la posición se liquida antes de que cualquier salida planificada pueda ejecutarse. Indica que las señales de 4H se generan frecuentemente justo antes de caídas bruscas.

---

### Veredicto — Plan A en 4H

**DESCARTADO — el Plan A no es compatible con el timeframe de 4H.**

El sistema fue diseñado para Daily/Weekly donde las EMAs tienen significado como soporte institucional y los pullbacks tardan días en resolverse. En 4H todos los parámetros del sistema — EMA9, C2, Cruce EMA de salida, MaxBars 6 — operan a una escala temporal donde el precio oscila más rápido de lo que el sistema puede reaccionar coherentemente.

Aumentar MaxBars o cambiar la EMA de salida no resolvería el problema estructural — el problema es que el Plan A evalúa condiciones diseñadas para swing (días) en un timeframe intraday extendido (horas).

**No se prueban más configuraciones en 4H.** El criterio de parada del protocolo se aplica: cuando el patrón de fallo es inequívoco en todos los activos probados, no se abren más hipótesis.

---

### Resumen final de la campaña completa Plan A Backtest

| Timeframe | Resultado | Universo operativo |
|---|---|---|
| **Daily** | ✅ Sistema válido | VGT/SMH/IGV (Tier 1), SPY/MSFT (Tier 2) con Config 5 |
| **Weekly** | ⚠️ Muestra insuficiente | IGV único resultado calculable (PF 2,64) |
| **4H** | ❌ Descartado | 0 de 5 tickers supera umbral |

**Conclusión definitiva:** el Plan A es un sistema de swing Daily. Su ventaja estadística real existe únicamente en ese timeframe con Config 5 (C4 OFF, minScore 4) en activos tech y broad market diversificado.

