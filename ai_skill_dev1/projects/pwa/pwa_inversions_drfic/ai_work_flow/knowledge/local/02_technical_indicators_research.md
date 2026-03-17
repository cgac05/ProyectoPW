# 📈 RESEARCH: Indicadores Técnicos & Cálculos

**Investigador**: @picoro  
**Fase**: 2.3 — Investigation & Research  
**Fecha**: March 17, 2026  
**Estado**: ✅ Completado

---

## 1. Indicadores Requeridos (SPECIFICATION v1.0)

Según SPECIFICATION.md, el core `technical_indicators` debe implementar:

```
✅ RSI (Relative Strength Index)
✅ MACD (Moving Average Convergence Divergence)
✅ Bollinger Bands
✅ EMA (Exponential Moving Average)
✅ ATR (Average True Range)
✅ Volume Analysis
```

---

## 2. RSI — Relative Strength Index

### 2.1 Descripción

```
Momentum oscillator que mide velocidad y magnitud de cambios de precio.
Rango: 0-100
Overbought: > 70
Oversold: < 30
```

### 2.2 Cálculo

```
1. RS = Average Gain / Average Loss (primer período n)
2. RSI = 100 - (100 / (1 + RS))

Parámetro típico: Period = 14 velas
```

### 2.3 Fórmula Técnica

```typescript
function calculateRSI(closes: number[], period: number = 14): number[] {
  const deltas = [];
  
  // Calcular cambios
  for (let i = 1; i < closes.length; i++) {
    deltas.push(closes[i] - closes[i - 1]);
  }
  
  // Separar ganancias y pérdidas
  let avgGain = 0;
  let avgLoss = 0;
  
  for (let i = 0; i < period; i++) {
    const delta = deltas[i];
    if (delta > 0) avgGain += delta;
    else avgLoss += Math.abs(delta);
  }
  
  avgGain /= period;
  avgLoss /= period;
  
  const rsi: number[] = [];
  
  for (let i = period; i < deltas.length; i++) {
    const delta = deltas[i];
    
    // Suavizado exponencial
    avgGain = (avgGain * (period - 1) + (delta > 0 ? delta : 0)) / period;
    avgLoss = (avgLoss * (period - 1) + (delta < 0 ? Math.abs(delta) : 0)) / period;
    
    const rs = avgGain / (avgLoss || 0.0001); // Evitar división por cero
    rsi.push(100 - 100 / (1 + rs));
  }
  
  return rsi;
}
```

### 2.4 Usos en Estrategia

```
Señal COMPRA (BUY):
- RSI < 30 (oversold) + cierre por arriba
- RSI cambia de <30 a >30 (bounce)

Señal VENTA (SELL):
- RSI > 70 (overbought) + cierre por abajo
- RSI cambia de >70 a <70 (pullback)

Confirmar con otros indicadores (MACD, Bollinger)
```

---

## 3. MACD — Moving Average Convergence Divergence

### 3.1 Descripción

```
Seguidor de tendencia que muestra relación entre dos promedios móviles.
Componentes:
  • MACD Line = EMA12 - EMA26
  • Signal Line = EMA9 de MACD
  • Histogram = MACD - Signal Line
```

### 3.2 Cálculo

```typescript
function calculateMACD(closes: number[]): {
  macd: number[],
  signal: number[],
  histogram: number[]
} {
  // Calcular EMA12 y EMA26
  const ema12 = calculateEMA(closes, 12);
  const ema26 = calculateEMA(closes, 26);
  
  // MACD Line = EMA12 - EMA26
  const macd = [];
  for (let i = 0; i < ema12.length; i++) {
    macd.push(ema12[i] - ema26[i]);
  }
  
  // Signal Line = EMA9(MACD)
  const signal = calculateEMA(macd, 9);
  
  // Histogram = MACD - Signal
  const histogram = [];
  for (let i = 0; i < signal.length; i++) {
    histogram.push(macd[i + (macd.length - signal.length)] - signal[i]);
  }
  
  return { macd, signal, histogram };
}
```

### 3.3 Usos en Estrategia

```
Señal COMPRA (BUY):
- MACD cruza arriba de Signal Line (bullish cross)
- Histogram cambia de negativo a positivo
- MACD está por arriba de línea cero

Señal VENTA (SELL):
- MACD cruza abajo de Signal Line (bearish cross)
- Histogram cambia de positivo a negativo
- MACD está por abajo de línea cero

Divergencia:
- Precio hace nuevo high pero MACD no → posible reversión SELL
```

---

## 4. Bollinger Bands

### 4.1 Descripción

```
Bandas de volatilidad alrededor de promedio móvil.
Componentes:
  • Middle Band = SMA20 (simple moving average)
  • Upper Band = SMA20 + (2 × std dev)
  • Lower Band = SMA20 - (2 × std dev)
  • Default: period=20, std_dev=2
```

### 4.2 Cálculo

```typescript
function calculateBollingerBands(closes: number[], period: number = 20) {
  const sma = calculateSMA(closes, period);
  const bands: Array<{
    upper: number,
    middle: number,
    lower: number,
    bandwidth: number
  }> = [];
  
  for (let i = period - 1; i < closes.length; i++) {
    const slice = closes.slice(i - period + 1, i + 1);
    
    // Desviación estándar
    const mean = sma[i - (closes.length - sma.length)];
    let variance = 0;
    for (let j = 0; j < slice.length; j++) {
      variance += Math.pow(slice[j] - mean, 2);
    }
    const stdDev = Math.sqrt(variance / period);
    
    // Bandas
    const upper = mean + (2 * stdDev);
    const lower = mean - (2 * stdDev);
    const bandwidth = upper - lower;
    
    bands.push({
      upper,
      middle: mean,
      lower,
      bandwidth: bandwidth / mean // % of price
    });
  }
  
  return bands;
}
```

### 4.3 Usos en Estrategia

```
Squeeze Setup (Baja volatilidad):
- Bandwidth < 5% del precio medio → esperar breakout
- Cuando rompe, movimiento suele ser fuerte

Mean Reversion:
- Precio toca banda superior → posible pullback SELL
- Precio toca banda inferior → posible bounce BUY

Breakout:
- Cierre ARRIBA de banda superior → continuación tendencia alcista
- Cierre ABAJO de banda inferior → continuación tendencia bajista
```

---

## 5. EMA — Exponential Moving Average

### 5.1 Descripción

```
Promedio móvil que da mayor peso a precios recientes.
Parámetro: period (típico 9, 21, 50, 200)
Formula: EMA = (Price × multiplier) + (EMA_prev × (1 - multiplier))
Multiplier = 2 / (period + 1)
```

### 5.2 Cálculo

```typescript
function calculateEMA(closes: number[], period: number): number[] {
  const multiplier = 2 / (period + 1);
  const ema: number[] = [];
  
  // EMA inicial = SMA del primer período
  let sum = 0;
  for (let i = 0; i < period; i++) {
    sum += closes[i];
  }
  ema.push(sum / period);
  
  // EMA recursivo
  for (let i = period; i < closes.length; i++) {
    const newEMA = (closes[i] * multiplier) + (ema[ema.length - 1] * (1 - multiplier));
    ema.push(newEMA);
  }
  
  return ema;
}
```

### 5.3 Cruces de EMA (EMA Crossover Strategy)

```
Bullish Signal (BUY):
- EMA_FAST (9) cruza arriba de EMA_SLOW (21)
- Ejemplo: EMA9 > EMA21 y precio > EMA21

Bearish Signal (SELL):
- EMA_FAST (9) cruza abajo de EMA_SLOW (21)
- Ejemplo: EMA9 < EMA21 y precio < EMA21

Trend Confirmation:
- Para uptrend: EMA9 > EMA21 > EMA50 > EMA200
- Para downtrend: EMA9 < EMA21 < EMA50 < EMA200
```

---

## 6. ATR — Average True Range

### 6.1 Descripción

```
Mide volatilidad del precio sin considerar dirección.
Usado para:
  • Stop-loss placement
  • Position sizing
  • Volatility confirmation
  • Breakout levels

Parámetro: period = 14 (típico)
```

### 6.2 Cálculo

```typescript
function calculateATR(highs: number[], lows: number[], closes: number[], period: number = 14): number[] {
  const trueRanges: number[] = [];
  
  // Calcular True Range
  for (let i = 1; i < closes.length; i++) {
    const tr = Math.max(
      highs[i] - lows[i],                    // High - Low
      Math.abs(highs[i] - closes[i - 1]),    // High - Close_prev
      Math.abs(lows[i] - closes[i - 1])      // Low - Close_prev
    );
    trueRanges.push(tr);
  }
  
  // Calcular ATR (EMA del TR)
  const atr: number[] = [];
  let sum = 0;
  
  for (let i = 0; i < period; i++) {
    sum += trueRanges[i];
  }
  atr.push(sum / period);
  
  // ATR suavizado
  for (let i = period; i < trueRanges.length; i++) {
    const newATR = (atr[atr.length - 1] * (period - 1) + trueRanges[i]) / period;
    atr.push(newATR);
  }
  
  return atr;
}
```

### 6.3 Aplicaciones

```
Stop Loss Dinámico:
- Stop Loss = Close - (2 × ATR)  // Para longs
- Stop Loss = Close + (2 × ATR)  // Para shorts

Position Sizing:
- Position Size ∝ 1/ATR
- Mayor volatilidad → posición más pequeña

Breakout Entry:
- Entry Level = Support/Resistance + (ATR × 0.5)
- Confirmación de volatilidad para trade válido
```

---

## 7. Volume Analysis

### 7.1 Componentes

```typescript
interface VolumeMetrics {
  volume: number;           // Volumen crudo
  volumeMA: number;         // Promedio móvil del volumen
  volumeRatio: number;      // Volumen actual / Promedio
  moneyFlow: number;        // Money Flow = Price × Volume
  moneyFlowMA: number;      // Promedio de Money Flow
  signalStrength: number;   // 0-1: fortaleza de signal
}
```

### 7.2 Cálculos

```typescript
function analyzeVolume(closes: number[], volumes: number[], period: number = 20) {
  const calculations: VolumeMetrics[] = [];
  
  for (let i = period; i < volumes.length; i++) {
    const slice = volumes.slice(i - period, i);
    const volumeMA = slice.reduce((a, b) => a + b) / period;
    const volumeRatio = volumes[i] / (volumeMA || 1);
    
    const moneyFlow = closes[i] * volumes[i];
    const moneyFlowSlice = closes.slice(i - period, i)
      .map((c, idx) => c * volumes[i - period + idx]);
    const moneyFlowMA = moneyFlowSlice.reduce((a, b) => a + b) / period;
    
    // Signal strength: qué tan anormal es este volumen
    const signalStrength = Math.min(1, volumeRatio / 2); // 2x = strong signal
    
    calculations.push({
      volume: volumes[i],
      volumeMA,
      volumeRatio,
      moneyFlow,
      moneyFlowMA,
      signalStrength
    });
  }
  
  return calculations;
}
```

### 7.3 Interpretación

```
Volumen Alto en COMPRA:
- Cierre verde (up) + volumen > 1.5× media
- → Señal fuerte de COMPRA (confirmación)

Volumen Alto en VENTA:
- Cierre rojo (down) + volumen > 1.5× media
- → Señal fuerte de VENTA (confirmación)

Volumen Bajo:
- Movimiento con bajo volumen
- → Débil, puede revertir

Climax Volume:
- Volumen extremadamente alto (>3× media)
- → Posible fin de movimiento (reversal setup)
```

---

## 8. Librería Recomendada

```json
{
  "dependencies": {
    "tulind": "^1.0.2",           // TradingView Lightweight Indicators
    "technicalindicators": "^3.1.0"  // Alternativa completa
  }
}
```

### 8.1 Uso con `technicalindicators`

```typescript
import { RSI, MACD, BollingerBands, EMA, ATR } from 'technicalindicators';

// RSI
const rsi = new RSI({ period: 14, values: closes });
console.log(rsi.getResult()); // Array de valores RSI

// MACD
const macd = new MACD({ values: closes });
console.log(macd.getResult()); // { macd, signal, histogram }

// Bollinger Bands
const bb = new BollingerBands({ period: 20, stdDev: 2, values: closes });
console.log(bb.getResult()); // Array de bandas

// EMA
const ema = new EMA({ period: 21, values: closes });
console.log(ema.getResult()); // Array de EMA
```

---

## 9. Integración en Core

```typescript
// technical_indicators core interface
export interface TechnicalIndicatorResult {
  timestamp: number;
  rsi: number;            // 0-100
  macd: {
    line: number;
    signal: number;
    histogram: number;
  };
  bollinger: {
    upper: number;
    middle: number;
    lower: number;
    bandwidth: number;
  };
  ema: {
    ema9: number;
    ema21: number;
    ema50: number;
  };
  atr: number;
  volume: {
    current: number;
    average: number;
    ratio: number;
  };
}

export function calculateIndicators(
  candles: OHLCV[],
  indicators: string[] = ['rsi', 'macd', 'bollinger', 'ema', 'atr', 'volume']
): TechnicalIndicatorResult[] {
  // Implementar cálculos aquí
}
```

---

## 10. Signals Generator

```typescript
export interface SignalStrength {
  direction: 'BUY' | 'SELL' | 'HOLD';
  confidence: number; // 0-1
  reasons: string[];
  indicators: string[];
}

export function generateSignal(indicators: TechnicalIndicatorResult[], previous: TechnicalIndicatorResult): SignalStrength {
  const reasons: string[] = [];
  let buyScore = 0;
  let sellScore = 0;
  
  // RSI Oversold → BUY
  if (indicators.rsi < 30) {
    buyScore += 1;
    reasons.push('RSI < 30 (oversold)');
  }
  
  // MACD Bullish Cross → BUY
  if (indicators.macd.line > indicators.macd.signal && 
      previous.macd.line <= previous.macd.signal) {
    buyScore += 1.5;
    reasons.push('MACD bullish crossover');
  }
  
  // Bollinger Lower Band Touch → BUY
  if (/* candle touched lower band */) {
    buyScore += 0.5;
    reasons.push('Price touched lower Bollinger band');
  }
  
  // EMA Crossover Bullish → BUY
  if (indicators.ema.ema9 > indicators.ema.ema21) {
    buyScore += 1;
    reasons.push('EMA9 > EMA21');
  }
  
  // Similar para SELL signals pero con dirección opuesta
  
  const totalScore = buyScore + sellScore;
  const direction = buyScore > sellScore ? 'BUY' : sellScore > buyScore ? 'SELL' : 'HOLD';
  const confidence = totalScore > 0 ? Math.min(1, Math.max(buyScore, sellScore) / totalScore) : 0;
  
  return {
    direction,
    confidence,
    reasons,
    indicators: ['rsi', 'macd', 'bollinger', 'ema']
  };
}
```

---

## ✅ Conclusiones

1. **Todos los indicadores son calculables** con fórmulas estándar
2. **Usar librería `technicalindicators`** para evitar bugs de implementación
3. **Generar signals** basadas en COMBINACIÓN de múltiples indicadores
4. **No usar un solo indicador** — siempre confirmar con al menos 2-3
5. **ATR es crítico** para stops y position sizing

---

**Siguiente**: TKT-INVRFIC-004 — Implementar technical_indicators core  
**Siguiente**: TKT-INVRFIC-005 — Refinar signal generation algorithm
