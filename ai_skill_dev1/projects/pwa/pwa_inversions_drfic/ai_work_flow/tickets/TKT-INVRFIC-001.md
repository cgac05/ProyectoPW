# TKT-INVRFIC-001: Database Schema Creation (Supabase PostgreSQL)

**Fase**: 2.4 → 3 (Infrastructure)  
**Asignado a**: @krillin  
**Sprint**: 1  
**Prioridad**: CRÍTICA  
**Estimación**: 3 días  
**Status**: 🟡 READY FOR SPRINT 1

---

## Descripción

Crear el schema Postgres completo en Supabase para almacenar datos críticos:
- Usuarios y cuentas de broker
- Trades ejecutados (histórico)
- Signals generadas (archive)
- Portfolio state
- Settings y configuración

Este es el **source de verdad** para datos que require ACID guarantees y replication.

---

## Requisitos Técnicos

### RF1: Tabla `users`
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  email_verified BOOLEAN DEFAULT false,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  password_hash VARCHAR(255),
  
  -- Security
  two_fa_enabled BOOLEAN DEFAULT false,
  two_fa_secret VARCHAR,
  
  -- Metadata
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now(),
  last_login TIMESTAMPTZ,
  
  -- Settings
  preferred_currency VARCHAR(3) DEFAULT 'USD',
  timezone VARCHAR(50),
  
  -- Supabase Auth Integration
  auth_user_id UUID UNIQUE REFERENCES auth.users(id)
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_auth_user_id ON users(auth_user_id);
```

### RF2: Tabla `accounts` (Broker Accounts)
```sql
CREATE TABLE accounts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  
  -- Broker Info
  broker_type VARCHAR(20) NOT NULL, -- 'IBKR', 'ALPACA'
  account_number VARCHAR(50) UNIQUE NOT NULL,
  account_name VARCHAR(100),
  
  -- APIs (ENCRYPTED via Supabase Vault)
  api_key_encrypted VARCHAR,      -- Vault secret ref
  api_secret_encrypted VARCHAR,   -- Vault secret ref
  api_base_url VARCHAR,
  
  -- Account State (cached from broker)
  account_type VARCHAR, -- 'LIVE', 'PAPER'
  balance DECIMAL(15, 2),
  buying_power DECIMAL(15, 2),
  cash DECIMAL(15, 2),
  net_liquidation_value DECIMAL(15, 2),
  
  -- Connection
  is_connected BOOLEAN DEFAULT false,
  last_sync TIMESTAMPTZ,
  connection_error VARCHAR,
  
  -- Metadata
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now(),
  is_active BOOLEAN DEFAULT true,
  
  CONSTRAINT unique_user_broker UNIQUE (user_id, broker_type)
);

CREATE INDEX idx_accounts_user_id ON accounts(user_id);
CREATE INDEX idx_accounts_broker ON accounts(broker_type);
```

### RF3: Tabla `trades` (Trade History)
```sql
CREATE TABLE trades (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  account_id UUID NOT NULL REFERENCES accounts(id) ON DELETE CASCADE,
  
  -- Trade Identification
  symbol VARCHAR(20) NOT NULL,
  trade_type VARCHAR(20), -- 'BUY', 'SELL', 'LONG_CALL', 'SHORT_CALL', 'BULL_SPREAD', etc
  quantity INTEGER NOT NULL,
  
  -- Entry
  entry_price DECIMAL(10, 2) NOT NULL,
  entry_time TIMESTAMPTZ NOT NULL,
  entry_signal_id UUID REFERENCES signals(id),
  
  -- Exit
  exit_price DECIMAL(10, 2),
  exit_time TIMESTAMPTZ,
  exit_reason VARCHAR, -- 'TP', 'SL', 'MANUAL', 'REVERSAL'
  
  -- Risk Management
  stop_loss DECIMAL(10, 2),
  take_profit DECIMAL(10, 2),
  risk_per_trade DECIMAL(5, 2), -- % del account
  
  -- P&L
  pnl DECIMAL(12, 2),
  pnl_percent DECIMAL(6, 2),
  commission DECIMAL(8, 2),
  
  -- Trade Status
  status VARCHAR(20), -- 'OPEN', 'CLOSED', 'CANCELLED'
  
  -- Broker Reference
  broker_order_id VARCHAR(100),
  
  -- Metadata
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now(),
  notes VARCHAR(500)
);

CREATE INDEX idx_trades_account ON trades(account_id);
CREATE INDEX idx_trades_symbol ON trades(symbol);
CREATE INDEX idx_trades_status ON trades(status);
CREATE INDEX idx_trades_date ON trades(entry_time DESC);
```

### RF4: Tabla `signals` (Signal Archive)
```sql
CREATE TABLE signals (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  
  -- Symbol & Time
  symbol VARCHAR(20) NOT NULL,
  timestamp TIMESTAMPTZ NOT NULL,
  
  -- Core Signals (JSON stored for flexibility)
  technical_signal VARCHAR, -- 'BUY', 'SELL', 'HOLD'
  technical_confidence DECIMAL(3, 2),
  
  structure_signal VARCHAR,
  structure_confidence DECIMAL(3, 2),
  
  institutional_signal VARCHAR,
  institutional_confidence DECIMAL(3, 2),
  
  news_signal VARCHAR,
  news_confidence DECIMAL(3, 2),
  
  fundamental_signal VARCHAR,
  fundamental_confidence DECIMAL(3, 2),
  
  -- AI Synthesis
  ai_recommendation VARCHAR,
  ai_confidence DECIMAL(3, 2),
  ai_reasoning TEXT,
  
  -- Trade Setup
  suggested_entry DECIMAL(10, 2),
  suggested_tp DECIMAL(10, 2),
  suggested_sl DECIMAL(10, 2),
  recommended_strategy VARCHAR, -- 'bull_call', 'bear_call', 'stock_long', etc
  
  -- Execution
  ready_for_execution BOOLEAN DEFAULT false,
  trade_executed BOOLEAN DEFAULT false,
  executed_via_trade_id UUID REFERENCES trades(id),
  
  -- Performance (post-trade)
  signal_accuracy DECIMAL(3, 2),
  
  -- Metadata
  created_at TIMESTAMPTZ DEFAULT now()
);

CREATE INDEX idx_signals_symbol ON signals(symbol);
CREATE INDEX idx_signals_timestamp ON signals(timestamp DESC);
CREATE INDEX idx_signals_ready ON signals(ready_for_execution);
```

### RF5: Tabla `portfolio` (Portfolio State)
```sql
CREATE TABLE portfolio (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  account_id UUID NOT NULL REFERENCES accounts(id) ON DELETE CASCADE,
  
  -- Portfolio Snapshot
  snapshot_date TIMESTAMPTZ DEFAULT now(),
  total_value DECIMAL(15, 2),
  cash_balance DECIMAL(15, 2),
  invested_value DECIMAL(15, 2),
  
  -- Positions
  open_positions INTEGER,
  winning_positions INTEGER,
  losing_positions INTEGER,
  
  -- Performance
  day_pnl DECIMAL(12, 2),
  day_pnl_percent DECIMAL(6, 2),
  week_pnl DECIMAL(12, 2),
  month_pnl DECIMAL(12, 2),
  ytd_pnl DECIMAL(12, 2),
  
  -- Statistics
  total_trades INTEGER,
  winning_trades INTEGER,
  losing_trades INTEGER,
  win_rate DECIMAL(5, 2),
  avg_win DECIMAL(12, 2),
  avg_loss DECIMAL(12, 2),
  largest_win DECIMAL(12, 2),
  largest_loss DECIMAL(12, 2),
  
  -- Risk Metrics
  max_drawdown DECIMAL(6, 2),
  sharpe_ratio DECIMAL(6, 2),
  sortino_ratio DECIMAL(6, 2),
  
  updated_at TIMESTAMPTZ DEFAULT now()
);

CREATE INDEX idx_portfolio_account ON portfolio(account_id);
```

### RF6: Tabla `settings` (User Settings)
```sql
CREATE TABLE settings (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  
  -- Trading Settings
  max_position_size_percent DECIMAL(5, 2) DEFAULT 5,
  max_risk_per_trade DECIMAL(5, 2) DEFAULT 2,
  min_win_rate DECIMAL(5, 2) DEFAULT 50,
  
  -- Notification
  notify_on_signal BOOLEAN DEFAULT true,
  notify_on_trade BOOLEAN DEFAULT true,
  notify_on_tp BOOLEAN DEFAULT true,
  notify_on_sl BOOLEAN DEFAULT true,
  
  -- API Settings
  auto_execute_enabled BOOLEAN DEFAULT false,
  auto_execute_min_confidence DECIMAL(3, 2) DEFAULT 0.75,
  
  -- Features
  use_ai_synthesis BOOLEAN DEFAULT true,
  monitor_options BOOLEAN DEFAULT true,
  monitor_earnings BOOLEAN DEFAULT true,
  
  -- UI Preferences
  dark_mode BOOLEAN DEFAULT true,
  chart_default_timeframe VARCHAR(5) DEFAULT '15m',
  
  updated_at TIMESTAMPTZ DEFAULT now()
);

CREATE INDEX idx_settings_user ON settings(user_id);
```

---

## Row Level Security (RLS)

```sql
-- Users pueden acceder solo su propios datos
ALTER TABLE users ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can read own data"
  ON users FOR SELECT
  USING (auth.uid() = auth_user_id);

CREATE POLICY "Users can update own data"
  ON users FOR UPDATE
  USING (auth.uid() = auth_user_id);

-- Similar para accounts, trades, portfolio, settings
-- [Implementar para cada tabla]
```

---

## Migrations

```sql
-- Crear en Supabase Dashboard o via Supabase CLI:
-- supabase db push

-- Ver: supabase/migrations/20260317_initial_schema.sql
```

---

## Acceptance Criteria

- [ ] Schema PostgreSQL 100% creado en Supabase
- [ ] Todas las tablas con indices para queries frecuentes
- [ ] RLS policies implementadas (acceso controlado por usuario)
- [ ] Foreign keys y constraints validados
- [ ] Sample data insertado (test records)
- [ ] Documentation de schema (.md file)
- [ ] FIC comments (EN/ES) en DDL statements

---

## Testing

```typescript
// Tests en supabase/tests/schema.test.ts
describe('Database Schema', () => {
  it('Should create a new user', async () => {
    // SQL insert → verify
  });
  
  it('Should enforce RLS policies', async () => {
    // Try para acessar otro user data → fail
  });
  
  it('Should cascade delete trades cuando account deleted', async () => {
    // Delete account → verify trades also gone
  });
});
```

---

## Dependencias

- Supabase project creado
- Supabase CLI instalado
- PostgreSQL 13+ (managed por Supabase)

---

## Notas

- Todas las PIIs cifradas via Vault
- Timestamps en UTC (TIMESTAMPTZ)
- NULLs permitidos solo donde logical (exit_price NULL si trade aún abierto)

---

**Checklist**:
- [ ] Schema completamente definido
- [ ] Indices creados para performance
- [ ] Tests unitarios pasando
- [ ] Documentation actualizado
- [ ] Deploy a Supabase producción
