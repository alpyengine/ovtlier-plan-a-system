# Ovtlier Plan A — Sistema de Indicadores Swing

> Indicadores Pine Script v6 para TradingView que implementan la metodología **Plan A** de swing trading de Christopher Uhl.

📖 [English version](./README.md)

---

## ¿Qué es el Plan A?

El Plan A es el marco de swing trading enseñado en **Ovtlier University** por Christopher Uhl. Está diseñado para **temporalidades Daily y Weekly** y se centra en comprar pullbacks de calidad dentro de regímenes de mercado alcistas confirmados.

A diferencia del Plan M (anomalías de momentum intradiario), el Plan A opera en temporalidades mayores y requiere paciencia — esperar a que el precio "respire" de vuelta a zonas de soporte institucional antes de entrar.

---

## Indicadores en este Repositorio

| Archivo | Descripción | Estado |
|---------|-------------|--------|
| `indicators/ovtlier_plan_a.pine` | Indicador principal de señales Plan A | ✅ Activo (v4) |

---

## Plan A — Condiciones Principales

El indicador evalúa **6 condiciones** más una verificación de régimen en timeframe superior opcional.

| # | Condición | Tipo | Lógica |
|---|-----------|------|--------|
| C1 | Régimen de Mercado Broad | **Obligatoria** | SPY + QQQ ambos por encima de EMA(20) |
| C1b | Régimen TF Superior | Opcional (puntuada u obligatoria) | SPY + QQQ en régimen en Weekly (si Daily) o Monthly (si Weekly) |
| C2+ | Pullback con Contexto de Tendencia | Puntuada | N barras previas sobre EMA rápida + Low toca EMA |
| C3 | Debilidad Relativa (RSI) | Puntuada | RSI(14) < 40 **o** RSI(2) < 10 |
| C4 | Volumen Relativo (RVOL) | Puntuada | Volumen > 1.5× media |
| C5 | Vela de Reversión (Hammer) | Puntuada | Mecha inferior ≥ 2× cuerpo |
| C6 | Tendencia del Activo | Puntuada / Opcional* | Close > SMA(50), Close > SMA(200), SMA(50) > SMA(200) |

**Niveles de señal:**
- 🟢 **READY** — C1 pasa + score >= scoreMax−1
- 🟡 **WATCH** — C1 pasa + score == scoreMax−2
- 🔴 **NOT READY** — C1 falla o score insuficiente

*El `scoreMax` es dinámico: 5 (TF superior OFF o obligatorio) o 6 (TF superior ON y puntuado).*

---

## Inputs Configurables

### 📊 Régimen de Mercado C1
| Input | Default | Descripción |
|-------|---------|-------------|
| EMA Régimen SPY | 20 | Longitud EMA para el check de SPY |
| EMA Régimen QQQ | 20 | Longitud EMA para el check de QQQ |
| Activar doble TF | OFF | Activa la verificación de régimen en el TF superior |
| TF superior obligatorio | OFF | ON = gate obligatoria; OFF = suma +1 al score |

### 🎯 C2 Pullback Mejorado
| Input | Default | Descripción |
|-------|---------|-------------|
| C2 Barras tendencia previa | 3 | Número de barras previas que deben haber cerrado sobre la EMA rápida antes del pullback. Rango: 1–10. La tabla muestra en tiempo real cuántas barras pasaron el check (`X/N barras OK`). |

> **¿Por qué importa este número?** Con 1 barra el filtro es mínimo — atrapa dips en rangos laterales. Con 5-10 barras el filtro es estricto — solo señales con tendencia previa clara. El default de 3 barras equilibra sensibilidad y calidad.

### 📈 Filtro Tendencia C6
| Input | Default | Descripción |
|-------|---------|-------------|
| SMA Media | 50 | Longitud de la SMA intermedia |
| SMA Larga | 200 | Longitud de la SMA primaria |
| C6 obligatoria | OFF | ON = sin estructura alcista no hay señal; OFF = puntúa 0 o 1 |

### ⚠️ Risk Management
| Input | Default | Descripción |
|-------|---------|-------------|
| ATR Longitud | 14 | Longitud del ATR para calcular el Stop Loss |
| ATR Stop-Loss × | 1.5 | Multiplicador ATR. SL = Low − (ATR × multiplicador) |

---

## Tabla en Gráfico — Columnas (v4)

| Columna | Contenido |
|---------|-----------|
| CONDICIÓN | Código y nombre (C1, C1b, C2+, C3–C6) |
| DESCRIPCIÓN | Lógica resumida + configuración activa |
| VALOR | Lectura numérica en tiempo real |
| STATUS | Badge PASS / FAIL |

**Fila especial C2+:** el campo VALOR muestra `X/N barras OK` — el número de barras previas que cerraron sobre la EMA rápida vs. las requeridas. Si el contexto de tendencia falla, el texto aparece en rojo.

**Fila C1b:** visible siempre pero visualmente atenuada cuando está desactivada. Cuando está activa muestra `SPY OK/X  QQQ OK/X` para el TF superior y el modo (OBLIGATORIA / PUNTUADA +1).

**Fila de configuración (row 11):** resumen compacto de los inputs clave activos, por ejemplo `C2:3b  C6:punt  TFsup:OFF`.

---

## Cómo Usarlo

1. Abre TradingView → Pine Editor → pega el contenido de `indicators/ovtlier_plan_a.pine`
2. Añade al gráfico en temporalidad **Daily o Weekly**
3. Ajusta `C2 Barras tendencia previa` según tu tolerancia:
   - **3 barras** (default): balance entre sensibilidad y calidad
   - **5+ barras**: más estricto, solo tendencias muy establecidas
4. Activa `Doble TF` si quieres confirmar que el régimen también es alcista en Weekly/Monthly
5. Lee la tabla en el gráfico (posición configurable desde los inputs)
6. Opera solo señales READY — nunca fuerces un trade cuando C1 falla

---

## Gestión del Riesgo

Cada señal incluye un **Stop Loss sugerido** calculado como:
```
SL = Minimo de barra - (ATR(14) x multiplicador)
```
Multiplicador por defecto: **1.5×**. La tabla muestra el nivel absoluto y la distancia porcentual al cierre actual.

> ⚠️ Este indicador es una herramienta educativa. No constituye asesoramiento financiero. Aplica siempre tus propias reglas de gestión del riesgo antes de entrar en cualquier posición.

---

## Plan A vs Plan M

| | Plan A | Plan M |
|---|---|---|
| Temporalidad | Daily / Weekly | Intradiario (15m / 1H) |
| Periodo de holding | 3-10 dias | Intradiario / 1-2 dias |
| Logica principal | Comprar el dip en regimen alcista | Deteccion de anomalias de momentum |
| Perfil de riesgo | Swing, riesgo definido | Intradiario, stops mas rapidos |

---

## Versionado

- **Versiones mayores** (`v1`, `v2`...) introducen funcionalidades nuevas o cambios estructurales significativos.
- **Versiones menores** (`v1.1`, `v2.1`...) son refinamientos o correcciones dentro del mismo conjunto de funcionalidades.

El historial completo de versiones esta en [CHANGELOG.md](./CHANGELOG.md).

---

## Autor

Construido sobre las ensenanzas de **Christopher Uhl — Ovtlier University**.
Codificado por [alpyengine](https://github.com/alpyengine).

---

## Licencia

[Mozilla Public License 2.0](./LICENSE) · © Ovtlier Plan A System contributors.
Basado en la metodologia Ovtlier University de Christopher Uhl. Para uso educativo.
