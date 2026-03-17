# 🎯 RESEARCH: Estrategias de Opciones & Flujo Institucional

**Investigador**: @picoro  
**Fase**: 2.3 — Investigation & Research  
**Fecha**: March 17, 2026  
**Estado**: ✅ Completado

---

## 1. Contexto: International Options Markets

### 1.1 Opciones en IBKR

```
Exchanges soportados:
✅ CBOE (Cboe Options Exchange) — US Options
✅ ISE (International Securities Exchange)
✅ EDGX (Edge Options)
✅ PHLX (Philadelphia Stock Exchange)
✅ MIAX (Miami International Holdings)

Subyacentes:
✅ Acciones US (SPY, QQQ, XLE, IWM, etc.)
✅ Índices (RUT, SPX, NDX)
✅ ETFs
✅ Futuros (FX, Commodity, Index)

Tipos de Opciones:
✅ American (antes de expiración, cualquier momento)
✅ European (solo al expiración)
```

### 1.2 Modelos de Precio (Greeks)

```
Delta (Δ):      Cambio de opción por $1 cambio en subyacente
Gamma (Γ):      Cambio de delta por $1 cambio en subyacente
Theta (Θ):      Decay por día (time decay)
Vega (ν):       Cambio por 1% cambio en volatilidad
Rho (ρ):        Cambio por 1% cambio en tasa de interés
```

---

## 2. Estrategias Básicas

### 2.1 Long Call (Alcista Moderada)

```
Entry:
  - Comprar 1 Call ITM (in-the-money) o ATM
  - Strike: Soporte próximo o actual

Exit:
  - TP: Strike + 2 × ATR
  - SL: Strike - 1 × ATR
  - Time: 30-45 DTE (days to expiration)

Greeks Utilizados:
  - Delta: 0.6-0.8 (se mueve con precio)
  - Gamma: Positiva (gana aceleración)
  - Theta: Negativa (pierde por tiempo)

Risk/Reward:
  - Risk: Costo de opción (premium)
  - Reward: Ilimitado (teóricamente)
  - R:R típico: 1:3 o 1:5
```

### 2.2 Short Call (Bajista)

```
Entry:
  - Vender 1 Call OTM (out-of-money)
  - Strike: Resistencia próxima

Exit:
  - TP: 50% del crédito recibido (ideal)
  - SL: Difícil, riesgo teórico ilimitado
  - Time: 7-21 DTE (decay rápido)

Greeks Utilizados:
  - Delta: -0.2 a -0.4 (neutral a bajista)
  - Theta: Positiva (sale ganando con tiempo)
  - Vega: Negativa (quiere baja volatilidad)

Risk/Reward:
  - Risk: Ilimitado (técnicamente)
  - Reward: Crédito recibido
  - R:R: NO RECOMENDADO para retail sin cobertura
```

### 2.3 Bull Call Spread (Alcista Controlada)

```
Estructura:
  BUY Call @ Strike 1 (ITM o ATM)
  SELL Call @ Strike 2 (OTM)
  
  Ejemplo SPY $450:
    BUY  SPY $450C (Delta 0.7)
    SELL SPY $455C (Delta 0.4)
    
Entry:
  - Entrada: Strike 1 (más bajo)
  - Spread: Diferencia entre strikes

Exit:
  - TP: Máximo profit = (Strike2 - Strike1) - Debit
  - SL: -100% del debit
  - Breakeven: Strike1 + Debit

Greeks Utilizados:
  - Delta: Neta 0.3-0.4 (moderadamente alcista)
  - Gamma: Positiva en zona central
  - Theta: Ligeramente positiva (shorts vence antes)
  - Vega: Neutral a ligeramente positiva

Risk/Reward:
  - Risk: Debit pagado
  - Reward: Spread máximo - Debit
  - R:R: 1:2 típico
  - % Ganancia Máxima: (Spread - Debit) / Debit × 100

Caso Práctico:
  Debit pagado: $0.50 × 100 = $50
  Spread: $5 × 100 = $500
  Max profit: $500 - $50 = $450
  % Max: 900%
```

### 2.4 Bear Call Spread (Bajista Controlada)

```
Estructura:
  SELL Call @ Strike 1 (ITM o ATM)
  BUY Call @ Strike 2 (OTM)
  
  Ejemplo SPY $450:
    SELL SPY $450C (Delta 0.7)
    BUY  SPY $455C (Delta 0.4)

Entry:
  - Entrada: Strike 1 (más bajo)
  - Crédito: Diferencia de primas

Exit:
  - TP: Máximo ganancia = Crédito recibido
  - SL: Spread máximo - Crédito
  - Breakeven: Strike2 - Crédito

Greeks Utilizados:
  - Delta: Neta -0.3 a -0.4 (moderadamente bajista)
  - Theta: Positiva (gana con tiempo)
  - Vega: Positiva (quiere baja volatilidad)

Risk/Reward:
  - Risk: Spread máximo - Crédito
  - Reward: Crédito
  - R:R típico: 1:1 a 1:2
```

### 2.5 Iron Condor (Neutral/Rango Bound)

```
Estructura:
  PUT Spread (bajista):
    SELL Put @ Strike 1
    BUY Put @ Strike 2
  
  CALL Spread (alcista):
    SELL Call @ Strike 3
    BUY Call @ Strike 4

Ejemplo SPY $450 ± $10:
    SELL SPY $440P / BUY $435P (downside protection)
    SELL SPY $460C / BUY $465C (upside protection)

Entry:
  - Rango esperado: Entre strikes interiores
  - Crédito: Suma de ambos spreads

Exit:
  - TP: 50% del crédito total
  - SL: Detrás de strikes exteriores

Greeks Utilizados:
  - Delta: Neta cercana a 0 (neutral)
  - Theta: Fuertemente positiva
  - Vega: Positiva (quiere baja volatilidad)

Risk/Reward:
  - Risk: ((Strike4 - Strike3) - Crédito total) × 100
  - Reward: Crédito total
  - R:R: 1:1 típico (ambos spreads del mismo ancho)
```

### 2.6 Covered Call (Ingreso con Acciones)

```
Requisitos:
  - Poseer acciones del subyacente
  - 100 acciones = 1 contrato opciones

Estructura:
  HOLD 100 acciones
  SELL 1 Call OTM (5-10% arriba del precio)

Entry:
  - Acciones compradas a cierto precio
  - Call vendido genera ingreso

Exit:
  - TP1: Call vence sin ejercerse (ingreso limpio)
  - TP2: Acciones llamadas (ejercidas, venta forzada)
  - SL: Si acciones caen, pérdida directa

Greeks Utilizados:
  - Delta: De acciones +100, de call -40 probablemente = Net +60
  - Theta: Positiva (venta gana con tiempo)

Risk/Reward:
  - Risk: Pérdida de acciones + oportunidad si baja
  - Reward: Dividendos + prima de opción
  - Ideal para: Cartera bullish de largo plazo

Ejemplo:
  HOLD 100 TSLA @ $200
  SELL 1 TSLA $210C (recibe $2 × 100 = $200)
  
  Escenario 1 (TSLA cae a $180):
    Pérdida: $2000 (pero cobró $200 opción)
    Neta: -$1800
  
  Escenario 2 (TSLA sube a $220):
    Acciones llamadas @ $210
    Ganancia: $1000 + $200 opción = $1200
  
  Escenario 3 (TSLA sube a $208):
    Call no ejercida, vence sin valor
    Ganancia: $800 + $200 opción = $1000
```

---

## 3. Flujo Institucional (Institutional Flow)

### 3.1 Concepto

```
Los traders institucionales (hedge funds, banks, market makers)
dejan rastros en datos de opciones que reveals sus intenciones.

Indicadores:
✅ Volumen de opciones inusual
✅ Open Interest creciente
✅ Put/Call ratio extremo
✅ IV Skew alteraciones
✅ Dark Pool activity
✅ Block trades
```

### 3.2 Put/Call Ratio Analysis

```
Put/Call Ratio = Volume de Puts / Volume de Calls

Interpretación:
  > 1.5: Muy bajista (máximo miedo)
  1.0 - 1.5: Moderadamente bajista
  0.6 - 1.0: Equilibrio
  0.3 - 0.6: Moderadamente alcista
  < 0.3: Muy alcista (máximo entusiasmo)

Señales Contrarias (Contrary Indicators):
  - Ratio extremo Put/Call ALTO → Posible rally (todos hedged, sin venta)
  - Ratio extremo Put/Call BAJO → Posible pullback (todos bullish, complacencia)

Uso Práctico:
  - Monitorear cambios rápidos en ratio
  - Buscar extremos (<0.2 o >2.0)
  - Confirmar con otros indicadores
```

### 3.3 Open Interest (OI)

```
Open Interest = Número total de contratos abiertos en un strike/expiration

Señal:
  - OI creciente en strike ITM  → Acumulación
  - OI creciente en strike OTM  → Protección/Hedge
  - OI decreciente                → Posiciones cerrándose

Combinado con Volumen:
  - High Volume + Rising OI  → Nuevo money entrando (conti tendencia)
  - High Volume + Falling OI → Posiciones saliendo (reversal posible)

Ejemplo Bullish:
  - Calls OTM: +50% OI, +100% volumen
  - Puts OTM: Estables u bajando
  - → Institucionales buildeando calls (alcista)
```

### 3.4 IV (Implied Volatility) Skew

```
IV Skew = Diferencia de IV entre strikes

Normal (Smirk):
  - OTM Puts: IV más alta (miedo a caída)
  - ATM: IV media
  - OTM Calls: IV más baja

Cambios Anormales:
  - Aumento en put IV  → Hedging increase (miedo)
  - Aumento en call IV → Bullish premium (optimismo extremo)

Señal Institucional:
  - IV skew extremo en dirección → Flujo institucional detectado
```

### 3.5 Sector Flows (Dinero Fluyendo Entre Sectores)

```
Monitorear:
✅ XLE (Energy) Calls vs Puts → Detectar si institucionales vuelven a XLE
✅ XLF (Finance) Puts → Detectar hedging bancario
✅ QQQ (Nasdaq) Calls → Tech bullishness
✅ IWM (Small Cap) → Risk appetite

Correlaciones:
  - Si QQQ calls suben + IWM llama baja = Profit taking en tech, risk off
  - Si XLE calls suben + Energy stocks suben = Real money entrando
```

---

## 4. Integración: News Events + Options

### 4.1 Pre-Earnings (Antes de Ganancias)

```
Patrón Institucional Común:

1. Semana antes de earnings: IV sube 30-50%
2. Posibles movimientos:
   a) Sell ITM Calls + Sell OTM Puts = "Estrangulo corto" (gana si range pequeño)
   b) Buy ITM Calls + Buy OTM Puts = "Straddle" (gana si range grande)
   
3. Detector de intención:
   - Si calls suben más que puts → Institucionales alcistas
   - Si puts suben más que calls → Institucionales bajistas

Nuestro Algoritmo:
  IF earnings_próximo < 10 días AND IV > histórico × 1.3:
    IF ratio_calls/puts > 2:
      Signal: "Institucionales acumulando calls — alcista"
      Entry: Bull Call Spread con breakout
    ELSE IF ratio_puts/calls > 2:
      Signal: "Institucionales acumulando puts — bajista"
      Entry: Bear Call Spread con breakdown
```

### 4.2 FDA Approval / News Events

```
Patrón Pre-Evento (24-48h antes):

OTM Calls aumentan 200%+ de OI:
  → Institucionales esperan outcome positivo
  → Alto gamma risk para market makers
  → Posible gapping up si aprobado

OTM Puts aumentan 200%+ de OI:
  → Institucionales hedging riesgo downside
  → Bajo reward si evento positivo
  → Posible gapping down si rechazado

Nuestra Estrategia:
  1. Monitorear OI cambios 48h antes de evento
  2. Si calls >> puts: Pequeña posición call (downside limitado)
  3. Si puts >> calls: Pequeña posición put (downside limitado)
  4. Exit inmediatamente después de evento (volatilidad colapsará)
```

---

## 5. Arquitectura del Core: institutional_flow

### 5.1 Data Recolección

```typescript
export interface InstitutionalSignal {
  timestamp: number;
  symbol: string;
  
  // Options Data
  optionsChain: {
    totalVolume: number;
    callVolume: number;
    putVolume: number;
    putCallRatio: number;
    
    // Open Interest
    openInterest: number;
    callOI: number;
    putOI: number;
    
    // IV Metrics
    iv: number;
    ivSkew: number; // Puts IV - Calls IV
    ivHistorical: number;
    ivPercentile: number; // 0-100
  };
  
  // Flujo Detectado
  flow: {
    type: 'BULLISH' | 'BEARISH' | 'NEUTRAL' | 'HEDGING';
    confidence: number; // 0-1
    sources: string[]; // Qué detectó esto
  };
  
  // Evento Próximo
  event?: {
    name: string;
    daysAway: number;
    type: 'EARNINGS' | 'FDA' | 'ECONOMIC' | 'CATALYST';
  };
}
```

### 5.2 Detectores de Flujo

```typescript
function detectInstitutionalFlow(options: OptionChain): InstitutionalSignal {
  const signals: string[] = [];
  let bullishScore = 0;
  let bearishScore = 0;
  
  // Detector 1: Put/Call Ratio
  if (options.putCallRatio > 1.5) {
    bearishScore += 1;
    signals.push('Put/Call ratio extreme bearish');
  }
  if (options.putCallRatio < 0.3) {
    bullishScore += 1;
    signals.push('Put/Call ratio extreme bullish');
  }
  
  // Detector 2: IV Skew
  if (options.ivSkew > 0.1) {
    bearishScore += 1;
    signals.push('IV skew toward puts (hedging)');
  }
  
  // Detector 3: OI Creciente
  if (options.callOI / previousCallOI > 1.5) {
    bullishScore += 1.5;
    signals.push('Call OI dramatically rising');
  }
  
  // Detector 4: IV Extremo
  if (options.ivPercentile > 80) {
    signals.push('IV percentile > 80% (event risk)');
  }
  
  const type = bullishScore > bearishScore ? 'BULLISH' : 'BEARISH' : 'NEUTRAL';
  const confidence = Math.min(1, Math.max(bullishScore, bearishScore) / 3);
  
  return {
    timestamp: Date.now(),
    symbol: options.symbol,
    optionsChain: options,
    flow: { type, confidence, sources: signals },
    event: detectNearbyEvents(options.symbol)
  };
}
```

---

## 6. Trade Setup: Options + Institucional + Technical

### 6.1 Combinación de Señales

```
SETUP FUERTE:
  1. Indicador técnico ha de CALL de compra (RSI<30, MACD cross, EMA bullish)
  2. Flujo institucional detecta BULLISH (Put/Call bajo, OI calls alto)
  3. Volatilidad CONTENIDA (no hay evento immediate)
  
  ACCIÓN: Bull Call Spread de 2-3 weeks DTE
  TARGETING: 30-50% ROI

SETUP HEDGED (Bajista):
  1. Indicador técnico SELL (RSI>70, MACD bajada, precio bajo EMA)
  2. Flujo institucional detecta HEDGING (Put OI subiendo)
  3. Evento próximo (earnings) dentro de 5-10 días
  
  ACCIÓN: Vender OTM Calls, Comprar OTM Puts (Iron Condor o Bear)
  RISK: Esperar evento
  TARGETING: 20-30% ROI si rango se mantiene
```

### 6.2 Stops Técnicos en Options

```
Para Long Call / Bull Spread:
  - Stop: Si breaks por debajo de soporte clave
  - Loss cierto en % del crédito pagado
  
Para Short Call / Bear Spread:
  - Stop: Si precio cierra arriba del strike vendido
  - Loss crece con movimiento alcista

Regla de Oro:
  - Siempre definir SL ANTES de entrada
  - Nunca dejar "abierto" a uncertainty
```

---

## 7. Implementación Roadmap

```
TKT-INVRFIC-006: Crear OptionChain data fetcher (IBKR)
TKT-INVRFIC-007: Calcular Greeks (delta,gamma,theta,vega)
TKT-INVRFIC-008: Implementar Put/Call Ratio analyzer
TKT-INVRFIC-009: Crear IV Skew detector
TKT-INVRFIC-010: Integrar event calendar (earnings, FDA)
TKT-INVRFIC-011: Implementar strategy builder (Bull/Bear/Iron Condor)
TKT-INVRFIC-012: Crear institutional flow detector
TKT-INVRFIC-013: Backtest strategies vs 2023-2024 data
TKT-INVRFIC-014: Paper trading integration (Alpaca options si disponible, else simulated)
```

---

## ✅ Conclusiones

1. **Options son herramienta poderosa** para entrada controlada (spreads) y hedging
2. **Estrategias principales**: Bull Call, Bear Call, Iron Condor, Covered Call
3. **Flujo institucional** se detecta mediante Put/Call ratio, OI, IV changes
4. **Combinación técnica + options + flujo** = Setup de alta probabilidad
5. **Risk/Reward debe ser 1:2 mínimo** para viabilidad de trading
6. **Siempre usar spreads** (no naked calls/puts) para riesgo controlado

---

**Siguiente**: TKT-INVRFIC-006 — Implementar OptionChain fetcher  
**Siguiente**: TKT-INVRFIC-008 — Analyzer de Put/Call Ratio
