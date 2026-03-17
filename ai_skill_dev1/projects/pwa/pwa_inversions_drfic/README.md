# 📋 Plataforma de Inversiones con IA — Sistema de Señales de Trading

## Metadata

```yaml
project:
  code: pwa_inversions_drfic
  name: Plataforma PWA de Inversiones con IA
  category: pwa
  version: 0.1.0
  status: initial_setup
  
description: |
  Aplicación web para trading enfocada en análisis técnico y señales de compra/venta
  en la bolsa de EE.UU. Integración con brokers reales (Interactive Brokers, Alpaca)
  y cálculo de indicadores en tiempo real (RSI, MACD, Bollinger Bands).

date_initialized: 2026-03-17
implemented_by: Dr. Francisco Ibarra Carlos
methodology: AI SKILL DEVELOPMENT v2.2 + SPEC DRIVEN ASSISTANCE AI
```

---

## 🚀 Propósito del Proyecto

Crear una plataforma de inversiones moderna que:
- ✅ Se conecte a brokers reales (IBKR, Alpaca)
- ✅ Calcule indicadores técnicos en tiempo real
- ✅ Genere señales automáticas de compra/venta
- ✅ Permita backtesting de estrategias
- ✅ Gestione portafolio de forma profesional
- ✅ Integre análisis de opciones (Iron Condor, Straddle, etc.)

---

## 📁 Estructura de Proyecto

```
pwa_inversions_drfic/
├── ai_work_flow/                    # Artefactos de metodología
│   ├── development/
│   │   ├── workflow_agents.yaml     # Tareas por agente
│   │   └── README.md
│   ├── docs/
│   │   └── specs/
│   │       ├── SPECIFICATION.md     # Spec completa del proyecto
│   │       └── incremental/         # Specs incrementales
│   ├── knowledge/
│   │   ├── README.md
│   │   ├── local/                   # Investigaciones
│   │   └── remote/                  # Referencias
│   └── tickets/
│       └── README.md
│
├── data/                            # Contratos de datos (referencias)
│   └── supabase/
│       ├── models/                  # TypeScript interfaces
│       ├── schema/                  # SQL definitions
│       └── data/                    # Seed data / fixtures
│
├── public/
│
├── src/                             # Código ejecutable React/TypeScript
│   ├── assets/                      # Imágenes, fuentes
│   ├── components/
│   │   └── ui/                      # Atomic Design
│   ├── features/                    # Módulos funcionales (dashboard, signals, etc.)
│   ├── hooks/                       # Custom hooks
│   ├── layouts/
│   ├── pages/
│   ├── routes/
│   ├── services/                    # Brokers, market data, indicators
│   ├── store/                       # Zustand/Redux
│   ├── styles/
│   ├── types/
│   ├── utils/
│   ├── App.tsx
│   └── main.tsx
│
├── tests/
│   └── e2e/
│
├── index.html
├── package.json
├── tsconfig.json
├── vite.config.ts
└── .env.example
```

---

## 📋 Estado Actual

| Fase | Componente | Estado | Fecha |
|------|-----------|--------|-------|
| **FASE 0** | Sistema Global | ✅ Completada | 2026-03-17 |
| **FASE 1** | Agentes + Skills | ✅ Completada | 2026-03-17 |
| **FASE 2** | Este proyecto | 🚧 En progreso | 2026-03-17 |
| **FASE 2.1** | SPECIFICATION.md | 🟡 Pendiente | - |
| **FASE 2.2** | Database Selection | 🟡 Pendiente | - |
| **FASE 2.3** | Investigación (Picoro) | 🟡 Pendiente | - |
| **FASE 2.4** | Diseño + Estructura | 🟡 Pendiente | - |
| **FASE 3** | Implementación | 🟡 Pendiente | - |

---

## 🎯 Próximos Pasos

### FASE 2.1 — Especificación Completa
1. **Solicitar**: Specs detalladas del usuario
2. **Crear**: SPECIFICATION.md usando template
3. **Validar**: Picoro revisa completitud

### FASE 2.2 — Seleccionar Base de Datos
1. **Pregunta**: ¿Cuál(es) motor(es) de BD usaremos?
2. **Opciones**: Supabase, MongoDB, PostgreSQL, MySQL, SQLite, Firebase
3. **Gate**: DATABASE SELECTION GATE

### FASE 2.3 — Investigación Profunda
1. **@picoro**: Investiga APIs de brokers
2. **@picoro**: Investigar librerías de indicadores
3. **@picoro**: Definir estrategias de trading
4. **Deliverable**: knowledge/local/ completo

### FASE 2.4 — Diseño + Estructura
1. **@picoro**: Diseña arquitectura completa
2. **@krillin**: Diseña esquemas de BD
3. **@goku**: Crea estructura base con Vite
4. **Deliverable**: Proyecto estructurado, listo para implementar

### FASE 3 — Implementación
1. **@goku**: Implementa módulos
2. **@krillin**: Expone servicios de BD
3. **@vegeta**: Optimiza y audita seguridad
4. **@bulma**: Crea tests y valida
5. **Deliverable**: Plataforma funcional

---

## 🔗 Archivos Importantes

- [Metodología Global](../../AI_SKILL_DEVELOPMENT_METHODOLOGY.md)
- [Agentes](../../ai_global/agents/README.md)
- [Skills](../../ai_global/skills/README.md)
- [Templates](../../ai_global/templates/README.md)

---

## 📧 Contacto

**Project Owner**: [Tu nombre/correo]  
**Lead Architect**: Dr. Francisco Ibarra Carlos  
**Iniciado**: March 17, 2026

---

**Este proyecto está bajo la metodología AI SKILL DEVELOPMENT v2.2**  
**Última actualización**: March 17, 2026
