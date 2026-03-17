# 📂 Archivos de datos (Contratos de Referencia)

> Contratos de datos que la PWA usa como referencia. NO son la persistencia real.
> La persistencia real vive en `projects/api/rest_api_inversions_drfic/`.

---

## 📋 Estructura

```
data/
└── supabase/                    # Motor BD seleccionado
    ├── models/                  # TypeScript interfaces/types
    │   ├── Strategy.ts          # Strategy configuration
    │   ├── PriceCandle.ts       # OHLCV candle
    │   ├── Indicator.ts         # Calculated indicators
    │   ├── Signal.ts            # Buy/sell signals
    │   ├── Trade.ts             # Executed trades
    │   └── Portfolio.ts         # User positions
    ├── schema/                  # SQL definitions
    │   └── schema.sql
    └── data/                    # Seed data / fixtures
        └── seeds.ts
```

---

## 🎯 Propósito

Estos archivos:
- ✅ Sirven como contrato para la PWA (src/)
- ✅ Se traducen a persistencia real por @krillin
- ✅ NO contienen datos reales ni persistencia
- ✅ Son referencia de estructura

---

## 🔄 Flujo

```
data/<motor>/models/ (contrato referencia)
        ↓
  @krillin traduce
        ↓
rest_api/src/models/ (persistencia real)
        ↓
rest_api/src/services/ (CRUD)
        ↓
REST API endpoints ← @goku integra en PWA
```

---

**Motor seleccionado**: Awaiting DATABASE SELECTION GATE  
**Estado**: 🟡 Pending specification
