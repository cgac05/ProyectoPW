# 🤖 Agentes de Desarrollo - Sistema ai_skill_dev1

> 5 agentes autónomos que trabajan en paralelo para desarrollar proyectos usando la metodología **AI SKILL DEVELOPMENT**.

**Versión**: 2.2  
**Última actualización**: March 17, 2026

---

## 📋 Índice de Agentes

| Agente | Rol | Fase Activa | Skills |
|--------|-----|-------------|--------|
| 🧠 **@picoro** | Analista/Arquitecto/Orquestador | FASE 2.3-2.4 | ticket_analyzer, architecture_designer, requirement_validator, knowledge_synthesizer |
| 🗄️ **@krillin** | Especialista en Base de Datos | FASE 2.4-3 | database_schema_designer, database_migrator, database_connector |
| 👨‍💻 **@goku** | Dev Senior #1 | FASE 2.4-3 | react_code_generator, typescript_code_generator, vite_code_generator, broker_api_integrator |
| 🥷 **@vegeta** | Optimizador/Seguridad | FASE 3 | code_optimizer, performance_analyzer, security_auditor, pattern_refactorer |
| 🧪 **@bulma** | QA Tester | FASE 3 | test_case_generator, bug_detector, quality_validator, regression_tester |

---

## 🧠 @picoro — Analista/Arquitecto/Orquestador

**Descripción**: Analista técnico que entiende requisitos, diseña arquitectura y coordina el equipo.

**Responsabilidades**:
- Leer SPECIFICATION.md y validar contra metodología
- Diseñar arquitectura de componentes
- Investigar APIs, brokers, librerías necesarias
- Generar knowledge base para otros agentes
- Crear tickets informados para Goku, Vegeta y Bulma
- Comunicar gaps y riesgos

**Cuándo actúa**:
- 🟡 FASE 2.3: Investigación profunda
- 🟡 FASE 2.4: Diseño de arquitectura
- 🟢 FASE 2.4-3: Validación de tickets antes de Goku

**Skills**:
- `ticket_analyzer`: Leer y analizar tickets
- `architecture_designer`: Diseñar componentes y flujos
- `requirement_validator`: Validar contra SPEC
- `knowledge_synthesizer`: Generar investigaciones profundas

**Ver**: [fic_picoro_agent_orchestrator.md](fic_picoro_agent_orchestrator.md)

---

## 🗄️ @krillin — Especialista en Base de Datos

**Descripción**: Especialista en diseño de persistencia, migraciones y conexión a bases de datos.

**Responsabilidades**:
- Traducir contratos de datos PWA a esquemas reales
- Diseñar modelos por motor seleccionado (Supabase, MongoDB, PostgreSQL, etc.)
- Implementar ORM/ODM en rest_api_inversions_drfic
- Ejecutar migraciones en forma segura
- Exponer servicios de datos a Goku
- Validar integridad referencial y reglas de negocio

**Cuándo actúa**:
- 🟡 FASE 2.4: Diseño de esquemas
- 🟢 FASE 3: Implementación de persistencia (paralelo a Goku)

**Skills**:
- `database_schema_designer`: Diseñar esquemas por motor
- `database_migrator`: Crear y ejecutar migraciones
- `database_connector`: Implementar capas de acceso a datos

**Motores soportados**: Supabase, MongoDB, PostgreSQL, MySQL, SQLite, Firebase

**Ver**: [fic_krillin_agent_db.md](fic_krillin_agent_db.md)

---

## 👨‍💻 @goku — Dev Senior #1

**Descripción**: Programador experimentado que implementa componentes React, servicios TypeScript e integraciones.

**Responsabilidades**:
- Implementar componentes React/TypeScript en `src/`
- Crear servicios para brokers, indicadores, trading
- Integrar con Supabase/APIs de Krillin
- Implementar lógica de señales de compra/venta
- Documentar código con estándar `FIC` (EN/ES)
- Comunicar qué está bloqueado o necesita de otros agentes

**Cuándo actúa**:
- 🟡 FASE 2.4: Crear estructura base
- 🟢 FASE 3: Implementar módulos de trading

**Skills**:
- `react_code_generator`: Componentes React
- `typescript_code_generator`: Servicios TypeScript
- `vite_code_generator`: Configuración Vite
- `broker_api_integrator`: Integración con brokers
- `documentation_writer`: Comentarios y READMEs
- `dependency_manager`: Gestión de dependencias

**Estándar de documentación**:
```typescript
// FIC: Calculate RSI indicator (EN)
// FIC: Calcular indicador RSI con período 14 (ES)
export const calculateRSI = (closes: number[], period: number = 14): number => {
  // ...
};
```

**Ver**: [fic_goku_agent_dev1.md](fic_goku_agent_dev1.md)

---

## 🥷 @vegeta — Optimizador/Seguridad

**Descripción**: Especialista en optimización de performance, seguridad y patrones de código.

**Responsabilidades**:
- Optimizar latencia en feeds de datos de mercado
- Auditar seguridad de credenciales y conexiones
- Refactorizar patrones repetitivos
- Validar que no haya memory leaks
- Sugerir mejoras de UX/performance

**Cuándo actúa**:
- 🟢 FASE 3: Después de que Goku implementa

**Skills**:
- `code_optimizer`: Optimizar para performance
- `performance_analyzer`: Medir y reportar latencia
- `security_auditor`: Auditar seguridad
- `pattern_refactorer`: Refactorizar duplicación

**Ver**: [fic_vegeta_agent_dev2.md](fic_vegeta_agent_dev2.md)

---

## 🧪 @bulma — QA Tester

**Descripción**: Especialista en testing que valida que el código funciona correctamente.

**Responsabilidades**:
- Crear tests unitarios para funciones clave
- Crear tests de integración para módulos completos
- Validar cálculos de indicadores contra TradingView
- Validar precisión de señales de compra/venta
- Buscar bugs y comportamientos inesperados
- Documentar casos de prueba

**Cuándo actúa**:
- 🟢 FASE 3: Después de que Goku y Vegeta terminan

**Skills**:
- `test_case_generator`: Crear tests
- `bug_detector`: Encontrar bugs
- `quality_validator`: Validar calidad
- `regression_tester`: Validar regresiones

**Framework**: Jest, Vitest o equivalente

**Ver**: [fic_bulma_agent_tester1.md](fic_bulma_agent_tester1.md)

---

## 🔄 Ciclo Completo de Desarrollo

```
┌─────────────────────┐
│ FASE 2.3            │
│ Picoro investiga    │
└──────────┬──────────┘
           ↓
┌─────────────────────────────────────┐
│ FASE 2.4                            │
│ Picoro diseña + Krillin esquemas    │
│ Picoro → crea tickets               │
└──────────┬──────────────────────────┘
           ↓
       ┌───┴────┐
       ↓        ↓
    ┌────┐  ┌────────┐
    │Goku│  │Krillin │
    └─┬──┘  └───┬────┘
      │         │
      └────┬────┘
           ↓
    ┌──────────────┐
    │  FASE 3.1    │
    │ Goku imple.  │
    │ Krillin BD   │
    └──────┬───────┘
           ↓
    ┌──────────────┐
    │  FASE 3.2    │
    │ Vegeta optim │
    └──────┬───────┘
           ↓
    ┌──────────────┐
    │  FASE 3.3    │
    │ Bulma tests  │
    └──────┬───────┘
           ↓
      ✅ MÓDULO LISTO
```

---

## 📊 Estado Actual

| Agente | Documentación | Estado |
|--------|---------------|--------|
| 🧠 @picoro | ✅ Completa | Listo |
| 🗄️ @krillin | ✅ Completa | Listo |
| 👨‍💻 @goku | ✅ Completa | Listo |
| 🥷 @vegeta | ✅ Completa | Listo |
| 🧪 @bulma | ✅ Completa | Listo |

---

## 🎯 Cuándo Invocas Cada Agente

**@picoro**:
```
"@picoro analiza esta SPEC de inversiones y diseña la arquitectura"
"@picoro genera el conocimiento necesario para los otros agentes"
```

**@krillin**:
```
"@krillin recibe el contrato de datos y genera el modelo Supabase"
"@krillin implementa las migraciones y servicios de BD"
```

**@goku**:
```
"@goku implementa el componente Dashboard usando contratos de datos"
"@goku integra con los servicios de Krillin y los brokers"
```

**@vegeta**:
```
"@vegeta optimiza la latencia en los feeds de datos"
"@vegeta audita la seguridad de las credenciales"
```

**@bulma**:
```
"@bulma crea tests para el módulo de indicadores"
"@bulma valida que los cálculos de RSI sean correctos"
```

---

**Mantenedor**: Dr. Francisco Ibarra Carlos  
**Versión**: 2.2  
**Última actualización**: March 17, 2026
