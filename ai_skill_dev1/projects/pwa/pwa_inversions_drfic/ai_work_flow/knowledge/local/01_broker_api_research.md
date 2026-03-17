# 📊 RESEARCH: APIs de Brokers — Interactive Brokers & Alpaca

**Investigador**: @picoro  
**Fase**: 2.3 — Investigation & Research  
**Fecha**: March 17, 2026  
**Estado**: ✅ Completado

---

## 1. Interactive Brokers (IBKR) — Broker Primario

### 1.1 Librería Recomendada

**Librería**: `@stoqey/ib`  
**Versión**: >=2.0.0  
**Repo**: https://github.com/stoqey/ib  
**Lenguaje**: TypeScript/Node.js wrapper sobre IB API

### 1.2 Conexión

```typescript
// Opción A: TWS Local (desarrollo)
IBKR_HOST=127.0.0.1
IBKR_PORT=7497
IBKR_CLIENT_ID=1

// Opción B: IB Gateway (producción)
IBKR_HOST=127.0.0.1
IBKR_PORT=4001
IBKR_CLIENT_ID=1
```

**Requisito**: Tener TWS (Trader Workstation) o IB Gateway corriendo localmente

### 1.3 Datos Disponibles

```
✅ Cotizaciones en tiempo real (Bid/Ask/Last/Volume)
✅ Datos históricos OHLCV (múltiples timeframes)
✅ Cadena de opciones (strikes, expirations, Greeks)
✅ Nivel 2 (profundidad de mercado)
✅ Open Interest
✅ Divisas
✅ Futuros
```

### 1.4 Características Clave

| Feature | Disponible | Notas |
|---------|-----------|-------|
| Real-time quotes | ✅ | Acceso directo a nivel 1 |
| Historical data | ✅ | Múltiples timeframes (1m-1Y) |
| Options chains | ✅ | Completas con Greeks calculados |
| Order execution | ✅ | Soporte completo (STK, OPT, FUT) |
| Portfolio margin | ✅ | Requiere cuenta específica |
| Paper trading | ⚠️ | Disponible pero limitado vs Alpaca |
| API stability | ✅ | Muy estable, conecta via TCP |
| Latencia | 🔥 | Excelente (<100ms típico) |
| Rate limits | ✅ | Datos sin límite una vez conectado |

### 1.5 Limitaciones Conocidas

```
❌ No hay Python 3 support oficial (solo Node.js wrapper)
❌ Documentación limitada vs otros brokers
❌ Requiere TWS activo (puede consumir recursos)
❌ Requisitos de cuenta mínimos para ciertas features
```

### 1.6 Conexión Típica

```typescript
import IBKRConnector from '@stoqey/ib';

const connector = new IBKRConnector({
  host: process.env.IBKR_HOST,
  port: parseInt(process.env.IBKR_PORT),
  clientId: parseInt(process.env.IBKR_CLIENT_ID),
  accountId: process.env.IBKR_ACCOUNT_ID,
});

await connector.connect();
// ✅ Conectado

// Suscribir a quotes
connector.onQuote(symbol, (quote) => {
  console.log(`${symbol}: ${quote.bid}/${quote.ask}`);
});

// Obtener datos históricos
const bars = await connector.getHistoricalData(symbol, '15 m', 100);
// ✅ 100 velas de 15m
```

---

## 2. Alpaca — Broker Secundario (Papel Trading)

### 2.1 Librería Recomendada

**Librería**: `@alpacahq/alpaca-trade-api`  
**Versión**: >=3.0.0  
**Repo**: https://github.com/alpacahq/alpaca-trade-api-js  
**Tipo**: REST API + WebSocket

### 2.2 Conexión

```typescript
// Paper Trading (Desarrollo)
ALPACA_API_KEY=PK_xxxxxx
ALPACA_SECRET_KEY=xxxxx
ALPACA_BASE_URL=https://paper-api.alpaca.markets
ALPACA_DATA_URL=https://data.alpaca.markets  // Polygon.io powered

// Live Trading (si aplica)
ALPACA_BASE_URL=https://api.alpaca.markets
```

### 2.3 Datos Disponibles

```
✅ Cotizaciones en tiempo real (vía WebSocket)
✅ Datos históricos OHLCV (vía Polygon.io)
✅ Sin opciones nativas (solo acciones y ETFs)
⚠️ Cadena de opciones NO disponible en Alpaca
✅ Noticias (vía Polygon.io)
```

### 2.4 Características Clave

| Feature | Disponible | Notas |
|---------|-----------|-------|
| Real-time quotes | ✅ | WebSocket stream |
| Historical data | ✅ | Powered by Polygon.io |
| Options chains | ❌ | NO disponible |
| Order execution | ✅ | REST API simple |
| Paper trading | ✅ | Perfecto para desarrollo |
| API stability | ✅ | Muy confiable |
| Latencia | ✅ | 100-500ms típico |
| Rate limits | ⚠️ | 1000 requests/min papel |
| Acciones US | ✅ | S&P 500 y más |
| Sin comisiones | ✅ | Comisiones = 0 |
| Margin | ⚠️ | Solo papel trading |

### 2.5 WebSocket Real-Time

```typescript
import Alpaca from '@alpacahq/alpaca-trade-api';

const alpaca = new Alpaca();

// Conectar a WebSocket
const websocket = alpaca.websocket();

// Suscribir a quotes
websocket.subscribe('trades', ['SPY', 'QQQ'], (trade) => {
  console.log(`${trade.S}: ${trade.p} @ ${trade.s} shares`);
});

websocket.connect();
```

---

## 3. Comparativa: IBKR vs Alpaca

| Aspecto | IBKR | Alpaca |
|--------|------|--------|
| **Para Producción** | ✅ Primario | ⚠️ Paper only |
| **Opciones** | ✅ Completo | ❌ No soporta |
| **Datos Históricos** | ✅ Nativo | ✅ Polygon.io |
| **Latencia** | 🔥 Excelente | ✅ Buena |
| **Documentación** | ⚠️ Limitada | ✅ Excelente |
| **Setup** | ⚠️ Complejo | ✅ Simple |
| **Requisitos** | TWS activo | API Key |

---

## 4. Recomendaciones de Arquitectura

### 4.1 Estrategia de Dual-Broker

```
DESARROLLO (ALPACA):
├─ Paper trading para validar signals
├─ Testing de estrategias sin riesgo
└─ Setup rápido y simple

PRODUCCIÓN (IBKR):
├─ Ejecución en vivo
├─ Acceso a opciones
├─ Portfolio margin si aplica
└─ Datos de institucionales (nivel 2)
```

### 4.2 Arquitectura de Connector Interface

```typescript
interface IBroker {
  connect(): Promise<void>;
  disconnect(): Promise<void>;
  getAccount(): Promise<Account>;
  getQuote(symbol: string): Promise<Quote>;
  getHistoricalData(symbol: string, timeframe: string, limit: number): Promise<Candle[]>;
  getOptionsChain(symbol: string): Promise<OptionChain>;
  placeOrder(order: Order): Promise<OrderResult>;
  getPositions(): Promise<Position[]>;
}

// Implementaciones:
// ├─ IBKRConnector implements IBroker
// └─ AlpacaConnector implements IBroker
```

### 4.3 Fallback Strategy

```
Intenta IBKR → Si falla, fallback a Alpaca (paper)
Esto permite que dev/test continúe incluso sin TWS
```

---

## 5. Plan de Integración

### 5.1 Sprint 1: Alpaca (Más Rápido)
- Implementar `AlpacaConnector`
- Obtener datos históricos
- Suscribir a real-time quotes
- Testing con paper trading

### 5.2 Sprint 2: IBKR Básico
- Implementar `IBKRConnector`
- Conectar a TWS local
- Obtener quotes y datos históricos
- Obtener cadena de opciones

### 5.3 Sprint 3: IBKR Completo
- Órdenes de acciones
- Órdenes de opciones
- Portfolio margin
- Monitoreo de posiciones

---

## 6. Dependencias NPM a Instalar

```json
{
  "dependencies": {
    "@stoqey/ib": "^2.0.0",
    "@alpacahq/alpaca-trade-api": "^3.1.0"
  }
}
```

---

## ✅ Conclusiones

1. **IBKR** es el broker primario para producción (opciones, institucionales)
2. **Alpaca** es excelente para desarrollo y validación de signals
3. **Arquitectura dual** permite máxima flexibilidad
4. **Implementar `IBroker` interface** primero, luego conectores específicos
5. **Alpaca como fallback** si IBKR no está disponible

---

**Siguiente**: TKT-INVRFIC-002 — Implementar AlpacaConnector  
**Siguiente**: TKT-INVRFIC-003 — Implementar market_data service
