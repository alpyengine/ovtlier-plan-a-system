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
| `indicators/ovtlier_plan_a.pine` | Indicador principal de señales Plan A | ✅ Activo (v3) |

---

## Plan A — Condiciones Principales

El indicador evalúa **6 condiciones**: una obligatoria fija, una obligatoria opcional y cuatro/cinco con puntuación.

| # | Condición | Tipo | Lógica |
|---|-----------|------|--------|
| C1 | Régimen de Mercado Broad | **Obligatoria** | SPY + QQQ ambos por encima de EMA(20) |
| C2 | Pullback a Zona de Valor | Puntuada | Low ≤ EMA(9), Close ≥ EMA(21)×0.98 |
| C3 | Debilidad Relativa (RSI) | Puntuada | RSI(14) < 40 **o** RSI(2) < 10 |
| C4 | Volumen Relativo (RVOL) | Puntuada | Volumen > 1.5× media |
| C5 | Vela de Reversión (Hammer) | Puntuada | Mecha inferior ≥ 2× cuerpo |
| C6 | Tendencia del Activo | Puntuada / Opcional* | Close > SMA(50), Close > SMA(200), SMA(50) > SMA(200) |

*C6 puede configurarse como obligatoria (input `C6 obligatoria`). Por defecto puntúa como condición técnica estándar.

**Niveles de señal (sistema 0–5):**
- 🟢 **READY** — C1 pasa + 4 o 5 de C2–C6
- 🟡 **WATCH** — C1 pasa + exactamente 3 de C2–C6
- 🔴 **NOT READY** — C1 falla o puntuación < 3

---

## Tabla en Gráfico — Columnas (v3)

| Columna | Contenido |
|---------|-----------|
| CONDICIÓN | Código y nombre (C1–C6) |
| DESCRIPCIÓN | Lógica resumida del check |
| VALOR | **Lectura numérica en tiempo real** (RSI, RVOL ratio, distancia SMA, etc.) |
| STATUS | Badge PASS / FAIL |

---

## Estructura del Repositorio

```
ovtlier-plan-a-system/
├── indicators/
│   └── ovtlier_plan_a.pine       <- versión activa
├── strategies/                    <- backtests futuros
├── libs/                          <- librerías compartidas futuras
├── README.md
├── README_ES.md
├── CHANGELOG.md
└── LICENSE
```

---

## Cómo Usarlo

1. Abre TradingView → Pine Editor → pega el contenido de `indicators/ovtlier_plan_a.pine`
2. Añade al gráfico en temporalidad **Daily o Weekly**
3. Lee la tabla en el gráfico (posición configurable desde los inputs)
4. Opera solo señales READY — nunca fuerces un trade cuando C1 (régimen) falla
5. Verifica C6: si el activo está por debajo de SMA 50/200, la señal es de menor calidad

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
