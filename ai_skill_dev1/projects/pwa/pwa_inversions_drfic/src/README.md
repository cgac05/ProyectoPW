# 🏗️ Código Fuente — PWA React/TypeScript

> Código ejecutable de la aplicación web.

---

## 📁 Estructura Estándar (src-first)

```
src/
├── assets/                 # Imágenes, fuentes, estilos globales
├── components/
│   └── ui/                 # Atomic Design: atoms, molecules, organisms
│       ├── atoms/          # BadButton, Input, Badge
│       ├── molecules/      # SignalCard, PortfolioRow
│       └── organisms/      # Dashboard, WatchlistPanel
├── features/               # Módulos funcionales
│   ├── dashboard/          # Dashboard principal
│   ├── market-scanner/     # Escáner de mercado
│   ├── signals/            # Motor de señales
│   ├── portfolio/          # Gestión de portafolio
│   ├── broker-connect/     # Conexión con brokers
│   ├── options-chain/      # Cadena de opciones
│   ├── backtesting/        # Backtesting de estrategias
│   └── alerts/             # Sistema de alertas
├── hooks/                  # Custom hooks
│   ├── useMarketData.ts
│   ├── useBrokerConnection.ts
│   └── useSignals.ts
├── layouts/                # Layouts generales
│   └── TradingLayout.tsx
├── pages/                  # Páginas principales
│   ├── DashboardPage.tsx
│   ├── SignalsPage.tsx
│   └── SettingsPage.tsx
├── routes/                 # Configuración de rutas
├── services/               # Servicios externos
│   ├── broker/             # Integración con brokers
│   │   ├── ibkr_connector.ts
│   │   └── alpaca_connector.ts
│   ├── market-data/        # Feeds de datos
│   ├── indicators/         # Motor de indicadores
│   │   ├── rsi.service.ts
│   │   ├── macd.service.ts
│   │   └── bollinger.service.ts
│   ├── ai-analysis/        # Análisis con IA/Claude
│   ├── news/               # Servicio de noticias
│   └── api/                # Cliente REST API
├── store/                  # Estado global (Zustand/Redux)
│   ├── marketStore.ts      # Precios, indicadores en tiempo real
│   ├── portfolioStore.ts   # Posiciones del usuario
│   └── signalsStore.ts     # Señales generadas
├── styles/                 # Estilos globales
│   └── globals.css
├── types/                  # Tipos TypeScript globales
│   ├── market.types.ts
│   ├── trade.types.ts
│   └── signal.types.ts
├── utils/                  # Funciones utilitarias
│   ├── calculateRSI.ts
│   ├── formatCurrency.ts
│   └── validators.ts
├── App.tsx                 # Componente raíz
└── main.tsx                # Punto de entrada
```

---

## 📚 Convenciones

### Nombres de Archivos
- `ComponentName.tsx` — Componentes React
- `serviceName.ts` — Servicios, utilidades
- `useName.ts` — Custom hooks
- `name.types.ts` — Definiciones de tipos

### Estructura de Componentes
```typescript
// ComponentName.tsx
interface Props {
  prop1: string;
  prop2?: number;
}

// FIC: ComponentName description (EN)
// FIC: Descripción componente (ES)
export const ComponentName: React.FC<Props> = ({ prop1, prop2 }) => {
  return (
    // ...
  );
};
```

### Estándar FIC Obligatorio
- ✅ Todo exported component, function, hook
- ✅ Comentarios EN/ES
- ✅ Ubicación: justo antes de export

---

## 🚀 Primeros Pasos

1. ✅ Estructura creada (skeleton)
2. 🟡 @goku configura Vite base (TKT-INVRFIC-001)
3. 🟡 @goku crea componentes principales
4. 🟡 @goku integra servicios de broker
5. 🟡 @goku integra indicadores
6. ✅ @vegeta optimiza
7. ✅ @bulma testa

---

**Proyecto**: pwa_inversions_drfic  
**Framework**: React 18+, TypeScript, Vite  
**Estado**: 🟡 Awaiting implementation
