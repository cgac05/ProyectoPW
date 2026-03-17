# 🏗️ RESEARCH: Arquitectura de Sistema — Integración Completa

**Investigador**: @picoro  
**Fase**: 2.3 — Investigation & Research  
**Fecha**: March 17, 2026  
**Estado**: ✅ Completado

---

## Executive Summary

La plataforma `pwa_inversions_drfic` es un **PWA de trading AI-asistido** que integra:

1. **Datos de múltiples brokers** (IBKR primario, Alpaca secundario)
2. **6 cores técnicos** generando signals semi-autónomas
3. **Base de datos híbrida** (Supabase para datos críticos, MongoDB para caché)
4. **Frontend React 18** con gráficos TradingView interactivos
5. **Backend Node.js** con orquestación de cores via messaging

**Objetivo**: Identificar oportunidades de trading (acciones + opciones) con confianza suficiente para ejecución semi-automática con aprobación del usuario.

---

## 1. Arquitectura de Sistema (High Level)

```
┌─────────────────────────────────────────────────────────────┐
│         FRONTEND — PWA (React 18 + TypeScript)              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Dashboard | Charts (TradingView) | Signals | Orders │   │
│  │  State Management (Zustand)                          │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                          ↑↓
              REST API / WebSocket
                          ↓
┌─────────────────────────────────────────────────────────────┐
│         BACKEND — Node.js + TypeScript                      │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ API Server (Express.js)                              │   │
│  │  ├─ /api/quotes                                      │   │
│  │  ├─ /api/signals                                    │   │
│  │  ├─ /api/orders                                     │   │
│  │  └─ /api/portfolio                                  │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ Message Queue (Redis or Bull Queues)                │   │
│  │  Events: market_update, signal_generated, etc       │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ ORCHESTRATOR CORE (Maestro)                          │   │
│  │  Coordina los 6 cores, retorna signals finales       │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
          ↑                           ↓ orchestrates
          │         ┌────────────────┼────────────────┐
          │         ↓                ↓                ↓
  ┌──────────────┐ ┌────────────┐ ┌────────────┐  ┌────────────┐
  │ technical_   │ │technical_  │ │ news_      │  │ fundamental│
  │ indicators   │ │ structure  │ │ events     │  │ s          │
  │              │ │            │ │            │  │            │
  │ RSI, MACD,   │ │ Support,   │ │ Earnings,  │  │ P/E, EPS,  │
  │ Bollinger    │ │ Resistance,│ │ Fed, FDA   │  │ Debts, ROE │
  │ EMA, ATR,    │ │ Breakouts  │ │ decisions  │  │ Div Yield  │
  │ Volume       │ │            │ │            │  │            │
  └──────────────┘ └────────────┘ └────────────┘  └────────────┘
          ↑                              ↑
          └──────────────┬───────────────┘
              Market Data:
              Quotes, OHLCV, Options Chain,
              News, Economic calendar
          ↑
          │ Brokers API
          │
  ┌───────┴────────────────┐
  │                        │
┌─────────────────┐  ┌─────────────────┐
│ IBKR (TWS)      │  │ Alpaca Paper    │
│ ├─ Stock Quotes │  │ ├─ Stock Quotes │
│ ├─ Options      │  │ ├─ Historical   │
│ ├─ Historical   │  │ └─ Orders (Dev) │
│ └─ Orders       │  └─────────────────┘
└─────────────────┘
          ↓
  ┌──────────────────────────────┐
  │ Market Data Cache (MongoDB)  │
  │  • Quotes (5min TTL)         │
  │  • Historical/OHLCV (1month) │
  │  • Options Chains (2h TTL)   │
  │  • News cache                │
  └──────────────────────────────┘
          
  ┌──────────────────────────────┐
  │ Critical Data (Supabase)     │
  │  • User Accounts             │
  │  • Trades Executed           │
  │  • Signals History           │
  │  • Portfolio State           │
  │  • Settings                  │
  └──────────────────────────────┘
```

---

## 2. Los 6 Cores + Maestro

### 2.1 Core 1: Technical Indicators

```typescript
// 📊 technical_indicators
export interface TechnicalSignal {
  symbol: string;
  timestamp: number;
  
  indicators: {
    rsi: number;
    macd: { line: number; signal: number; histogram: number };
    bollinger: { upper: number; middle: number; lower: number };
    ema: { ema9: number; ema21: number; ema50: number };
    atr: number;
    volume: { current: number; ratio: number };
  };
  
  signal: 'BUY' | 'SELL' | 'HOLD';
  confidence: 0-1;
  reasoning: string[];
}

// Inputs: OHLCV candles (últimas 200)
// Output: Signal cada minuto/vela
// Responsibility: Análisis puro de números, sin opinión de evento
```

### 2.2 Core 2: Technical Structure

```typescript
// 📍 technical_structure
export interface StructureSignal {
  symbol: string;
  timestamp: number;
  
  levels: {
    supports: Array<{ price: number; strength: number; touches: number }>;
    resistances: Array<{ price: number; strength: number; touches: number }>;
  };
  
  patterns: Array<{
    name: 'double_top' | 'double_bottom' | 'triangle' | 'flag';
    confidence: number;
    projection?: number; // Precio target
  }>;
  
  signal: 'BREAKOUT_UP' | 'BREAKOUT_DOWN' | 'REJECTION' | 'HOLD';
  confidence: 0-1;
}

// Inputs: OHLCV candles (últimas 500-1000 para pattern finding)
// Output: Identification de levels y patterns
// Responsibility: Identificar estructura de precio, no timing
```

### 2.3 Core 3: Institutional Flow

```typescript
// 🏦 institutional_flow
export interface InstitutionalSignal {
  symbol: string;
  timestamp: number;
  
  optionsAnalysis: {
    putCallRatio: number;
    oi: { calls: number; puts: number };
    ivSkew: number;
    flowType: 'BULLISH' | 'BEARISH' | 'HEDGING' | 'NEUTRAL';
  };
  
  event?: {
    name: string;
    daysAway: number;
    historicalMove: number; // % movimento típico post-evento
  };
  
  signal: 'ACCUMULATION' | 'DISTRIBUTION' | 'HEDGING' | 'NORMAL';
  confidence: 0-1;
}

// Inputs: Options chain, Put/Call data
// Output: Detección de flujo institucional
// Responsibility: Interpretar comportamiento de opciones
```

### 2.4 Core 4: News & Events

```typescript
// 📰 news_events
export interface NewsEventSignal {
  symbol: string;
  timestamp: number;
  
  upcoming: Array<{
    event: string; // "Earnings", "FDA Decision", etc
    date: number;
    daysAway: number;
    expectedMove?: number; // % volatilidad esperada
  }>;
  
  recent: Array<{
    headline: string;
    source: string;
    sentiment: 'BULLISH' | 'BEARISH' | 'NEUTRAL';
    impact: 'HIGH' | 'MEDIUM' | 'LOW';
    publishedAt: number;
  }>;
  
  aggregateSentiment: number; // -1 to +1
  signal: 'BULLISH' | 'BEARISH' | 'CAUTIOUS' | 'MONITOR';
}

// Inputs: News feeds, Event calendars
// Output: Sentiment + upcoming catalysts
// Responsibility: Context del mercado, no timing
```

### 2.5 Core 5: Fundamentals

```typescript
// 💼 fundamentals
export interface FundamentalSignal {
  symbol: string;
  timestamp: number;
  
  metrics: {
    pe: number;
    peg: number;
    ps: number;
    pbv: number;
    debt: number; // Debt-to-Equity
    currentRatio: number;
    roe: number;
    roa: number;
    eps: number;
    eps_growth: number;
  };
  
  valuation: 'UNDERVALUED' | 'FAIR' | 'OVERVALUED';
  quality: 'HIGH' | 'MEDIUM' | 'LOW';
  
  signal: 'STRONG_BUY' | 'BUY' | 'HOLD' | 'SELL' | 'STRONG_SELL';
  confidence: 0-1;
}

// Inputs: Financial statements, SEC filings
// Output: Valuation + quality score
// Responsibility: Long-term value assessment
```

### 2.6 Core 6: AI Advisor (Claude API)

```typescript
// 🤖 ai_advisor
export interface AISignal {
  symbol: string;
  timestamp: number;
  
  analysis: string; // Markdown synthesis de todos los cores
  recommendation: {
    action: 'BUY' | 'SELL' | 'HOLD' | 'WAIT';
    confidence: number;
    riskLevel: 'LOW' | 'MEDIUM' | 'HIGH';
    timeframe: 'INTRADAY' | 'SWING' | 'POSITION';
    positionSize: 'SMALL' | 'MEDIUM' | 'LARGE';
  };
  
  reasoning: string;
  risks: string[];
  opportunities: string[];
}

// Inputs: Outputs de todos los 5 cores anteriores
// Output: Síntesis + recomendación final
// Responsibility: Orquestar, sintetizar, recomendar
```

### 2.7 Maestro Orchestrator

```typescript
// 🎼 maestro_orchestrator
export async function orchestrateSignal(symbol: string): Promise<FinalSignal> {
  // Ejecutar cores en paralelo
  const [technical, structure, institutional, news, fundamental] = 
    await Promise.all([
      technical_indicators.get(symbol),
      technical_structure.get(symbol),
      institutional_flow.get(symbol),
      news_events.get(symbol),
      fundamentals.get(symbol)
    ]);
  
  // Enviar a AI Advisor para síntesis
  const aiSignal = await ai_advisor.synthesize({
    technical,
    structure,
    institutional,
    news,
    fundamental
  });
  
  // Generar trade setup final
  const finalSignal: FinalSignal = {
    symbol,
    timestamp: Date.now(),
    coreSignals: {
      technical: technical.signal,
      structure: structure.signal,
      institutional: institutional.signal,
      news: news.signal,
      fundamental: fundamental.signal
    },
    aiRecommendation: aiSignal.recommendation,
    
    tradeSetup: {
      entry: calculateEntry(structure, technical),
      takeProfit: calculateTP(technical, aiSignal),
      stopLoss: calculateSL(technical, aiSignal),
      riskReward: calculateRR(entry, tp, sl),
      
      strategy: selectStrategy(aiSignal, institutional),
      // 'long_call' | 'bull_spread' | 'covered_call' | 'basic_stock' | etc
    },
    
    readyForExecution: aiSignal.recommendation.confidence > 0.65,
  };
  
  return finalSignal;
}
```

---

## 3. Data Flow Completo

### 3.1 Market Update Cycle (Cada minuto)

```
1. Market Data fetcher (IBKR/Alpaca)
   ↓
2. Store latest candle in MongoDB cache
   ↓
3. Publish 'market_update' event to queue
   ↓
4. Cores subscriben a event, recalculan:
   ├─ technical_indicators (5m, 15m, 1h candles)
   ├─ technical_structure (buscar breaks)
   ├─ institutional_flow (cambios en OI)
   ├─ news_events (check calendar)
   └─ fundamentals (cache valido, no recalc cada min)
   ↓
5. Cada core publica su 'signal_updated' event
   ↓
6. Maestro orchestrator escucha todos y genera FinalSignal
   ↓
7. FinalSignal published to WebSocket → Frontend actualiza
   ↓
8. Si readyForExecution=true, almacenar en Supabase y alertar usuario
```

### 3.2 Trade Execution Flow

```
User ve signal en UI:
  ↓
Click "Approve Trade"
  ↓
Backend valida:
  ├─ Cuenta tiene suficiente capital?
  ├─ Position size correcto para risk?
  ├─ Stop-loss y TP dentro de limites?
  └─ No hay trade abierto en ese símbolo?
  ↓
IF all validations pass:
  ├─ Send order to IBKR/Alpaca broker
  ├─ Almacenar en Supabase.trades
  ├─ Iniciar stop-loss monitor
  ├─ Iniciar take-profit monitor
  └─ Publish 'order_executed' event
  ↓
Monitor Loop (cada minuto):
  ├─ Check si hit SL → auto-sell, lock loss
  ├─ Check si hit TP → auto-sell, lock profit
  ├─ Check si signal reversa → alert user para early exit
  └─ Update UI con P&L
  ↓
Order cierra:
  ├─ Record trade result en Supabase
  ├─ Calcular P&L exacto
  ├─ Update portfolio state
  └─ Publish 'trade_closed' event
```

---

## 4. Database Schema

### 4.1 Supabase (PostgreSQL) — Critical Data

```sql
-- Users & Accounts
CREATE TABLE users (
  id UUID PRIMARY KEY,
  email VARCHAR UNIQUE,
  created_at TIMESTAMP,
  last_login TIMESTAMP
);

CREATE TABLE accounts (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users,
  broker_type VARCHAR, -- 'IBKR', 'ALPACA'
  api_key_encrypted VARCHAR,
  account_number VARCHAR,
  balance DECIMAL,
  buying_power DECIMAL,
  updated_at TIMESTAMP
);

-- Trades
CREATE TABLE trades (
  id UUID PRIMARY KEY,
  account_id UUID REFERENCES accounts,
  symbol VARCHAR,
  trade_type VARCHAR, -- 'BUY', 'SELL', 'CALL', 'PUT', 'SPREAD'
  entry_price DECIMAL,
  entry_time TIMESTAMP,
  quantity INTEGER,
  
  stop_loss DECIMAL,
  take_profit DECIMAL,
  
  exit_price DECIMAL,
  exit_time TIMESTAMP,
  exit_reason VARCHAR,
  
  pnl DECIMAL,
  pnl_percent DECIMAL,
  
  signal_id UUID REFERENCES signals,
  created_at TIMESTAMP
);

-- Signals Archive (histórico)
CREATE TABLE signals (
  id UUID PRIMARY KEY,
  symbol VARCHAR,
  timestamp TIMESTAMP,
  
  technical_indicator_signal VARCHAR,
  technical_structure_signal VARCHAR,
  institutional_signal VARCHAR,
  news_signal VARCHAR,
  fundamental_signal VARCHAR,
  
  ai_recommendation VARCHAR,
  ai_confidence DECIMAL,
  
  trade_executed BOOLEAN,
  accuracy DECIMAL, -- Post-trade, si ganó o perdió
  
  created_at TIMESTAMP
);

-- Portfolio State
CREATE TABLE portfolio (
  id UUID PRIMARY KEY,
  account_id UUID REFERENCES accounts,
  total_value DECIMAL,
  cash DECIMAL,
  open_positions INTEGER,
  win_rate DECIMAL,
  avg_win DECIMAL,
  avg_loss DECIMAL,
  updated_at TIMESTAMP
);
```

### 4.2 MongoDB — Cache & Ephemeral Data

```json
// Colección: candles
{
  _id: ObjectId,
  symbol: "SPY",
  timeframe: "1m",
  timestamp: 1710777600000,
  open: 450.25,
  high: 451.00,
  low: 449.50,
  close: 450.75,
  volume: 1250000,
  ttl_expires: 1710777900000  // 5 minutos TTL
}

// Colección: options_chains
{
  _id: ObjectId,
  symbol: "SPY",
  timestamp: 1710777600000,
  expiration: "2026-03-20",
  
  calls: [
    { strike: 450, bid: 2.50, ask: 2.55, iv: 0.15, oi: 125000, volume: 5000 },
    // ... more strikes
  ],
  puts: [
    { strike: 450, bid: 2.40, ask: 2.45, iv: 0.16, oi: 110000, volume: 4500 },
    // ... more strikes
  ],
  
  ttl_expires: 1710781200000  // 1 hora TTL
}

// Colección: news_cache
{
  _id: ObjectId,
  symbol: "SPY",
  items: [
    { 
      headline: "Market rallies on Fed comments",
      source: "Bloomberg",
      sentiment: "BULLISH",
      url: "...",
      published_at: 1710777600000
    }
  ],
  ttl_expires: 1710864000000  // 24 horas TTL
}
```

---

## 5. Technology Stack Decisiones

### 5.1 Frontend

```
✅ React 18+
   - Server Components ready
   - Suspense para data loading
   - Context API + Zustand para state

✅ TypeScript
   - Type-safe signals pipeline
   - Autocomplete en core integrations

✅ Vite
   - Build super rápido
   - HMR durante dev

✅ TailwindCSS
   - Utility-first, rápido de iterar
   - Dark mode incluido

✅ TradingView Lightweight Charts
   - Gráficos profesionales
   - Rendimiento excelente
   - Soporte para markers (entries, exits)

✅ Zustand
   - State ligero, simple
   - No boilerplate como Redux
```

### 5.2 Backend

```
✅ Node.js 18+
   - Ecosistema NPM maduro para finance
   - Async/await para I/O broker

✅ TypeScript
   - Type safety en cores
   - Debugging más fácil

✅ Express.js
   - API REST simple
   - Middleware ecosystem

✅ Bull Queues o Redis Streams
   - Event-driven architecture
   - Garantizar entrega de eventos

✅ Passport.js
   - Auth simple
   - JWT tokens

✅ Socket.io
   - WebSocket real-time
   - Fallback a polling
```

### 5.3 Databases

```
✅ Supabase (PostgreSQL)
   - Replication segura
   - ACID guarantees
   - Row-level security (permisos por usuario)
   - Backups automáticos

✅ MongoDB
   - Flexible schema para caché
   - TTL indices automáticos
   - Agregaciones rápidas
```

### 5.4 External APIs

```
✅ @stoqey/ib (IBKR)
   - Best-in-class para opciones
   - Datos completos

✅ @alpacahq/alpaca-trade-api (Alpaca)
   - REST + WebSocket
   - Excelente para paper trading dev

✅ Anthropic Claude API
   - Síntesis AI de signals
   - Explicación para usuario
   - Highest quality reasoning

✅ NewsAPI o similar
   - Headlines y sentiment
   - Filtrado por símbolo
```

---

## 6. Deployment Architecture

### 6.1 Frontend Deployment

```
Options:
1. Vercel (Recomendado para PWA)
   - Edge functions para API routing
   - Serverless functions
   - Automatic PWA optimization

2. Cloudflare Pages
   - Similar a Vercel
   - Mejor para Europa

3. GitHub Pages
   - Gratis pero limitado (SSG only)
```

### 6.2 Backend Deployment

```
Options:
1. Railway (Recomendado)
   - Postgres incluido
   - MongoDB add-on
   - ENV variables manejadas
   - Auto-deploy from GitHub

2. Render.com
   - Similar a Railway
   - Free tier disponible

3. AWS (Más complejo pero escalable)
   - EC2 para backend
   - RDS para Postgres
   - DocumentDB para MongoDB
   - SQS para mensajería
```

### 6.3 Broker Integration

```
Local Development:
  ├─ IBKR: TWS local en puerto 7497
  ├─ Alpaca: API keys en .env
  └─ Claude: API key en .env

Production:
  ├─ IBKR: IB Gateway en servidor privado
  ├─ Alpaca: Live API (si aplicable)
  └─ Claude: API key vía Supabase secrets
```

---

## 7. Security Considerations

### 7.1 Broker Credentials

```
❌ NUNCA guardar en código
✅ Usar Supabase Vault para encripción
✅ Acceso via environment variables en runtime
✅ Rotar keys regularmente
✅ Usar API keys con scopes limitados
```

### 7.2 Trade Execution

```
✅ Validar ALL órdenes en backend
✅ Autenticación multi-factor para cambiar settings
✅ Rate limiting en endpoints
✅ Audit log de todas las trades ejecutadas
✅ Email confirmation para cambios sensibles
```

### 7.3 Data Privacy

```
✅ HTTPS/TLS para todas las conexiones
✅ JWT tokens con expiración corta
✅ Rate limiting por usuario
✅ No almacenar passwords en plain text
✅ GDPR compliance si hay usuarios EU
```

---

## 8. Roadmap de Implementación

### Fase 1: Core Infrastructure (FASE 2.4)
```
TKT-001: Database schema creation (Supabase + MongoDB)
TKT-002: Authentication system (Passport.js + JWT)
TKT-003: Broker connector abstraction (IBroker interface)
TKT-004: Message queue setup (Bull Queues)
```

### Fase 2: Core Implementation (FASE 3 Part 1)
```
TKT-005: Technical Indicators core
TKT-006: Technical Structure core
TKT-007: Institutional Flow core
TKT-008: News & Events core
TKT-009: Fundamentals core
```

### Fase 3: AI & Orchestration (FASE 3 Part 2)
```
TKT-010: Claude API integration
TKT-011: Maestro orchestrator
TKT-012: Signal generation pipeline
TKT-013: Trade execution service
```

### Fase 4: Frontend (FASE 3 Part 3)
```
TKT-014: React component library (buttons, cards, modals)
TKT-015: TradingView charts integration
TKT-016: Signal display + approval UI
TKT-017: Portfolio dashboard
TKT-018: Trade history view
```

### Fase 5: Testing & Polish (FASE 3 Part 4)
```
TKT-019: Integration tests
TKT-020: Performance optimization
TKT-021: PWA optimization (service workers)
```

---

## 9. Performance Targets

```
Frontend:
  ✅ Lighthouse score > 90
  ✅ LCP (Largest Contentful Paint) < 2.5s
  ✅ TTI (Time to Interactive) < 3.5s
  ✅ Chart rendering < 100ms

Backend:
  ✅ API response <200ms
  ✅ Signal generation <500ms
  ✅ IBKR data refresh <100ms
  ✅ 99.9% uptime

Database:
  ✅ Supabase: <50ms query time
  ✅ MongoDB: <10ms cache hit
```

---

## 10. Alternativas Consideradas

### 10.1 Trading Libraries
```
❌ Backtrader (Python, no PWA)
❌ Lean (C# empresarial)
✅ TradingView Pine Script (herramienta exterior)
✅ Implementación custom (máximo control)
```

### 10.2 Messaging
```
❌ Kafka (overkill para escala actual)
✅ Bull Queues (simple, Redis-based)
⚠️ RabbitMQ (alternativa si escalas)
```

### 10.3 AI Models
```
❌ Local LLM (requiere GPU, lentitud)
✅ Claude API (mejor reasoning)
⚠️ GPT-4 (más caro)
```

---

## ✅ Conclusiones Arquitectura

1. **Loose coupling**: Cada core independiente, via message queue
2. **Type safety**: TypeScript end-to-end reduce bugs
3. **Scalability**: Mastercard horizontal (load balancer)
4. **Hybrid data**: Supabase para crítico, MongoDB para cache
5. **AI-first**: Claude API como orquestador y synthesizer
6. **Security**: Keys nunca en código, validación en backend
7. **Real-time**: WebSockets para updates en vivo
8. **Developer experience**: Vite + TypeScript + hot reload

---

## Tickets Generados

Ver sección siguiente: [Tickets de Implementación](#tickets)

---

**Siguiente**: FASE 2.3.2 — Generar 20 Tickets (TKT-INVRFIC-001 a TKT-INVRFIC-020)  
**Siguiente**: FASE 2.4 — Detailed Design con @krillin + @goku
