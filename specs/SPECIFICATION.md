# ESPECIFICACIÓN TÉCNICA — v1.0

## Proyecto: Plataforma de Inversiones con IA

**Código del Proyecto**: `pwa_inversions_drfic`
**Categoría**: PWA (Progressive Web App)
**Tech Stack Principal**: React + TypeScript + Vite + TailwindCSS
**Versión**: 1.2
**Fecha**: 2026-03-11
**Autor**: Dr. Francisco Ibarra Carlos
**Estado**: 🟡 En Especificación
**Cambios v1.2**: Arquitectura por cores independientes, ranking diario de oportunidades, análisis fundamental/eventos, confluencia configurable y motor de estrategias sobre opciones

---

## 1. Visión General

### 1.1 Objetivo

Desarrollar una **Plataforma Web de Inversiones asistida por Inteligencia Artificial** que permita detectar señales de compra y venta de alta confianza en el mercado de acciones y opciones de EE.UU. (S&P 500 / SPY / QQQ y sus derivados), combinando análisis técnico multicapa (RSI, MACD, Bollinger Bands, EMA/SMA, Volume), análisis de la cadena de opciones, monitoreo de flujo institucional, y confirmación mediante IA (Claude API), todo integrado con Interactive Brokers (IBKR) como broker primario y Alpaca como entorno de desarrollo y paper trading.

### 1.2 Filosofía de la Plataforma

La plataforma opera bajo el modelo **semi-automático**: el cerebro de decisión vive dentro del proyecto como un conjunto de **cores programados y desacoplados**, cada uno especializado en una fuente de verdad distinta. La IA no reemplaza la lógica base; actúa como un core adicional de análisis y sugerencia.

Cada core debe generar señales de compra y venta de forma independiente para el instrumento seleccionado, con su propio score, confianza, razones y contexto. El usuario decide qué cores participan en la decisión final y el sistema solo combina las coincidencias entre los cores activados.

No existe ejecución automática sin intervención humana en v1.0. La plataforma debe:

- descubrir oportunidades diarias de mayor prioridad
- analizar en profundidad el instrumento seleccionado
- combinar coincidencias entre indicadores, estructura técnica, institucionales, noticias, fundamentales e IA
- sugerir la mejor estrategia de opciones para el contexto actual
- permitir la ejecución manual asistida en el broker elegido

```
Mercado + Noticias + Fundamentales + Opciones + Eventos
  ↓ [Data ingestion unificado]
Cores independientes del proyecto
  ↓ [Indicadores | Estructura | Institucionales | Noticias | Fundamentales | IA]
Core de Ranking Diario de Oportunidades
  ↓ [Qué empresas/instrumentos conviene vigilar hoy]
Core de Confluencia
  ↓ [Coincidencias entre cores seleccionados por el usuario]
Core de Estrategias y Opciones
  ↓ [Prioriza estrategias y propone calls/puts]
Dashboard de Oportunidades + Señales
  ↓ [Trader evalúa → Aprueba con 1 click]
Ejecución Asistida en Broker
  ↓ [Confirmación + Log + Alertas + Histórico]
```

### 1.3 Resultado Esperado de v1.0

- **Input**: Watchlist configurable, universos de análisis, instrumentos seleccionados y configuración de cores/estrategias
- **Output**: Ranking diario de oportunidades + señales BUY/SELL/HOLD por core + señal combinada + estrategias sugeridas + propuesta de ejecución en opciones
- **Modo de Operación**: Semi-automático (señal generada por IA → aprobación manual → ejecución)
- **Mercados**: Acciones US + Opciones sobre acciones (cadena de opciones)
- **Broker Primario**: Interactive Brokers (TWS API / IB Gateway)
- **Broker Desarrollo**: Alpaca (paper trading y validación de señales)
- **Timeframes**: 1m, 5m, 15m, 1h, 4h, 1D, 1W, 1M, 1Y (9 timeframes completos)
- **Capacidad Clave**: El sistema puede crecer incorporando más indicadores, más estrategias, más fuentes de noticias y más reglas de institucionales sin rehacer el núcleo existente

---

## 2. Entrada (Input)

### 2.1 Fuentes de Datos de Mercado

#### Fuente Primaria: Interactive Brokers TWS API
```
Librería: @stoqey/ib (Node.js wrapper oficial)
Conexión: TWS local (puerto 7497) o IB Gateway (puerto 4001)
Datos disponibles:
  - Cotizaciones en tiempo real (Bid/Ask/Last/Volume)
  - Datos históricos OHLCV (velas)
  - Cadena de opciones (strikes, expirations, IV, delta, gamma, theta, vega)
  - Nivel 2 (profundidad de mercado)
  - Open Interest
Variables de entorno:
  - IBKR_HOST=127.0.0.1
  - IBKR_PORT=7497          (TWS) | 4001 (Gateway)
  - IBKR_CLIENT_ID=1
  - IBKR_ACCOUNT_ID=<cuenta>
```

#### Fuente Secundaria/Desarrollo: Alpaca Markets API
```
Librería: @alpacahq/alpaca-trade-api + WebSocket nativo
Tipo: REST + WebSocket streaming
Datos disponibles:
  - Cotizaciones en tiempo real (paper + live)
  - Datos históricos OHLCV
  - Noticias del mercado
Variables de entorno:
  - ALPACA_API_KEY=<key>
  - ALPACA_SECRET_KEY=<secret>
  - ALPACA_BASE_URL=https://paper-api.alpaca.markets  (dev)
  - ALPACA_DATA_URL=https://data.alpaca.markets
```

### 2.2 Watchlist (Input del Usuario)

```
Configuración: JSON editable desde la UI (Settings > Watchlist)
Estructura:
  {
    "watchlist": [
      { "symbol": "SPY",  "type": "ETF",    "sector": "Index", "active": true },
      { "symbol": "QQQ",  "type": "ETF",    "sector": "Tech",  "active": true },
      { "symbol": "AAPL", "type": "Stock",  "sector": "Tech",  "active": true },
      { "symbol": "TSLA", "type": "Stock",  "sector": "Auto",  "active": true },
      ...
    ]
  }
Límite v1.0: 20 símbolos activos simultáneos
Almacenamiento: localStorage + exportable a JSON
```

### 2.3 Parámetros de Configuración de Señales

```
Configuración: Editable desde UI (Settings > Signal Config)
Parámetros por defecto:

  Cores habilitables por el usuario:
    technical_indicators: true
    technical_structure: true
    institutional_flow: true
    news_events: true
    fundamentals: true
    ai_advisor: true

  Pesos base por core:
    technical_indicators: 1.0
    technical_structure: 1.0
    institutional_flow: 1.0
    news_events: 0.9
    fundamentals: 0.8
    ai_advisor: 0.7

  RSI:
    period: 14
    oversold_threshold: 30        (señal BUY)
    overbought_threshold: 70      (señal SELL)
  
  MACD:
    fast_period: 12
    slow_period: 26
    signal_period: 9
  
  Bollinger Bands:
    period: 20
    std_dev: 2
  
  EMA:
    fast: 9
    slow: 21
    trend: 50
  
  ATR:
    period: 14
    volatility_threshold: 0.02    (2% = alta volatilidad)
  
  Signal Confidence:
    min_confirmation_cores: 2        (mínimo de cores alineados)
    high_confidence_threshold: 0.75  (≥75% = señal de alta confianza)
    medium_confidence_threshold: 0.50

  Opportunity Ranking:
    enabled: true
    refresh_interval_minutes: 5
    top_n_daily_candidates: 25
    minimum_opportunity_score: 0.60

  Eventos y noticias:
    lookahead_hours: 72
    macro_events_enabled: true
    earnings_events_enabled: true
    analyst_updates_enabled: true

  Fundamentales:
    use_earnings_surprise: true
    use_guidance: true
    use_revenue_growth: true
    use_profitability: true
    use_valuation_multiples: true
  
  Timeframes disponibles: ["1m", "5m", "15m", "1h", "4h", "1D", "1W", "1M", "1Y"]
  Timeframe principal UI: "15m"
  
  Nota de datos históricos por timeframe:
    1m  → últimas 500 velas  (~8 horas de mercado)
    5m  → últimas 500 velas  (~2 días)
    15m → últimas 500 velas  (~5 días)
    1h  → últimas 500 velas  (~3 meses)
    4h  → últimas 500 velas  (~10 meses)
    1D  → últimas 500 velas  (~2 años)
    1W  → últimas 260 velas  (~5 años)
    1M  → últimas 120 velas  (~10 años)
    1Y  → últimas 30 velas   (~30 años — disponibilidad según broker)
```

### 2.4 Parámetros de Gestión de Riesgo

```
Configuración: Editable desde UI (Settings > Risk Management)
Parámetros por defecto:

  max_position_size_pct: 5        (máximo 5% del portafolio por posición)
  max_daily_loss_pct: 2           (stop total del día al -2%)
  default_stop_loss_pct: 1.5      (stop loss automático sugerido)
  default_take_profit_pct: 3.0    (take profit sugerido = 2:1 R/R)
  max_concurrent_positions: 5     (máximo 5 posiciones abiertas)
  
  Opciones:
    max_iv_percentile: 80         (no entrar si IV > percentil 80)
    preferred_dte_range: [7, 45]  (días a expiración preferidos)
    max_option_premium_pct: 2     (máximo 2% del portafolio por prima)
```

### 2.5 Sistema de Estrategias (Strategy Manager)

```
Propósito: Permitir al usuario definir QUÉ combinación de cores,
        filtros y reglas activa una señal y cuál estrategia de opciones
        debe priorizarse para el contexto actual.

TIPOS DE ESTRATEGIA:

A. PRESETS PREDEFINIDOS (incluidos en v1.0):
   ┌─────────────────────────────────────────────────────────────┐
  │ ID  │ Nombre                    │ Cores principales         │
   ├─────┼───────────────────────────┼───────────────────────────┤
  │ P01 │ RSI + MACD Crossover      │ RSI + MACD                │
  │ P02 │ Bollinger Breakout        │ Bollinger + Volume        │
  │ P03 │ Triple EMA Trend          │ EMA + Structure           │
  │ P04 │ Full Confluence (default) │ Technical + Structure + IA│
  │ P05 │ Institutional Momentum    │ Technical + Institutional │
  │ P06 │ Earnings Event Trader     │ News + Events + Structure │
  │ P07 │ Value + Catalyst Swing    │ Fundamentals + News       │
   └─────┴───────────────────────────┴───────────────────────────┘

B. SELECTOR MANUAL (personalización libre):
  El usuario activa/desactiva CADA core individualmente:
  ☑ Indicadores técnicos
  ☑ Soportes / Resistencias / Tendencias
  ☑ Flujo institucional
  ☑ Noticias y eventos
  ☑ Fundamentales
  ☑ IA de sugerencia

  Dentro de cada core, el usuario activa/desactiva elementos específicos:
   ☑ RSI         period: [_14_]   oversold: [_30_]  overbought: [_70_]
   ☑ MACD        fast: [_12_]     slow: [_26_]      signal: [_9_]
   ☑ Bollinger   period: [_20_]   stdDev: [_2_]
   ☑ EMA         fast: [_9_]      slow: [_21_]      trend: [_50_]
   ☐ ATR         period: [_14_]
   ☑ Volume      avgPeriod: [_20_]  ratio_threshold: [_1.5_]
  ☑ Support/Resistance  swing_lookback: [_50_]
  ☑ Trend Bias          multi_tf: [_15m_][_1h_][_1D_]
  ☑ Institutional       UOA: [on]   OI sweeps: [on]
  ☑ News                real_time_feeds: [on]
  ☑ Fundamentals        earnings + guidance + valuation
  ☑ IA Advisor          suggest_buy_sell: [on]
   
  Cada core e indicador puede tener su PESO en el score ajustado.

C. GUARDAR ESTRATEGIA PERSONALIZADA:
   El usuario puede guardar su configuración con nombre propio:
   Nombre: [Mi Estrategia SPY Intraday____________]
   Descripción: [RSI oversold + MACD cross en 15m___]
   Timeframes: ☑15m  ☑1h  ☐otros
   [💾 GUARDAR COMO ESTRATEGIA]
   
   Almacenamiento: localStorage → strategies[]
   Límite: 20 estrategias personalizadas guardadas

ESTRUCTURA DE DATOS:
  interface Strategy {
    id: string                          // UUID o "preset_P01"
    name: string                        // "RSI + MACD Crossover"
    description: string
    isPreset: boolean
    enabledCores: {
      technicalIndicators: boolean
      technicalStructure: boolean
      institutionalFlow: boolean
      newsEvents: boolean
      fundamentals: boolean
      aiAdvisor: boolean
    }
    indicators: {
      rsi:       { active: boolean, period: number, oversold: number, overbought: number, weight: number }
      macd:      { active: boolean, fast: number, slow: number, signal: number, weight: number }
      bollinger: { active: boolean, period: number, stdDev: number, weight: number }
      ema:       { active: boolean, fast: number, slow: number, trend: number, weight: number }
      atr:       { active: boolean, period: number, weight: number }
      volume:    { active: boolean, avgPeriod: number, ratioThreshold: number, weight: number }
    }
    structure: {
      supportsResistances: { active: boolean, weight: number }
      trends:              { active: boolean, weight: number }
    }
    institutional: {
      unusualOptions: { active: boolean, weight: number }
      openInterest:   { active: boolean, weight: number }
    }
    news: {
      realTimeNews: { active: boolean, weight: number }
      eventRisk:    { active: boolean, weight: number }
    }
    fundamentals: {
      earnings:     { active: boolean, weight: number }
      valuation:    { active: boolean, weight: number }
      growth:       { active: boolean, weight: number }
    }
    aiAdvisor: {
      active: boolean
      weight: number
    }
    optionStrategies: string[]          // ["long_call", "bull_call_spread"]
    recommendedTimeframes: string[]     // ["15m", "1h"]
    minConfidenceThreshold: number      // 0.50
    createdAt: ISO8601
    lastUsed: ISO8601
  }

SELECTOR EN UI (barra sobre la gráfica):
  [Estrategia: Full Confluence ▼]  [⚙️ Personalizar]  [💾 Guardar]
  Timeframe: [1m][5m][15m][1h][4h][1D][1W][1M][1Y]
```

---

### 3.1 Servicio de Conexión con Broker (`broker_connector`)

```typescript
PASO 1: Inicializar Conexión al Broker
├─ Detectar broker activo (IBKR o Alpaca según .env)
├─ Intentar conexión con timeout de 30 segundos
├─ Retry automático: máximo 3 intentos, backoff exponencial
├─ Emitir evento: CONNECTION_STATUS (connected/disconnected/error)
└─ Registrar en log: timestamp, broker, status

PASO 2: Autenticación y Validación de Cuenta
├─ IBKR: verificar que TWS/Gateway está activo y autenticado
├─ Alpaca: validar API Key + Secret con endpoint /v2/account
├─ Obtener datos de cuenta: balance, buying_power, positions
└─ Emitir evento: ACCOUNT_LOADED

PASO 3: Mantener Conexión (Heartbeat)
├─ Ping cada 30 segundos
├─ Reconexión automática si se pierde la conexión
└─ Notificar al usuario si la reconexión falla
```

### 3.2 Servicio de Datos de Mercado (`market_data`)

```typescript
PASO 4: Suscribir a Datos en Tiempo Real
├─ Para cada símbolo en watchlist activa:
│  ├─ Suscribir a quotes (Bid/Ask/Last/Volume)
│  ├─ Suscribir a trade ticks (últimas transacciones)
│  └─ Throttle de actualización: máximo 2 updates/segundo por símbolo
├─ Emitir evento: QUOTE_UPDATE(symbol, quote)
└─ Actualizar store global: marketData[symbol]

PASO 5: Obtener Datos Históricos (Velas OHLCV)
├─ Por cada símbolo + timeframe activo:
│  ├─ Solicitar últimas 200 velas históricas al iniciar
│  ├─ Actualizar vela actual en tiempo real (sin re-request completo)
│  └─ Mantener buffer circular de 500 velas por símbolo/timeframe
├─ Validación: verificar que datos no están stale (>5 min sin update)
└─ Emitir evento: CANDLES_UPDATED(symbol, timeframe, candles[])

PASO 6: Obtener Cadena de Opciones
├─ Trigger: cuando usuario selecciona símbolo en detalle
├─ Solicitar expirations disponibles
├─ Filtrar por DTE dentro de rango configurado (7-45 días)
├─ Obtener strikes ATM ±10% para expirations filtradas
├─ Datos por strike: bid, ask, last, volume, OI, IV, delta, gamma, theta, vega
└─ Actualizar cada 60 segundos (no tiempo real para evitar saturación)
```

### 3.3 Core de Indicadores Técnicos (`technical_indicators`)

```typescript
PASO 7: Calcular Indicadores por Símbolo y Timeframe
└─ Para cada símbolo × timeframe cuando llegan nuevas velas:

    RSI(14):
    ├─ Input: close prices[], period=14
    ├─ Output: { value: number, signal: "oversold"|"neutral"|"overbought" }
    └─ Señal BUY si RSI < 30, SELL si RSI > 70

    MACD(12, 26, 9):
    ├─ Input: close prices[], fastPeriod=12, slowPeriod=26, signalPeriod=9
    ├─ Output: { macdLine, signalLine, histogram, crossover: "bullish"|"bearish"|null }
    └─ Señal BUY si crossover bullish (MACD cruza sobre signal line)

    Bollinger Bands(20, 2):
    ├─ Input: close prices[], period=20, stdDev=2
    ├─ Output: { upper, middle, lower, width, percentB, signal: "squeeze"|"breakout"|"normal" }
    └─ Señal BUY si precio toca lower band, SELL si toca upper band

    EMA(9, 21, 50):
    ├─ Input: close prices[]
    ├─ Output: { ema9, ema21, ema50, trend: "bullish"|"bearish"|"sideways" }
    └─ Trend bullish si EMA9 > EMA21 > EMA50

    ATR(14):
    ├─ Input: OHLC candles[], period=14
    ├─ Output: { value: number, volatility: "high"|"medium"|"low" }
    └─ Usado para cálculo dinámico de stop loss sugerido

    Volume Analysis:
    ├─ Input: volume[], close[], period=20
    ├─ Output: { avgVolume, currentVolume, ratio, signal: "high"|"normal"|"low" }
    └─ Señal de confirmación si volumen actual > 1.5× promedio

    Mínimo requerido para calcular: 50 velas
    Si insuficientes: retornar null con flag "insufficient_data"

  SALIDA ESTÁNDAR DEL CORE:
  ├─ side: BUY | SELL | HOLD
  ├─ confidence: number
  ├─ score: number
  ├─ reasons: string[]
  ├─ timeframe: string
  └─ indicatorBreakdown: object
```

  ### 3.4 Core de Estructura Técnica (`technical_structure`)

```typescript
  PASO 8: Detectar Soportes, Resistencias y Tendencias

    SOPORTES Y RESISTENCIAS:
    ├─ Identificar pivots relevantes por timeframe
    ├─ Detectar zonas en lugar de líneas rígidas
    ├─ Calcular fuerza de zona por número de rechazos, volumen y recencia
    └─ Emitir señal BUY cerca de soporte fuerte y SELL cerca de resistencia fuerte

    TENDENCIAS:
    ├─ Calcular bias por timeframe (1m, 5m, 15m, 1h, 4h, 1D)
    ├─ Clasificar como bullish, bearish o sideways
    ├─ Detectar cambios de estructura (BOS / CHoCH si aplica)
    └─ Emitir confirmación o invalidación para BUY/SELL

  SALIDA ESTÁNDAR DEL CORE:
  ├─ side: BUY | SELL | HOLD
  ├─ confidence: number
  ├─ score: number
  ├─ supportResistanceZones: object[]
  ├─ trendBiasByTimeframe: object
  └─ reasons: string[]
  ```

  ### 3.5 Core de Flujo Institucional (`institutional_flow`)

  ```typescript
  PASO 9: Detectar Actividad Institucional Relevante

    ├─ Analizar unusual options activity
    ├─ Analizar cambios abruptos en open interest
    ├─ Analizar bloques y sweeps cuando la fuente lo permita
    ├─ Relacionar la actividad con precio, volumen y expiraciones
    └─ Emitir sesgo institucional BUY/SELL/HOLD

  SALIDA ESTÁNDAR DEL CORE:
  ├─ side: BUY | SELL | HOLD
  ├─ confidence: number
  ├─ score: number
  ├─ institutionalEvents: object[]
  └─ reasons: string[]
  ```

  ### 3.6 Core de Noticias y Eventos (`news_events`)

  ```typescript
  PASO 10: Analizar Noticias, Reportes y Eventos Próximos

    ├─ Ingerir noticias financieras en tiempo real del instrumento
    ├─ Clasificar sentimiento y relevancia
    ├─ Detectar earnings, guidance, dividendos, splits y eventos macro
    ├─ Evaluar riesgo de evento dentro de la ventana configurada
    └─ Emitir sesgo BUY/SELL/HOLD y nivel de riesgo

  SALIDA ESTÁNDAR DEL CORE:
  ├─ side: BUY | SELL | HOLD
  ├─ confidence: number
  ├─ score: number
  ├─ sentiment: bullish | bearish | neutral
  ├─ nextRelevantEvents: object[]
  └─ reasons: string[]
  ```

  ### 3.7 Core Fundamental y Ranking Diario (`fundamentals` + `opportunity_ranking`)

  ```typescript
  PASO 11: Analizar Estado Actual de la Empresa

    FUNDAMENTALES:
    ├─ Revisar revenue growth, EPS, márgenes y guidance
    ├─ Revisar valuación relativa si la fuente lo permite
    ├─ Detectar earnings surprise y revisiones de analistas
    └─ Emitir bias de calidad/contexto para el instrumento

    RANKING DIARIO DE OPORTUNIDADES:
    ├─ Evaluar universo definido por el usuario (watchlist, sectores, ETFs, etc.)
    ├─ Calcular opportunityScore por instrumento
    ├─ Clasificar mejores candidatos BUY, SELL, WATCH y AVOID
    └─ Ordenar de mayor a menor prioridad diaria

  SALIDA ESTÁNDAR DEL RANKING:
  ├─ symbol: string
  ├─ priorityRank: number
  ├─ opportunityScore: number
  ├─ recommendedAction: BUY | SELL | WATCH | AVOID
  ├─ whyNow: string[]
  ├─ whyNot: string[]
  └─ nextRelevantEvent: object | null
  ```

  ### 3.8 Core de Confluencia (`confluence_engine`)

  ```typescript
  PASO 12: Generar Señal Combinada por Símbolo (usando Estrategia Activa)

    ALGORITMO DE CONFLUENCIA (dinámico según estrategia seleccionada):
    ├─ Obtener estrategia activa del settingsStore
    ├─ Filtrar: solo considerar los cores habilitados en la estrategia
    ├─ Tomar la salida estandarizada de cada core activo
    ├─ Calcular scores parciales:
    │   technicalScore
    │   structureScore
    │   institutionalScore
    │   newsScore
    │   eventRiskScore
    │   fundamentalScore
    │   aiScore
    ├─ Combinar por pesos configurados
    ├─ Separar el análisis de BUY y SELL para evitar ambigüedad
    ├─ Detectar coincidencias obligatorias y conflictos entre cores
    ├─ Calcular scoreFinal y confidenceFinal
    └─ Emitir evento: SIGNAL_GENERATED(symbol, signal, confidence, selectedCores, strategyId)

  REGLAS:
  ├─ Si 2 o más cores seleccionados coinciden en BUY, generar BUY candidate
  ├─ Si 2 o más cores seleccionados coinciden en SELL, generar SELL candidate
  ├─ Si hay conflicto fuerte entre cores, clasificar como HOLD o WATCH
  └─ La estrategia puede definir requisitos mínimos por core
  ```

  ### 3.9 Core de Sugerencia IA (`ai_advisor`)

  ```typescript
  PASO 13: Confirmación y Sugerencia por IA (Claude API)

  ├─ Trigger: cuando signal = BUY o SELL con confidence ≥ minConfidenceThreshold
├─ Construir prompt con:
│   - Symbol, timeframe, precio actual
  │   - Estrategia activa y sus parámetros
  │   - Resumen de todos los cores activos con sus valores exactos
  │   - Noticias, eventos y fundamentales relevantes
  │   - Contexto del mercado general (SPY trend)
│   - Parámetros de riesgo configurados
├─ Solicitar a Claude:
│   { confirmed: boolean, confidence_adjustment: number,
│     reasoning: string, risk_notes: string,
  │     suggested_entry: number, suggested_stop: number,
  │     suggested_target: number, preferred_strategy: string[] }
  └─ Combinar: confidence_final = (signal_confidence + ai_confidence) / 2
```

  ### 3.10 Core de Estrategias y Cadena de Opciones (`strategy_engine`)

  ```typescript
  PASO 14: Priorizar Estrategias para la Situación Actual

    ├─ Recibir señal final del core de confluencia
    ├─ Recibir sesgo de IA y contexto de riesgo
    ├─ Analizar cadena de opciones (DTE, IV, delta, liquidez, spread)
    ├─ Evaluar qué estrategia aplica mejor:
    │   long_call, long_put, bull_call_spread, bear_put_spread,
    │   covered_call, protective_put, iron_condor, straddle, strangle
    ├─ Ordenar estrategias de mayor a menor prioridad
    └─ Proponer la mejor implementación posible según el contexto actual

  SALIDA:
  ├─ rankedStrategies: object[]
  ├─ preferredContracts: object[]
  ├─ recommendedExecutionType: CALL | PUT | STOCK | NO_TRADE
  └─ reasons: string[]
  ```

  ### 3.11 Gestión de Órdenes (`order_manager`)

```typescript
  PASO 15: Preparar Orden (cuando trader aprueba señal)
├─ Calcular tamaño de posición:
│   position_size = (account_balance * max_position_size_pct) / entry_price
├─ Aplicar reglas de riesgo:
│   - Verificar que daily_loss no supere límite
│   - Verificar que concurrent_positions < máximo
│   - Verificar que buying_power es suficiente
├─ Construir orden:
│   { symbol, action: BUY/SELL, quantity, orderType: LIMIT/MARKET,
│     limitPrice, stopLoss, takeProfit, timeInForce: DAY,
│     assetType: STOCK | OPTION, strategyId, contractSelection }
└─ Mostrar confirmación al trader (modal de revisión)

PASO 16: Ejecutar Orden (tras confirmación explícita del trader)
├─ Enviar orden al broker activo (IBKR o Alpaca)
├─ Monitorear estado: PENDING → FILLED / PARTIAL / CANCELLED
├─ Registrar en historial local: timestamp, symbol, side, qty, price, P&L
├─ Actualizar posiciones abiertas en store
└─ Emitir alerta: notificación visual + sonido (configurable)

PASO 17: Monitoreo de Posiciones Abiertas
├─ Por cada posición abierta, verificar cada minuto:
│   - Si precio alcanzó stop loss → alertar para cerrar
│   - Si precio alcanzó take profit → alertar para cerrar
│   - Si señal original se revirtió → alertar como advertencia
└─ Emitir evento: POSITION_ALERT(symbol, type, currentPrice, targetPrice)
```

### 3.12 Sistema de Alertas (`alerts`)

```typescript
PASO 18: Gestionar Alertas
├─ Tipos de alerta:
│   SIGNAL_HIGH_CONFIDENCE  → popup + sonido + log (confidence ≥ 75%)
│   SIGNAL_MEDIUM           → badge en UI + log (confidence 50-74%)
│   DAILY_OPPORTUNITY       → ranking diario actualizado
│   EVENT_RISK              → earnings / macro evento cercano
│   POSITION_STOP_LOSS      → popup urgente + sonido
│   POSITION_TAKE_PROFIT    → popup + sonido
│   CONNECTION_LOST         → banner persistente
│   AI_CONFIRMATION         → integrado en tarjeta de señal
├─ Canales de entrega: UI (siempre) + Email (configurable)
└─ Email de notificaciones: ojosdragon@gmail.com (configurable en .env)
    Variable: ALERT_EMAIL=ojosdragon@gmail.com
```

---

## 4. Salida (Output)

### 4.1 Dashboard Principal (UI)

```
📊 LAYOUT PRINCIPAL (Dark Theme — Inspirado en plataformas profesionales)

┌──────────────────────────────────────────────────────────────────────────────┐
│ HEADER                                                                        │
│ 🔴 Logo  │  Market: OPEN ● NYSE 09:32  │  Balance: $54,320  │  IBKR ● LIVE  │
├─────────────────────────┬────────────────────────────────────────────────────┤
│  WATCHLIST / SCANNER    │  TOOLBAR                                            │
│  [🔍 Buscar símbolo]    │  [Estrategia: Full Confluence ▼] [⚙️] [💾]         │
│  ─────────────────────  │  TF: [1m][5m][15m✓][1h][4h][1D][1W][1M][1Y]       │
│  TOP BUY #1 NVDA  91   │ ─────────────────────────────────────────────────  │
│  TOP BUY #2 META  87   │  CHART PANEL (TradingView Lightweight Charts)       │
│  TOP SELL #1 TSLA 84   │                                                     │
│  WATCH     AAPL   73   │                                                     │
│  SPY   $589.40  ↑2.1%  │                                                     │
│  QQQ   $478.22  ↑1.8%  │                                                     │
│  AAPL  $224.10  ↓0.3%  │  ┌──────────────────────────────────────────────┐  │
│  TSLA  $312.55  ↑4.2%  │  │  Velas OHLCV + Bollinger Bands overlay       │  │
│  NVDA  $875.20  ↑3.1%  │  │  + EMA 9 / 21 / 50 overlay                  │  │
│  MSFT  $412.80  ↑0.9%  │  │                                              │  │
│  AMZN  $198.40  ↑1.2%  │  │  [Marcadores de señales sobre velas:        │  │
│  META  $565.30  ↑2.7%  │  │   🔺 BUY  🔻 SELL en el punto exacto]      │  │
│  ─────────────────────  │  ├──────────────────────────────────────────────┤  │
│  [+ Agregar símbolo]    │  │  Sub-panel 1: MACD (línea + señal + histo.) │  │
│                         │  ├──────────────────────────────────────────────┤  │
│                         │  │  Sub-panel 2: RSI (línea + zonas 30/70)     │  │
│                         │  ├──────────────────────────────────────────────┤  │
│                         │  │  Sub-panel 3: Volume (barras + avg line)    │  │
│                         │  └──────────────────────────────────────────────┘  │
├─────────────────────────┴────────────────────────────────────────────────────┤
│  📋 TABLA DE OPORTUNIDADES Y SEÑALES — ANÁLISIS ULTRA-DETALLADO               │
│  [Filtro: Todas ▼]  [Solo BUY]  [Solo SELL]  [Alta confianza ≥75%]           │
│  [Estrategia: Full Confluence]  [TF: 15m]  [🔄 Auto-refresh ON]              │
│  [Cores: Tech][Structure][Institutional][News][Fundamental][IA]             │
│                                                                                │
│ ┌──────┬──────┬───────┬───────┬─────────────────────────────────────────────┐│
│ │Símb. │Señal │Conf.% │TF     │  DESGLOSE DE INDICADORES Y RAZONES          ││
│ ├──────┼──────┼───────┼───────┼─────────────────────────────────────────────┤│
│ │ SPY  │🟢BUY │ 82%   │ 15m   │ ✅RSI 28.4 — Oversold (umbral <30)          ││
│ │      │      │       │       │ ✅MACD — Crossover alcista (MACD cruzó ↑    ││
│ │      │      │       │       │    sobre signal line hace 2 velas)          ││
│ │      │      │       │       │ ✅Bollinger — Precio toca lower band ($587) ││
│ │      │      │       │       │ ✅EMA Trend — Bullish: EMA9>EMA21>EMA50     ││
│ │      │      │       │       │ ✅Volume — 1.8× promedio (confirma fuerza)  ││
│ │      │      │       │       │ 🤖IA: "Señal sólida. Soporte en EMA50.      ││
│ │      │      │       │       │    Riesgo: reporte Fed mañana."             ││
│ │      │      │       │       │ 📈Entrada: $589.40 | Stop: $580.50 | T: $607││
│ │      │      │       │       │ 📅Señal anterior: BUY 2026-02-28 (+2.3%)   ││
│ │      │      │       │ [▼Historial SPY] [⚡Ejecutar] [✕Descartar]         ││
│ ├──────┼──────┼───────┼───────┼─────────────────────────────────────────────┤│
│ │ AAPL │🔴SELL│ 71%   │ 15m   │ ✅RSI 73.2 — Overbought (umbral >70)        ││
│ │      │      │       │       │ ✅MACD — Crossover bajista (MACD cruzó ↓)  ││
│ │      │      │       │       │ ✅Bollinger — Precio toca upper band ($226) ││
│ │      │      │       │       │ ❌EMA Trend — Neutral (no confirmado)       ││
│ │      │      │       │       │ ✅Volume — 1.5× promedio (confirma señal)   ││
│ │      │      │       │       │ 🤖IA: "Sobrecompra técnica confirmada.      ││
│ │      │      │       │       │    Cuidado: earnings en 3 días."            ││
│ │      │      │       │       │ 📈Entrada: $224.10 | Stop: $227.50 | T: $217││
│ │      │      │       │       │ 📅Sin señal previa esta semana             ││
│ │      │      │       │ [▼Historial AAPL] [⚡Ejecutar] [✕Descartar]        ││
│ ├──────┼──────┼───────┼───────┼─────────────────────────────────────────────┤│
│ │ QQQ  │⚪HOLD│ 38%   │ 15m   │ ❌RSI 51.3 — Neutral (zona 30-70)           ││
│ │      │      │       │       │ ❌MACD — Sin crossover activo               ││
│ │      │      │       │       │ ❌Bollinger — Precio en zona media          ││
│ │      │      │       │       │ ✅EMA Trend — Bullish (confirmación parcial)││
│ │      │      │       │       │ ❌Volume — Normal (0.9× promedio)           ││
│ │      │      │       │       │ 📊Confluencia insuficiente: 1/5 indicadores ││
│ └──────┴──────┴───────┴───────┴─────────────────────────────────────────────┘│
│                                                                                │
│  SUB-PANEL: HISTORIAL DE SEÑALES DEL SÍMBOLO SELECCIONADO (expandible)        │
│  SPY — Últimas 10 señales:                                                    │
│  ┌──────────────┬──────┬───────┬────────┬───────┬────────────────────────┐   │
│  │ Fecha/Hora   │Señal │ Conf. │TF      │Resultado│ Razones clave        │   │
│  ├──────────────┼──────┼───────┼────────┼─────────┼────────────────────────┤  │
│  │2026-02-28 14:32│🟢BUY│ 79% │15m     │+2.3% ✅ │RSI+MACD+BB alineados │   │
│  │2026-02-25 10:15│🔴SELL│68% │15m     │+1.1% ✅ │RSI overbought+MACD↓  │   │
│  │2026-02-20 15:47│🟢BUY│ 55% │15m     │-0.8% ❌ │RSI+MACD (sin Volume) │   │
│  │ ...           │      │      │        │         │                      │   │
│  └──────────────┴──────┴───────┴────────┴─────────┴────────────────────────┘  │
├────────────────────────────────────────────────────────────────────────────────┤
│  PANEL INFERIOR: POSICIONES ABIERTAS                                           │
│  SPY 5 shares @ $589.40 | P&L: +$14.20 (+0.48%) | Stop: $580.50 | T: $607.00 │
└────────────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Estructura de Datos de la Tabla de Señales (Ultra)

```typescript
interface CoreSignalResult {
  coreId: string
  symbol: string
  timeframe: string
  side: "BUY" | "SELL" | "HOLD"
  confidence: number
  score: number
  reasons: string[]
  risks: string[]
  timestamp: ISO8601
}

interface OpportunityRankRow {
  symbol: string
  priorityRank: number
  opportunityScore: number
  recommendedAction: "BUY" | "SELL" | "WATCH" | "AVOID"
  whyNow: string[]
  whyNot: string[]
  nextRelevantEvent: string | null
}

// Estructura completa de cada fila de la tabla de señales enriquecida
interface SignalTableRow {
  // ── IDENTIFICACIÓN ─────────────────────────────────────────────
  id:           string           // UUID de la señal
  symbol:       string           // "SPY"
  timestamp:    ISO8601          // Cuándo se generó la señal
  timeframe:    string           // "15m"
  strategyId:   string           // ID de la estrategia usada
  strategyName: string           // "Full Confluence"

  // ── CLASIFICACIÓN DE SEÑAL ─────────────────────────────────────
  signal:       "BUY" | "SELL" | "HOLD"
  confidence:   number           // 0-100 %
  score:        number           // score numérico crudo
  scoreMax:     number           // score máximo posible con esta estrategia

  // ── PRECIO AL MOMENTO DE LA SEÑAL ──────────────────────────────
  priceAtSignal: number
  bid:           number
  ask:           number
  volume:        number

  // ── DESGLOSE POR CORE (el corazón de la tabla) ─────────────────
  selectedCores: string[]
  coreResults: CoreSignalResult[]

  // ── DESGLOSE POR INDICADOR (cuando aplica) ─────────────────────
  indicators: {
    rsi: {
      active:     boolean        // ¿estaba activo en la estrategia?
      value:      number         // 28.4
      threshold:  { oversold: 30, overbought: 70 }
      signal:     "oversold" | "neutral" | "overbought"
      aligned:    boolean        // ¿contribuye a la señal principal?
      score:      number         // puntos aportados (+1 / 0 / -1 × peso)
      reason:     string         // "RSI 28.4 — Por debajo del umbral de sobreventa (30)"
    }
    macd: {
      active:     boolean
      macdLine:   number         // 1.23
      signalLine: number         // 0.98
      histogram:  number         // 0.25
      crossover:  "bullish" | "bearish" | null
      candlesAgo: number         // crossover ocurrió hace N velas
      aligned:    boolean
      score:      number
      reason:     string         // "MACD cruzó al alza sobre la línea de señal hace 2 velas"
    }
    bollinger: {
      active:     boolean
      upper:      number         // 601.20
      middle:     number         // 589.40
      lower:      number         // 577.60
      bandwidth:  number         // % de amplitud
      percentB:   number         // 0.03 (precio cerca del lower)
      touch:      "upper" | "lower" | "middle" | "none"
      aligned:    boolean
      score:      number
      reason:     string         // "Precio ($587.10) tocando la banda inferior ($577.60)"
    }
    ema: {
      active:     boolean
      ema9:       number
      ema21:      number
      ema50:      number
      trend:      "bullish" | "bearish" | "sideways"
      aligned:    boolean
      score:      number
      reason:     string         // "EMA9 (591) > EMA21 (588) > EMA50 (582) — Tendencia alcista"
    }
    atr: {
      active:     boolean
      value:      number         // 3.20
      volatility: "high" | "medium" | "low"
      suggestedStop: number      // precio sugerido de stop loss basado en ATR
      reason:     string         // "ATR 3.20 → stop sugerido a 2×ATR = $6.40 bajo entrada"
    }
    volume: {
      active:        boolean
      current:       number
      average20:     number
      ratio:         number      // 1.8 (1.8× el promedio)
      signal:        "high" | "normal" | "low"
      aligned:       boolean
      score:         number
      reason:        string      // "Volumen actual (2.1M) es 1.8× el promedio de 20 períodos (1.17M)"
    }
  }

  technicalStructure?: {
    supportZones: object[]
    resistanceZones: object[]
    trendBias: object
  }

  institutionalFlow?: {
    events: object[]
  }

  newsEvents?: {
    sentiment: "bullish" | "bearish" | "neutral"
    catalysts: object[]
    eventRiskLevel: "low" | "medium" | "high"
  }

  fundamentals?: {
    earnings: object
    growth: object
    valuation: object
  }

  // ── CONFIRMACIÓN IA ────────────────────────────────────────────
  aiConfirmation: {
    confirmed:           boolean
    confidenceAdjustment: number  // ajuste al % de confianza (-10 a +10)
    reasoning:           string   // texto completo del análisis de Claude
    riskNotes:           string   // advertencias específicas
    availableAt:         ISO8601 | null
    error:               string | null  // si falló la API
  }

  // ── PARÁMETROS SUGERIDOS ───────────────────────────────────────
  suggested: {
    entry:         number        // $589.40
    stopLoss:      number        // $580.50
    takeProfit:    number        // $607.00
    riskReward:    number        // 2.0
    stopLossPct:   number        // -1.5%
    takeProfitPct: number        // +3.0%
    positionSize:  number        // 5 (shares calculadas por riesgo)
    capitalRequired: number      // $2,947.00
  }

  // ── OPCIONES RELACIONADAS (si aplica) ─────────────────────────
  strategyRanking?: {
    id: string
    name: string
    priority: number
    score: number
    reason: string
  }[]

  relatedOptions?: {
    type:        "CALL" | "PUT"
    strike:      number
    expiration:  string
    premium:     number
    iv:          number          // Implied Volatility %
    delta:       number
    suggestion:  string          // "SPY 590 CALL — bajo IV (18%), dentro del DTE preferido"
  }[]

  // ── HISTORIAL DEL SÍMBOLO ─────────────────────────────────────
  symbolHistory: {
    totalSignals:    number       // total de señales generadas para este símbolo
    winRate:         number       // % de señales que resultaron ganadoras
    avgReturn:       number       // retorno promedio por señal
    lastSignals: {
      timestamp:    ISO8601
      signal:       "BUY" | "SELL"
      confidence:   number
      timeframe:    string
      result:       number | null  // % de retorno (null si aún abierta o sin seguimiento)
      resultLabel:  "win" | "loss" | "open" | "unknown"
    }[]                           // últimas 10 señales
  }

  // ── ESTADO EN LA TABLA ────────────────────────────────────────
  status:     "active" | "executed" | "dismissed" | "expired"
  expiresAt:  ISO8601              // señal expira si no se actúa en N horas
}
```

### 4.3 Modal de Señal Detallada (al hacer click en una fila)

```
┌──────────────────────────────────────────────────────────────────────┐
│ 🟢 BUY SIGNAL — SPY · 15m · Full Confluence            Conf: 82%    │
│ Generada: 2026-03-04 09:47:23 EST    Expira en: 47 min              │
├──────────────────────────────────────────────────────────────────────┤
│ DESGLOSE DE INDICADORES                                               │
│                                                                       │
│ ✅ RSI(14): 28.4                                                      │
│    Por debajo del umbral de sobreventa (30). Históricamente,         │
│    este nivel en SPY ha precedido rebotes de +1.5% en promedio.      │
│                                                                       │
│ ✅ MACD(12,26,9): Crossover alcista                                   │
│    La línea MACD (1.23) cruzó sobre la línea de señal (0.98)        │
│    hace 2 velas. Histograma positivo y en expansión (+0.25).         │
│                                                                       │
│ ✅ Bollinger Bands(20,2): Precio en lower band                        │
│    Precio actual ($587.10) tocando la banda inferior ($577.60).      │
│    %B = 0.03 (precio en percentil 3 del canal). Squeeze previo.     │
│                                                                       │
│ ✅ EMA Trend: Alcista (EMA9 > EMA21 > EMA50)                         │
│    EMA9: $591.20 · EMA21: $588.40 · EMA50: $582.10                  │
│    Estructura de tendencia alcista intacta.                          │
│                                                                       │
│ ✅ Volume: 1.8× promedio (2.1M vs avg 1.17M)                         │
│    Volumen significativamente por encima del promedio de 20 velas.   │
│    Confirma presión compradora detrás del movimiento.                │
├──────────────────────────────────────────────────────────────────────┤
│ 🤖 CONFIRMACIÓN IA (Claude)                           ✅ Confirmada  │
│                                                                       │
│ "Señal técnicamente sólida con 5/5 indicadores alineados en 15m.    │
│  El RSI en zona de sobreventa combinado con crossover MACD bullish   │
│  es una de las confluencias más confiables históricamente en SPY.   │
│  EMA50 actúa como soporte dinámico en $582. El volumen elevado      │
│  sugiere participación institucional en este nivel.                  │
│                                                                       │
│  ⚠️ Riesgo: Reporte de empleo (NFP) mañana 8:30 AM EST. Considerar  │
│  reducir tamaño de posición al 50% o usar opciones para limitar     │
│  exposición al evento macro."                                        │
│                                         Ajuste confianza: +5% → 87%│
├──────────────────────────────────────────────────────────────────────┤
│ PARÁMETROS DE OPERACIÓN SUGERIDOS                                    │
│                                                                       │
│  Entrada:      $589.40     Stop Loss:   $580.50  (-1.5% · 2×ATR)   │
│  Target:       $607.00     R/R Ratio:   2.0 : 1                     │
│  Posición:     5 shares    Capital:     $2,947.00  (5% portafolio)  │
│                                                                       │
│  Tipo orden:  [LIMIT ▼]    Precio límite: [$589.40]                 │
│  TIF:         [DAY  ▼]                                               │
├──────────────────────────────────────────────────────────────────────┤
│ OPCIONES RELACIONADAS                                                 │
│  SPY 590 CALL · Exp 21-Mar (17 DTE) · Prima: $3.20 · IV: 18% (bajo)│
│  SPY 585 CALL · Exp 21-Mar (17 DTE) · Prima: $5.10 · IV: 19%       │
│  Delta: 0.52 · Gamma: 0.08 · Theta: -0.18 · Vega: 0.31            │
├──────────────────────────────────────────────────────────────────────┤
│ HISTORIAL SPY (últimas 5 señales con esta estrategia)               │
│  2026-02-28 14:32 🟢BUY 79%  → +2.3% ✅ (cerrada)                  │
│  2026-02-25 10:15 🔴SELL 68% → +1.1% ✅ (cerrada)                  │
│  2026-02-20 15:47 🟢BUY 55%  → -0.8% ❌ (cerrada)                  │
│  Win Rate estrategia en SPY: 67% (8/12 señales) · Avg: +1.4%       │
├──────────────────────────────────────────────────────────────────────┤
│           [✕ DESCARTAR]    [📋 COPIAR]    [⚡ EJECUTAR ORDEN]       │
└──────────────────────────────────────────────────────────────────────┘
```

### 4.4 Logs y Persistencia

```
📁 localStorage (browser):
├─ watchlist.json          [Watchlist del usuario]
├─ settings.json           [Configuración de indicadores y riesgo]
├─ strategies.json         [Estrategias personalizadas guardadas (max 20)]
├─ active_strategy.json    [ID de la estrategia actualmente seleccionada]
├─ trade_history.json      [Historial de operaciones ejecutadas]
└─ signal_log.json         [Log de señales generadas (últimas 500)]

📁 Variables de entorno (.env):
├─ IBKR_HOST, IBKR_PORT, IBKR_CLIENT_ID, IBKR_ACCOUNT_ID
├─ ALPACA_API_KEY, ALPACA_SECRET_KEY, ALPACA_BASE_URL
├─ VITE_CLAUDE_API_KEY    (Claude API para confirmación de señales)
├─ ALERT_EMAIL            (Email para notificaciones críticas)
└─ ACTIVE_BROKER          (ibkr | alpaca)
```

### 4.5 Historial de Operaciones

```typescript
// Estructura de cada trade registrado
{
  id: string,              // UUID
  timestamp: ISO8601,      // Fecha/hora de ejecución
  symbol: string,          // "SPY"
  side: "BUY" | "SELL",
  quantity: number,
  entryPrice: number,
  exitPrice: number | null, // null si posición abierta
  stopLoss: number,
  takeProfit: number,
  status: "OPEN" | "CLOSED" | "CANCELLED",
  pnl: number | null,      // P&L realizado (si cerrada)
  pnlPct: number | null,
  signalConfidence: number, // % de confianza de la señal original
  aiReasoning: string,     // Razonamiento de Claude
  indicators: { rsi, macd, bb, ema, volume }, // Snapshot al momento de la señal
  broker: "ibkr" | "alpaca",
  orderId: string          // ID de orden en el broker
}
```

---

## 5. Requisitos Técnicos

### 5.1 Stack Tecnológico

```yaml
Frontend Framework:   React 18 + TypeScript 5 + Vite 5
UI Styling:           TailwindCSS 3 (dark theme por defecto)
Estado Global:        Zustand 4
Gráficas Financieras: TradingView Lightweight Charts v4
Componentes UI:       shadcn/ui (base de componentes)
Iconos:               lucide-react
HTTP Client:          axios
WebSocket:            native browser WebSocket API
Validación:           zod (schemas de datos)

Brokers:
  Primario:    @stoqey/ib (Interactive Brokers TWS API)
  Secundario:  @alpacahq/alpaca-trade-api (paper trading)

Indicadores Técnicos:
  Librería:    technicalindicators (npm)
  Alternativa: tulind (bindings C, más rápido para cálculos masivos)

IA / Análisis:
  Claude API:  @anthropic-ai/sdk
  Model:       claude-sonnet-4-20250514

Notificaciones Email:
  Librería:    nodemailer (vía backend proxy) | EmailJS (directo cliente)

Testing:
  Unit:        Vitest + Testing Library
  E2E:         Playwright
  Benchmark:   Comparación vs TradingView Pine Script

Linting/Format: ESLint + Prettier
```

### 5.2 Variables de Entorno (.env)

```bash
# ── BROKER: Interactive Brokers ───────────────────────────────
IBKR_HOST=127.0.0.1
IBKR_PORT=7497
IBKR_CLIENT_ID=1
IBKR_ACCOUNT_ID=<tu_cuenta_ibkr>

# ── BROKER: Alpaca (Paper Trading / Desarrollo) ───────────────
ALPACA_API_KEY=<key>
ALPACA_SECRET_KEY=<secret>
ALPACA_BASE_URL=https://paper-api.alpaca.markets
ALPACA_DATA_URL=https://data.alpaca.markets

# ── IA: Claude API ────────────────────────────────────────────
VITE_CLAUDE_API_KEY=<anthropic_api_key>
VITE_CLAUDE_MODEL=claude-sonnet-4-20250514

# ── ALERTAS ───────────────────────────────────────────────────
ALERT_EMAIL=ojosdragon@gmail.com
EMAILJS_SERVICE_ID=<service_id>
EMAILJS_TEMPLATE_ID=<template_id>
EMAILJS_PUBLIC_KEY=<public_key>

# ── CONFIGURACIÓN GENERAL ─────────────────────────────────────
ACTIVE_BROKER=alpaca                # ibkr | alpaca
VITE_APP_ENV=development            # development | production
VITE_APP_VERSION=1.0.0
```

### 5.3 Versiones de Dependencias

```json
{
  "dependencies": {
    "react": "^18.3.0",
    "react-dom": "^18.3.0",
    "typescript": "^5.4.0",
    "vite": "^5.2.0",
    "tailwindcss": "^3.4.0",
    "zustand": "^4.5.0",
    "lightweight-charts": "^4.1.0",
    "technicalindicators": "^3.1.0",
    "@anthropic-ai/sdk": "^0.20.0",
    "@stoqey/ib": "^2.0.0",
    "@alpacahq/alpaca-trade-api": "^3.1.0",
    "axios": "^1.6.0",
    "zod": "^3.22.0",
    "lucide-react": "^0.263.0",
    "emailjs-com": "^3.2.0"
  },
  "devDependencies": {
    "vitest": "^1.4.0",
    "@testing-library/react": "^14.2.0",
    "playwright": "^1.43.0",
    "eslint": "^8.57.0",
    "prettier": "^3.2.0"
  }
}
```

### 5.4 Estructura del Proyecto (SRC-First)

```
projects/pwa/pwa_inversions_drfic/
├── public/
│   └── favicon.ico
├── src/
│   ├── ai_work_flow/                    # Metodología AI Skill Development
│   │   ├── development/
│   │   │   ├── workflow_agents.yaml
│   │   │   └── README.md
│   │   ├── docs/
│   │   │   └── specs/
│   │   │       ├── SPECIFICATION.md     ← Este documento
│   │   │       └── incremental/
│   │   ├── knowledge/
│   │   │   ├── remote/
│   │   │   └── local/
│   │   └── tickets/
│   │       ├── TKT-INVRFIC-001.md
│   │       └── ...
│   │
│   ├── assets/
│   │   ├── icons/
│   │   └── images/
│   │
│   ├── components/
│   │   └── ui/
│   │       ├── Badge.tsx
│   │       ├── Button.tsx
│   │       ├── Card.tsx
│   │       ├── Modal.tsx
│   │       ├── Tooltip.tsx
│   │       └── index.ts
│   │
│   ├── features/
│   │   ├── dashboard/
│   │   │   ├── components/
│   │   │   │   ├── DashboardLayout.tsx
│   │   │   │   ├── MarketStatusBar.tsx
│   │   │   │   └── AccountSummary.tsx
│   │   │   ├── hooks/
│   │   │   └── index.ts
│   │   │
│   │   ├── watchlist/
│   │   │   ├── components/
│   │   │   │   ├── WatchlistPanel.tsx
│   │   │   │   ├── WatchlistRow.tsx
│   │   │   │   └── AddSymbolModal.tsx
│   │   │   ├── hooks/
│   │   │   │   └── useWatchlist.ts
│   │   │   └── index.ts
│   │   │
│   │   ├── strategy/
│   │   │   ├── components/
│   │   │   │   ├── StrategySelector.tsx      (dropdown en toolbar)
│   │   │   │   ├── StrategyBuilder.tsx       (modal personalización)
│   │   │   │   ├── PresetStrategyCard.tsx
│   │   │   │   └── SaveStrategyModal.tsx
│   │   │   ├── hooks/
│   │   │   │   └── useStrategy.ts
│   │   │   └── index.ts
│   │   │
│   │   │   ├── components/
│   │   │   │   ├── TradingChart.tsx       (TradingView Lightweight Charts)
│   │   │   │   ├── IndicatorPanel.tsx
│   │   │   │   └── TimeframeSelector.tsx
│   │   │   ├── hooks/
│   │   │   │   └── useTradingChart.ts
│   │   │   └── index.ts
│   │   │
│   │   ├── signals/
│   │   │   ├── components/
│   │   │   │   ├── SignalsTable.tsx            (tabla ultra-detallada)
│   │   │   │   ├── SignalRow.tsx               (fila expandible con desglose)
│   │   │   │   ├── IndicatorBreakdown.tsx      (✅/❌ por indicador con razón)
│   │   │   │   ├── SignalDetailModal.tsx        (modal de aprobación completo)
│   │   │   │   ├── SignalHistoryPanel.tsx       (historial del símbolo)
│   │   │   │   └── SignalFilters.tsx            (filtros: tipo, confianza, TF)
│   │   │   ├── hooks/
│   │   │   │   └── useSignals.ts
│   │   │   └── index.ts
│   │   │
│   │   ├── options-chain/
│   │   │   ├── components/
│   │   │   │   ├── OptionsChainTable.tsx
│   │   │   │   └── GreeksDisplay.tsx
│   │   │   ├── hooks/
│   │   │   │   └── useOptionsChain.ts
│   │   │   └── index.ts
│   │   │
│   │   ├── positions/
│   │   │   ├── components/
│   │   │   │   ├── PositionsPanel.tsx
│   │   │   │   └── PositionRow.tsx
│   │   │   ├── hooks/
│   │   │   │   └── usePositions.ts
│   │   │   └── index.ts
│   │   │
│   │   ├── trade-history/
│   │   │   ├── components/
│   │   │   │   └── TradeHistoryTable.tsx
│   │   │   └── index.ts
│   │   │
│   │   └── settings/
│   │       ├── components/
│   │       │   ├── SettingsPage.tsx
│   │       │   ├── WatchlistConfig.tsx
│   │       │   ├── SignalConfig.tsx
│   │       │   ├── RiskConfig.tsx
│   │       │   └── BrokerConfig.tsx
│   │       └── index.ts
│   │
│   ├── hooks/
│   │   ├── useBrokerConnection.ts
│   │   ├── useMarketData.ts
│   │   └── useAlerts.ts
│   │
│   ├── services/
│   │   ├── broker/
│   │   │   ├── ibkr.connector.ts
│   │   │   ├── alpaca.connector.ts
│   │   │   └── broker.interface.ts      (interface común IBroker)
│   │   ├── market-data/
│   │   │   ├── marketData.service.ts
│   │   │   └── candles.service.ts
│   │   ├── indicators/
│   │   │   ├── rsi.service.ts
│   │   │   ├── macd.service.ts
│   │   │   ├── bollinger.service.ts
│   │   │   ├── ema.service.ts
│   │   │   ├── atr.service.ts
│   │   │   ├── volume.service.ts
│   │   │   └── indicators.service.ts
│   │   ├── structure/
│   │   │   ├── supportResistance.service.ts
│   │   │   ├── trendBias.service.ts
│   │   │   └── technicalStructure.service.ts
│   │   ├── institutional/
│   │   │   ├── unusualOptions.service.ts
│   │   │   ├── openInterest.service.ts
│   │   │   └── institutionalFlow.service.ts
│   │   ├── news/
│   │   │   ├── newsFeed.service.ts
│   │   │   ├── eventsCalendar.service.ts
│   │   │   └── newsEvents.service.ts
│   │   ├── fundamentals/
│   │   │   ├── earnings.service.ts
│   │   │   ├── valuation.service.ts
│   │   │   ├── companySnapshot.service.ts
│   │   │   └── fundamentals.service.ts
│   │   ├── opportunities/
│   │   │   └── opportunityRanking.service.ts
│   │   ├── confluence/
│   │   │   └── confluenceEngine.service.ts
│   │   ├── ai/
│   │   │   └── aiAdvisor.service.ts
│   │   ├── strategies/
│   │   │   ├── strategyEngine.service.ts
│   │   │   └── optionsStrategySelector.service.ts
│   │   ├── orders/
│   │   │   └── orderManager.service.ts
│   │   └── alerts/
│   │       └── alerts.service.ts
│   │
│   ├── store/
│   │   ├── brokerStore.ts        (conexión, cuenta, estado broker)
│   │   ├── marketDataStore.ts    (quotes, candles por símbolo/timeframe)
│   │   ├── opportunityStore.ts   (ranking diario, top movers, watchlist priorizada)
│   │   ├── signalsStore.ts       (señales activas, historial últimas 500)
│   │   ├── strategyStore.ts      (estrategia activa, presets, personalizadas)
│   │   ├── positionsStore.ts     (posiciones abiertas, historial)
│   │   └── settingsStore.ts      (configuración del usuario)
│   │
│   ├── types/
│   │   ├── broker.types.ts       (Account, Order, Position)
│   │   ├── market.types.ts       (Quote, Candle, OHLCV, Timeframe)
│   │   ├── indicators.types.ts   (RSIResult, MACDResult, BBResult, etc.)
│   │   ├── core.types.ts         (CoreSignalResult, OpportunityRankRow)
│   │   ├── signal.types.ts       (Signal, SignalTableRow, IndicatorBreakdown)
│   │   ├── strategy.types.ts     (Strategy, StrategyPreset, IndicatorConfig)
│   │   ├── fundamentals.types.ts (EarningsSnapshot, ValuationSummary)
│   │   ├── news.types.ts         (NewsItem, EventRisk, Catalyst)
│   │   └── options.types.ts      (OptionChain, Greeks, Strike)
│   │
│   ├── utils/
│   │   ├── formatters.ts         (formatPrice, formatPct, formatDate)
│   │   ├── riskCalculator.ts     (positionSize, stopLoss, R/R)
│   │   └── candleUtils.ts        (normalizar datos OHLCV)
│   │
│   ├── styles/
│   │   └── globals.css           (TailwindCSS + dark theme vars)
│   │
│   ├── App.tsx
│   ├── main.tsx
│   └── vite-env.d.ts
│
├── tests/
│   ├── unit/
│   │   ├── indicators/
│   │   │   ├── rsi.test.ts
│   │   │   ├── macd.test.ts
│   │   │   └── bollinger.test.ts
│   │   └── signals/
│   │       └── signalDetector.test.ts
│   └── e2e/
│       └── dashboard.spec.ts
│
├── .env.example
├── .env
├── index.html
├── package.json
├── tsconfig.json
├── vite.config.ts
├── tailwind.config.ts
└── vitest.config.ts
```

---

## 6. Skills y Agentes

### 6.1 Skills Necesarios

```
🎯 SKILLS GLOBALES (Reutilizables en otros proyectos):
├─ broker_connector          Conexión a brokers financieros (IBKR, Alpaca)
├─ market_data_feed          Streaming de datos OHLCV en tiempo real (9 timeframes)
├─ technical_indicators      Cálculo de RSI, MACD, BB, EMA, ATR, Volume
├─ technical_structure       Soportes, resistencias, tendencias y cambios de estructura
├─ institutional_flow        Detección de actividad institucional y unusual flow
├─ news_events_analyzer      Noticias en tiempo real y eventos corporativos/macro
├─ fundamentals_analyzer     Snapshot financiero, earnings, guidance y valuación
├─ opportunity_ranking       Ranking diario de empresas/instrumentos prioritarios
├─ confluence_engine         Motor de coincidencias entre cores activos
├─ ai_market_analyzer        Confirmación de señales con Claude API
├─ options_chain_reader      Lectura y parsing de cadena de opciones
└─ strategy_recommender      Priorización de estrategias sobre opciones

🎯 SKILLS LOCALES (Específicos de pwa_inversions_drfic):
├─ strategy_manager          Sistema de presets + personalización + guardado
├─ opportunity_scanner_ui    Scanner diario de oportunidades
├─ signal_table_builder      Tabla ultra-detallada con desglose por indicador
├─ risk_calculator           Cálculo de tamaño de posición y R/R
├─ order_builder             Construcción de órdenes con parámetros de riesgo
└─ trade_logger              Registro y persistencia del historial de operaciones
```

### 6.2 Agentes de Desarrollo

```
🤖 fic_picoro_agent_orchestrator
   FASE 2.3: Investiga APIs de brokers, fuentes de noticias, fundamentales,
       institutional flow, TradingView Lightweight Charts, Claude API
   FASE 2.4: Diseña arquitectura por cores, ranking diario y workflow_agents.yaml

👨‍💻 fic_goku_agent_dev1
   FASE 3: Implementa los cores, ranking diario, motor de estrategias,
     integración Claude API, servicios de broker y UI/UX completa

🥷 fic_vegeta_agent_dev2
   FASE 3: Optimiza latencia de feeds, escaneo diario, confluencia,
     audita seguridad de API keys y performance en streams de datos

🧪 fic_bulma_agent_tester1
   FASE 3: Valida cálculos de indicadores vs TradingView, precisión de ranking,
     tests de señales, noticias, eventos y estrategias con datos conocidos
```

---

## 7. Casos de Prueba (Aceptación)

### 7.1 Caso Feliz — Señal BUY de Alta Confianza

**Escenario**: SPY en zona de sobreventa con múltiples indicadores alineados

- [ ] DADO: RSI < 30, MACD crossover bullish, precio en lower Bollinger, EMA trend bullish, volumen alto
- [ ] CUANDO: El motor de señales procesa los indicadores
- [ ] ENTONCES:
  - ✅ Score de confluencia ≥ +2.0
  - ✅ Confidence ≥ 75%
  - ✅ Claude API confirma señal con reasoning
  - ✅ Señal aparece en SignalsPanel con badge 🟢 BUY
  - ✅ Modal de detalle muestra todos los indicadores y parámetros sugeridos
  - ✅ Al aprobar, orden se envía al broker y aparece en Positions

### 7.2 Caso — Señal Rechazada por Baja Confianza

**Escenario**: Solo 1 indicador alineado

- [ ] DADO: Solo RSI < 30, el resto neutral
- [ ] CUANDO: El motor procesa los indicadores
- [ ] ENTONCES:
  - ✅ Score < +2.0
  - ✅ Señal clasificada como HOLD
  - ✅ No se genera alerta
  - ✅ Estado en watchlist: ⚪ HOLD

### 7.3 Caso — Broker Desconectado

**Escenario**: TWS de IBKR no está activo

- [ ] DADO: IB Gateway no está corriendo
- [ ] CUANDO: La app intenta conectar al iniciar
- [ ] ENTONCES:
  - ✅ Retry automático 3 veces con backoff
  - ✅ Banner de error persistente: "Broker desconectado"
  - ✅ Opción de cambiar a Alpaca (paper trading)
  - ✅ Dashboard sigue funcionando en modo lectura (sin datos live)

### 7.4 Caso — Sin Datos Suficientes para Indicadores

**Escenario**: Símbolo recién agregado al watchlist, < 50 velas disponibles

- [ ] DADO: Símbolo con menos de 50 velas históricas cargadas
- [ ] CUANDO: Motor de indicadores intenta calcular
- [ ] ENTONCES:
  - ✅ Retorna null con flag "insufficient_data"
  - ✅ UI muestra "Cargando datos..." en lugar de señal
  - ✅ No genera señal falsa

### 7.5 Caso — Error en Claude API

**Escenario**: La API de Claude no está disponible o hay error de red

- [ ] DADO: Claude API timeout o error 5xx
- [ ] CUANDO: signalDetector intenta confirmación IA
- [ ] ENTONCES:
  - ✅ Señal se emite sin confirmación IA (flag: ai_confirmed = false)
  - ✅ Modal de señal muestra advertencia: "Sin confirmación IA disponible"
  - ✅ Trader puede igualmente aprobar o rechazar
  - ✅ Error registrado en log

### 7.6 Caso — Validación de Indicadores vs TradingView

**Escenario de Testing**: Verificar precisión de cálculos

- [ ] DADO: Dataset histórico de SPY (últimos 200 días, diario)
- [ ] CUANDO: Se calcula RSI(14), MACD(12,26,9), BB(20,2) con los mismos datos
- [ ] ENTONCES:
  - ✅ RSI final difiere < 0.1% respecto a TradingView Pine Script
  - ✅ MACD line difiere < 0.1%
  - ✅ Bollinger Upper/Lower difieren < 0.1%

---

## 8. Requisitos No Funcionales

### 8.1 Performance

```
- Latencia de actualización de quotes: < 500ms desde mercado hasta UI
- Tiempo de cálculo de indicadores (20 símbolos × 9 timeframes): < 500ms
- Tiempo de respuesta de Claude API: < 10 segundos (timeout configurado)
- Render del chart al cambiar símbolo: < 300ms
- Throttle de actualizaciones de precios en UI: máximo 2/seg por símbolo
```

### 8.2 Seguridad

```
- API keys NUNCA en código fuente ni en repositorio git
- API keys en .env (ignorado en .gitignore)
- IBKR: credenciales manejadas solo en TWS local, nunca en cliente web
- Alpaca keys: acceso solo desde variables de entorno del servidor
- VITE_CLAUDE_API_KEY: prefijo VITE_ expone al cliente — usar proxy en producción
- .env.example en repositorio con valores de placeholder únicamente
```

### 8.3 Usabilidad

```
- Dark theme por defecto (estándar en plataformas de trading)
- Responsive: desktop first (mínimo 1280px), con vista compacta en tablet
- Tiempo de carga inicial: < 3 segundos
- Acceso offline: mostrar últimos datos en caché, indicar modo offline
- Atajos de teclado para acciones frecuentes (configurable en v2.0)
```

---

## 9. Tickets de Implementación (Primera Generación)

Los siguientes tickets serán creados automáticamente a partir de esta especificación:

```
TKT-INVRFIC-001: Setup inicial del proyecto (estructura, Vite, deps, .env)
TKT-INVRFIC-002: Implementar broker_connector — Alpaca paper trading
TKT-INVRFIC-003: Implementar market_data — quotes, velas OHLCV y cadena de opciones
TKT-INVRFIC-004: Implementar technical_indicators — RSI, MACD, BB, EMA, ATR, Volume
TKT-INVRFIC-005: Implementar technical_structure — soportes, resistencias y tendencias
TKT-INVRFIC-006: Implementar institutional_flow — UOA, OI y actividad institucional
TKT-INVRFIC-007: Implementar news_events — noticias en tiempo real, earnings y eventos macro
TKT-INVRFIC-008: Implementar fundamentals — snapshot financiero, guidance y valuación
TKT-INVRFIC-009: Implementar opportunity_ranking — ranking diario de empresas prioritarias
TKT-INVRFIC-010: Implementar strategy_manager — presets + selector manual + guardar
TKT-INVRFIC-011: Implementar confluence_engine — coincidencias entre cores activos
TKT-INVRFIC-012: Implementar ai_advisor — integración Claude API y sugerencia contextual
TKT-INVRFIC-013: Implementar strategy_engine — priorización de estrategias y opciones
TKT-INVRFIC-014: Implementar Dashboard UI principal (scanner + chart + TF selector)
TKT-INVRFIC-015: Implementar SignalsTable ultra-detallada (desglose por core + historial)
TKT-INVRFIC-016: Implementar SignalDetailModal (análisis completo + aprobación de operación)
TKT-INVRFIC-017: Implementar PositionsPanel + order_manager (ejecución de acciones/opciones)
TKT-INVRFIC-018: Implementar alerts_service (notificaciones UI + email)
TKT-INVRFIC-019: Tests unitarios — validación indicadores, estructura y ranking diario
TKT-INVRFIC-020: Integración IBKR TWS API (broker primario — post paper-trading)
```

---

## 10. Roadmap: Especificaciones Incrementales Planeadas

Una vez completada la v1.0, las siguientes funcionalidades se especificarán en documentos incrementales:

```
SPEC_002: Módulo de Backtesting de Estrategias
  - Motor de backtesting sobre datos históricos (Polygon.io)
  - Simulación de Iron Condor, Straddle, Strangle
  - Métricas: Sharpe Ratio, Max Drawdown, Win Rate, Profit Factor

SPEC_003: Scanner Avanzado de Flujo Institucional
  - Dark pool prints detection
  - Unusual Options Activity (UOA) scanner
  - Open Interest sweeps y block trades
  - Correlación con precio, IV y eventos

SPEC_004: Módulo de Noticias, Eventos y Fundamentales Avanzado
  - Integración multi-feed de noticias financieras
  - Earnings intelligence y guidance tracker
  - Motor de score fundamental y catalyst heatmap

SPEC_005: Módulo de Opciones Avanzado
  - Builder visual de estrategias (Iron Condor, Straddle, Butterfly)
  - Simulador de P&L por precio y volatilidad (payoff diagram)
  - Greeks dashboard en tiempo real

SPEC_006: Integración IBKR Completa
  - Migración de Alpaca paper trading a IBKR como primario
  - Soporte para órdenes de opciones complejas (multi-leg)
  - Portfolio margin y cálculo de margen en tiempo real

SPEC_007: Dashboard de Performance
  - Estadísticas completas de trading (Win Rate, Avg. Return, etc.)
  - Equity curve y drawdown chart
  - Comparación de performance vs SPY benchmark
```

---

## 11. Preguntas Pendientes (A resolver antes de TKT-001)

### Broker y Cuenta

- [ ] **¿Tienes una cuenta activa en Interactive Brokers?**
      Si sí: confirmar si usarás TWS (port 7497) o IB Gateway (port 4001)
- [ ] **¿Tienes una cuenta en Alpaca?**
      Si sí: confirmar si usarás paper trading o live trading para desarrollo

### API Keys

- [ ] **¿Tienes API Key de Anthropic (Claude)?**
      Necesaria para el servicio de confirmación IA de señales
- [ ] **¿Tienes cuenta y API Key en Alpaca?**
      Necesaria para el broker de desarrollo

### Configuración Inicial

- [ ] **¿Qué símbolos quieres en la watchlist inicial?**
      Sugerencia: SPY, QQQ, AAPL, TSLA, NVDA, MSFT, AMZN, META
- [ ] **¿Quieres comenzar con Alpaca (paper trading) o directo con IBKR?**
      Recomendación: iniciar con Alpaca para desarrollo y testing de señales

---

## 12. Próximos Pasos

Una vez validada esta especificación:

1. ✅ **Picoro generará automáticamente**:
  - Knowledge base inicial (brokers, indicadores, noticias, fundamentales, institucionales y estrategias)
  - `workflow_agents.yaml` con tareas específicas por agente
  - `config.yaml` del proyecto con tech stack completo y arquitectura por cores

2. ✅ **Goku implementará ticket por ticket**:
  - TKT-INVRFIC-001: Setup → estructura + Vite + deps + .env
  - TKT-INVRFIC-002 en adelante: core por core

3. ✅ **Tú validarás**:
  - Ranking diario de oportunidades
  - Señales contra TradingView (benchmark visual)
  - Precisión de indicadores y estructura con datasets conocidos
  - UX del dashboard de oportunidades y señales

4. ✅ **Vegeta y Bulma asegurarán**:
   - Latencia de feeds < 500ms
   - Cálculos de indicadores con < 0.1% de error vs referencia
   - Seguridad de API keys sin exposición en cliente

---

**Esta es la metodología real con IA aplicada a inversiones**:  
Tú especificas el mercado y las reglas, la IA genera la arquitectura y el código, tú validas la lógica financiera y ejecutas con criterio propio.

---

**Última actualización**: 2026-03-11  
**Estado**: 🟡 En Especificación — Pendiente validación de preguntas Sección 11  
**Versión**: 1.2.0  
**Cambios v1.2**:
- ✅ Arquitectura por cores independientes dentro del proyecto
- ✅ Core de indicadores, estructura, institucionales, noticias, fundamentales e IA
- ✅ Ranking diario de oportunidades por empresa/instrumento
- ✅ Sistema de confluencia configurable por cores seleccionados por el usuario
- ✅ Estrategias ampliadas con prioridad dinámica y sugerencia sobre opciones
- ✅ Scanner diario + tabla enriquecida por core
- ✅ Estructura técnica del proyecto alineada con los nuevos cores
- ✅ Tickets TKT-INVRFIC-001 al 020 reorganizados por núcleo funcional
**Próxima revisión**: Tras respuesta a preguntas pendientes → generar TKT-INVRFIC-001
