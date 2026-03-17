# TKT-INVRFIC-002 through TKT-INVRFIC-020: Summary

**Fase**: FASE 2.4 → FASE 3  
**Generador**: @picoro  
**Fecha**: March 17, 2026  

---

## TKT-INVRFIC-002: MongoDB Collections Setup

**Asignado**: @krillin | **Sprint**: 1 | **Est**: 2 días

Crear collections MongoDB para cache ephemeral:
- `candles` (OHLCV data, TTL 5min)
- `options_chains` (Options data, TTL 2h)
- `news_cache` (Headlines, TTL 24h)
- `indicators_cache` (Calculados indicators, TTL 5min)
- `quotes` (Real-time precios, TTL 1min)

Indices: TTL automáticos, compound indices para (symbol, timestamp)

---

## TKT-INVRFIC-003: Authentication System (Passport.js + JWT)

**Asignado**: @goku | **Sprint**: 1 | **Est**: 3 días

Implementar autenticación:
- [ ] Signup/Login endpoint
- [ ] JWT token generation (expires 24h)
- [ ] Refresh token strategy
- [ ] Email verification
- [ ] Password hashing (bcrypt)
- [ ] Two-factor auth (TOTP)
- [ ] Passport.js middleware

**FIC comment required** en todos los exports públicos (EN/ES)

---

## TKT-INVRFIC-004: IBroker Interface & Implementations

**Asignado**: @krillin | **Sprint**: 1 | **Est**: 3 días

Crear abstracción genérica para brokers:

```typescript
export interface IBroker {
  connect(): Promise<void>;
  disconnect(): Promise<void>;
  getQuote(symbol: string): Promise<Quote>;
  getHistoricalData(symbol, timeframe, limit): Promise<Candle[]>;
  getOptionsChain(symbol): Promise<OptionChain>;
  placeOrder(order: Order): Promise<OrderResult>;
  getPositions(): Promise<Position[]>;
}
```

Interfaces para Quote, Candle, OptionChain, Order, etc.

---

## TKT-INVRFIC-005: AlpacaConnector Implementation

**Asignado**: @goku | **Sprint**: 2 | **Est**: 3 días

Implementar `AlpacaConnector` que implementa `IBroker`:
- [ ] REST API connection
- [ ] Real-time WebSocket quotes
- [ ] Historical data fetchin
- [ ] Order placement (paper trading)
- [ ] Error handling + retry logic
- [ ] Connection pooling

Usar: `@alpacahq/alpaca-trade-api`

---

## TKT-INVRFIC-006: IBKRConnector Implementation

**Asignado**: @goku | **Sprint**: 2 | **Est**: 4 días

Implementar `IBKRConnector` que implementa `IBroker`:
- [ ] TWS connection (localhost:7497)
- [ ] Real-time quotes
- [ ] Options chain fetching
- [ ] Historical data
- [ ] Order placement
- [ ] Greeks calculation (para options)
- [ ] Error handling

Usar: `@stoqey/ib`

---

## TKT-INVRFIC-007: Message Queue Infrastructure (Bull)

**Asignado**: @krillin | **Sprint**: 1 | **Est**: 2 días

Setup event-driven architecture:
- [ ] Redis instance (local dev, managed prod)
- [ ] Bull queues initialization
- [ ] Event types definition:
  - `market_update` (candle cerrado)
  - `signal_generated` (nuevo signal)
  - `trade_executed` (orden enviada)
  - `trade_closed` (orden cerrada)
- [ ] Queue consumers base implementation
- [ ] Error handling & dead-letter queues

---

## TKT-INVRFIC-008: Technical Indicators Core

**Asignado**: @goku | **Sprint**: 2 | **Est**: 4 días

Implementar `technical_indicators` core service:
- [ ] RSI calculator (period 14)
- [ ] MACD calculator (12, 26, 9)
- [ ] Bollinger Bands (period 20, stddev 2)
- [ ] EMA calculators (9, 21, 50, 200)
- [ ] ATR calculator (period 14)
- [ ] Volume analysis

usar librería `technicalindicators`

Output: `TechnicalSignal` con signal ('BUY'/'SELL'/'HOLD') + confidence

**FIC comments required** en todos los exports

---

## TKT-INVRFIC-009: Technical Structure Core

**Asignado**: @goku | **Sprint**: 2 | **Est**: 4 días

Implementar `technical_structure` core:
- [ ] Support/Resistance detection
- [ ] Chart pattern recognition:
  - Double top/bottom
  - Triangles (symmetric, ascending, descending)
  - Flags and pennants
- [ ] Breakout detection
- [ ] Trend line identification

Output: `StructureSignal` con levels, patterns, breakout info

---

## TKT-INVRFIC-010: Institutional Flow Core

**Asignado**: @goku | **Sprint**: 3 | **Est**: 4 días

Implementar `institutional_flow` core:
- [ ] Options data fetching via IBKR
- [ ] Put/Call ratio calculator
- [ ] Open Interest analyzer
- [ ] IV Skew detector
- [ ] Sector flow aggregation

Output: `InstitutionalSignal` detectando BULLISH/BEARISH/HEDGING behavior

---

## TKT-INVRFIC-011: News & Events Core

**Asignado**: @goku | **Sprint**: 3 | **Est**: 3 días

Implementar `news_events` core:
- [ ] Integration con news API (NewsAPI, etc)
- [ ] Sentiment analysis (basic NLP)
- [ ] Earnings calendar (Polygon.io o similar)
- [ ] Economic calendar events
- [ ] FDA/regulatory event tracking

Output: `NewsEventSignal` con upcoming events, recent news, aggregate sentiment

---

## TKT-INVRFIC-012: Fundamentals Core

**Asignado**: @goku | **Sprint**: 3 | **Est**: 3 días

Implementar `fundamentals` core:
- [ ] Financial data fetching (SEC EDGAR, data providers)
- [ ] Ratio calculations:
  - P/E, PEG, P/S, P/B
  - Debt-to-equity
  - Current ratio
  - ROE, ROA
- [ ] Valuation assessment
- [ ] Quality scoring

Output: `FundamentalSignal` con valuation + quality + signal

---

## TKT-INVRFIC-013: Claude API Integration (AI Advisor)

**Asignado**: @goku | **Sprint**: 3 | **Est**: 3 días

Integrar Anthropic Claude API:
- [ ] Client setup con API key management
- [ ] Prompt engineering para síntesis
- [ ] Input: Outputs de los 5 cores anteriores
- [ ] Output: `AISignal` con recomendación + reasoning

Prompt debe:
- Sintetizar todos los cores
- Dar recomendación clara (BUY/SELL/HOLD/WAIT)
- Explicar confidence level
- Identificar riesgos

**FIC comments** en prompts y funciones

---

## TKT-INVRFIC-014: Maestro Orchestrator

**Asignado**: @goku | **Sprint**: 4 | **Est**: 4 días

Implementar `maestro_orchestrator` que:
- [ ] Ejecuta 6 cores en paralelo (Promise.all)
- [ ] Collects outputs
- [ ] Calls Claude API con síntesis
- [ ] Generates `FinalSignal` con:
  - Recommendation (BUY/SELL/HOLD)
  - Confidence score
  - Trade setup (entry, TP, SL)
  - Risk/reward ratio
- [ ] Publish signal a message queue
- [ ] Store en Supabase

Output frequency: Cada minuto (o configurable)

---

## TKT-INVRFIC-015: REST API Endpoints

**Asignado**: @goku | **Sprint**: 2 | **Est**: 3 días

Crear Express.js API endpoints:
- [ ] `POST /auth/signup`, `/login`, `/refresh`
- [ ] `GET /api/quotes/:symbol`
- [ ] `GET /api/signals/:symbol`
- [ ] `POST /api/orders` (place order)
- [ ] `GET /api/orders` (order history)
- [ ] `GET /api/portfolio` (portfolio state)
- [ ] `GET /api/trades` (trade history)
- [ ] `WebSocket /ws` (real-time updates)

Rate limiting, error handling, validation, authentication middleware

---

## TKT-INVRFIC-016: React Component Library

**Asignado**: @goku | **Sprint**: 4 | **Est**: 3 días

Crear components base:
- [ ] `<Button>`, `<Input>`, `<Card>`, `<Modal>`
- [ ] `<SignalCard>` (display signals)
- [ ] `<PortfolioWidget>` (portfolio summary)
- [ ] `<TradeForm>` (place trade)
- [ ] `<Navbar>`, `<Sidebar>`

TailwindCSS styling, dark mode support

**FIC comments** en todos los exports

---

## TKT-INVRFIC-017: TradingView Charts Integration

**Asignado**: @goku | **Sprint**: 4 | **Est**: 3 días

Integrar TradingView Lightweight Charts:
- [ ] Chart initialization
- [ ] Real-time OHLCV data updates
- [ ] Series rendering (candlestick, line)
- [ ] Markers para entries/exits
- [ ] Price level lines (SL, TP)
- [ ] Responsive layout

React hooks para data management

---

## TKT-INVRFIC-018: Trade Execution & Monitoring

**Asignado**: @goku | **Sprint**: 4 | **Est**: 4 días

Implementar trade lifecycle:
- [ ] Order validation (risk checks)
- [ ] Place order via broker connector
- [ ] Monitor for TP/SL hits
- [ ] Auto-close on targets (configurable)
- [ ] P&L calculation
- [ ] Trade history logging

Supabase storage, real-time WebSocket updates

---

## TKT-INVRFIC-019: E2E Integration & Testing

**Asignado**: @bulma | **Sprint**: 5 | **Est**: 5 días

Comprehensive testing:
- [ ] End-to-end API tests
- [ ] Database integrity tests
- [ ] Broker connector tests (paper trading)
- [ ] Signal generation pipeline tests
- [ ] Frontend component tests (React Testing Library)
- [ ] UI flow tests (Cypress)

Target: >80% code coverage

**FIC comments required** en test files

---

## TKT-INVRFIC-020: Performance Optimization & PWA

**Asignado**: @vegeta | **Sprint**: 5 | **Est**: 4 días

Optimizar para producción:
- [ ] Frontend optimization:
  - Code splitting
  - Lazy loading
  - Image optimization
  - Compression
- [ ] PWA setup:
  - Service Worker
  - Offline support
  - Add to home screen
- [ ] Performance monitoring (Sentry)
- [ ] SEO optimization

Target: Lighthouse >90, Time to Interactive <3.5s

---

## FIC Documentation Standard

**CRITICAL**: Sin FIC comments, tickets NO pueden cerrarse.

Patrón obligatorio en TODOS los public exports:

```typescript
/**
 * FIC: Calculate RSI indicator
 * FIC: Calcula el indicador RSI
 * 
 * @param closes Array de precios de cierre
 * @param period Período del RSI (default 14)
 * @returns Array de valores RSI 0-100
 */
export function calculateRSI(closes: number[], period: number = 14): number[] {
  // Implementation
}
```

---

## Summary

- **20 tickets total**
- **5 sprints** de ~2 semanas cada = ~10 semanas
- **Recursos**: @goku (primary dev), @krillin (DB/infra), @bulma (testing), @vegeta (optimization)
- **Blockers**: Ninguno, pueden ejecutarse en paralelo

**Next Step**: FASE 2.4 — Detailed architectural design con @krillin + @goku

---

**Generated by**: @picoro  
**Review**: Pendiente @bulma + @vegeta  
**Approved**: Pendiente Project Manager
