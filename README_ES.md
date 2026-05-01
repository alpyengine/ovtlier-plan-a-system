# Ovtlier Plan A — Sistema de Indicadores Swing

> Indicadores Pine Script v6 para TradingView que implementan la metodología **Plan A** de swing trading de Christopher Uhl.

📖 [English version](./README.md)

---

## ¿Qué es el Plan A?

El Plan A es el marco de swing trading enseñado en **Ovtlier University** por Christopher Uhl. Está diseñado para **temporalidades Daily y Weekly** y se centra en comprar pullbacks de calidad dentro de regímenes de mercado alcistas confirmados.

---

## Indicadores en este Repositorio

| Archivo | Descripción | Estado |
|---------|-------------|--------|
| `indicators/ovtlier_plan_a.pine` | Indicador principal de señales Plan A | ✅ Activo (v5) |

---

## Plan A — Condiciones Principales

El indicador evalúa **7 condiciones** más una verificación de régimen en timeframe superior opcional.

| # | Condición | Tipo | Lógica |
|---|-----------|------|--------|
| C1 | Régimen de Mercado Broad | **Obligatoria** | SPY + QQQ ambos por encima de EMA(20) |
| C1b | Régimen TF Superior | Opcional (puntuada u obligatoria) | SPY + QQQ en régimen en Weekly (si Daily) o Monthly (si Weekly) |
| C2+ | Pullback con Contexto de Tendencia | Puntuada | N barras previas sobre EMA rápida + Low toca EMA |
| C3 | Debilidad Relativa (RSI) | Puntuada | RSI(14) < 40 **o** RSI(2) < 10 |
| C4 | Volumen Relativo (RVOL) | Puntuada | Volumen > 1.5× media |
| C5 | Vela de Reversión (Hammer) | Puntuada | Mecha inferior ≥ 2× cuerpo |
| C6 | Tendencia del Activo | Puntuada / Opcional | Close > SMA(50), Close > SMA(200), SMA(50) > SMA(200) |
| C7 | Divergencia Alcista RSI | Puntuada / Opcional | Precio: mínimo más bajo (LL) + RSI: mínimo más alto (HL) |

**Score máximo dinámico:**
```
scoreMax = 5 (base C2-C6)
         + 1 si TF superior activado en modo puntuado
         + 1 si C7 activado en modo puntuado
         = máximo 7
```

**Niveles de señal:**
- 🟢 **READY** — C1 pasa + score >= scoreMax−1
- 🟡 **WATCH** — C1 pasa + score == scoreMax−2
- 🔴 **NOT READY** — C1 falla o score insuficiente

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
| C2 Barras tendencia previa | 3 | Barras previas que deben cerrar sobre EMA rápida antes del pullback. Rango: 1–10. La tabla muestra `X/N barras OK` en tiempo real. |

### 📉 Divergencia RSI — C7
| Input | Default | Descripción |
|-------|---------|-------------|
| Activar C7 Divergencia RSI | ON | Activa la condición de divergencia alcista RSI |
| C7 Ventana barras (lookback) | 5 | Barras hacia atrás para buscar el mínimo previo de precio y RSI. Más barras = divergencias más largas. Rango: 2–20. |
| C7 obligatoria | OFF | ON = sin divergencia no hay señal; OFF = suma +1 al score |
| Mostrar línea divergencia | ON | Dibuja una línea punteada verde conectando los dos mínimos cuando se detecta divergencia |

> **¿Qué es la divergencia alcista RSI?** Cuando el precio marca un mínimo más bajo que N barras atrás pero el RSI marca un mínimo más alto — el momentum bajista se está agotando aunque el precio siga cayendo. Es una de las señales de reversión más fiables en la metodología Uhl, especialmente cuando coincide con C2 (pullback al soporte) y C3 (RSI en sobreventa).

> **Cómo interpretar el VALOR de C7:** La tabla muestra `RSIdiff +X.X  LB:Nb`. Un `RSIdiff` positivo confirma que el RSI actual está por encima del RSI mínimo de la ventana — divergencia real. Un valor negativo indica que no hay divergencia aunque el precio haya bajado.

### 📈 Filtro Tendencia C6
| Input | Default | Descripción |
|-------|---------|-------------|
| SMA Media | 50 | Longitud de la SMA intermedia |
| SMA Larga | 200 | Longitud de la SMA primaria |
| C6 obligatoria | OFF | ON = sin estructura alcista no hay señal |

### ⚠️ Risk Management
| Input | Default | Descripción |
|-------|---------|-------------|
| ATR Longitud | 14 | Longitud del ATR |
| ATR Stop-Loss × | 1.5 | SL = Low − (ATR × multiplicador) |

---

## Tabla en Gráfico — Estructura (v5)

| Fila | Contenido |
|------|-----------|
| 0 | Cabecera (CONDICION / VALOR / STATUS) |
| 1 | Ticker + TF + estado SPY/QQQ |
| 2 | C1 Régimen — obligatoria |
| 3 | C1b TF Superior — opcional |
| 4 | C2+ Pullback + contexto de tendencia |
| 5 | C3 RSI |
| 6 | C4 RVOL |
| 7 | C5 Hammer |
| 8 | C6 Tendencia SMA |
| 9 | **C7 Divergencia RSI** ← nueva en v5 |
| 10 | Score + SL precio y % |
| 11 | Resultado final (READY / WATCH / NOT READY) |
| 12 | Config activa resumida |
| 13 | Versión + fecha |

---

## Cómo Usarlo

1. Abre TradingView → Pine Editor → pega `indicators/ovtlier_plan_a.pine`
2. Añade al gráfico en temporalidad **Daily o Weekly**
3. Ajusta los inputs según tu estilo:
   - **Conservador:** C6 obligatoria + C7 obligatoria + TF sup obligatorio
   - **Balanceado (default):** todo en modo puntuado
   - **Sensible:** reduce C2 a 1-2 barras, desactiva C7
4. Lee la tabla — presta atención especial a C7 cuando C3 también pasa (doble confirmación RSI)
5. La línea verde punteada en el gráfico marca visualmente la divergencia detectada
6. Opera solo señales READY

---

## Gestión del Riesgo

```
SL = Minimo de barra - (ATR(14) x multiplicador)
```
Multiplicador por defecto: **1.5×**. La tabla muestra nivel absoluto y distancia porcentual.

> ⚠️ Este indicador es una herramienta educativa. No constituye asesoramiento financiero.

---

## Estructura del Repositorio

```
ovtlier-plan-a-system/
├── indicators/
│   └── ovtlier_plan_a.pine          <- v5 (activo)
├── screeners/                        <- scanner v1 (próximo)
├── strategies/                       <- backtest v1 (próximo)
├── README.md
├── README_ES.md
├── CHANGELOG.md
└── LICENSE
```

> Los ficheros `scanner` y `backtest` son proyectos independientes con su propio ciclo de versiones (`scanner/v1`, `backtest/v1`), separados del indicador principal.

---

## Plan A vs Plan M

| | Plan A | Plan M |
|---|---|---|
| Temporalidad | Daily / Weekly | Intradiario (15m / 1H) |
| Periodo de holding | 3-10 dias | Intradiario / 1-2 dias |
| Logica principal | Comprar el dip en regimen alcista | Deteccion de anomalias de momentum |
| Perfil de riesgo | Swing, riesgo definido | Intradiario, stops mas rapidos |

---

## Autor

Construido sobre las ensenanzas de **Christopher Uhl — Ovtlier University**.
Codificado por [alpyengine](https://github.com/alpyengine).

## Licencia

[Mozilla Public License 2.0](./LICENSE) · © Ovtlier Plan A System contributors.
