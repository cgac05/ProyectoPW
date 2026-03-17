# 🧪 Tests — Proyecto pwa_inversions_drfic

> Pruebas unitarias, integración y end-to-end.

---

## 📁 Estructura

```
tests/
├── unit/                   # Pruebas unitarias (funciones aisladas)
│   ├── services/
│   ├── utils/
│   └── hooks/
├── integration/            # Pruebas de integración (componentes + servicios)
│   ├── broker-connection.test.ts
│   └── signal-generator.test.ts
└── e2e/                    # Pruebas end-to-end (usuario completo)
    ├── dashboard.e2e.ts
    └── trading-flow.e2e.ts
```

---

## 🧪 Framework

- **Jest** o **Vitest** para unit + integration tests
- **Cypress** o **Playwright** para E2E tests

---

## 🎯 Prioridades de Testing

1. **Cálculos matemáticos** (RSI, MACD, Bollinger)
2. **Conexión a broker** (conectar, desconectar, manejar errores)
3. **Generación de señales** (validar precisión vs histórico)
4. **Portafolio calculations** (P&L, riesgo)
5. **UI interactions** (click, drag, form submission)

---

## 🚀 Ejecución

```bash
# Unit + Integration
npm test

# E2E
npm run test:e2e

# Coverage
npm run test:coverage
```

---

**Proyecto**: pwa_inversions_drfic  
**Estado**: 🟡 Awaiting @bulma (FASE 3)
