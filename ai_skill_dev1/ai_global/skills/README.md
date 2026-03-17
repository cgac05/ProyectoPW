# 🎯 Skills Globales - Catálogo de Habilidades

> Catálogo de skills (habilidades) reutilizables que cada agente posee y puede usar en múltiples proyectos.

**Versión**: 2.2  
**Última actualización**: March 17, 2026

---

## 📋 Estructura de Skills

```
skills/
├── README.md (este archivo)
├── ticket_analyzer/
├── architecture_designer/
├── requirement_validator/
├── knowledge_synthesizer/
├── database_schema_designer/
├── database_migrator/
├── database_connector/
├── react_code_generator/
├── typescript_code_generator/
├── vite_code_generator/
├── broker_api_integrator/
├── documentation_writer/
├── dependency_manager/
├── code_structure_organizer/
├── code_optimizer/
├── performance_analyzer/
├── security_auditor/
├── pattern_refactorer/
├── test_case_generator/
├── bug_detector/
├── quality_validator/
└── regression_tester/
```

---

## 🎯 Skills por Agente

### 🧠 @picoro — Analista/Arquitecto

| Skill | Descripción | Uso |
|-------|-------------|-----|
| `ticket_analyzer` | Leer y analizar tickets para extender comprensión | Validar tickets antes de diseño |
| `architecture_designer` | Diseñar componentes, flujos y estructura | Diseñar PWA + API |
| `requirement_validator` | Validar requisitos vs SPEC | Validar completitud |
| `knowledge_synthesizer` | Generar investigaciones profundas | Crear knowledge base |

---

### 🗄️ @krillin — Especialista BD

| Skill | Descripción | Uso |
|-------|-------------|-----|
| `database_schema_designer` | Diseñar esquemas por motor (Supabase, MongoDB, PostgreSQL, etc.) | Crear modelos en `data/` y traducir a persistencia real |
| `database_migrator` | Crear y ejecutar migraciones versionadas | Aplicar cambios a BD en DEV/PROD |
| `database_connector` | Implementar capas de acceso a datos (ORM/ODM) | Crear servicios de datos en REST API |

---

### 👨‍💻 @goku — Dev Senior #1

| Skill | Descripción | Uso |
|-------|-------------|-----|
| `react_code_generator` | Generar componentes React con Atomic Design | Crear UI interactiva en PWA |
| `typescript_code_generator` | Generar servicios y lógica TypeScript | Implementar lógica de negocio |
| `vite_code_generator` | Configurar y optimizar Vite | Setup inicial PWA |
| `broker_api_integrator` | Integrar con APIs de brokers (IBKR, Alpaca, etc.) | Conectar a mercado real |
| `documentation_writer` | Escribir comentarios y documentación inline | Documentar según estándar `FIC` |
| `dependency_manager` | Gestionar dependencias y versions | Mantener package.json |
| `code_structure_organizer` | Organizar archivos en estructura correcta | Mantener coherencia en `src/` |

---

### 🥷 @vegeta — Optimizador/Seguridad

| Skill | Descripción | Uso |
|-------|-------------|-----|
| `code_optimizer` | Optimizar para performance y memory | Reducir latencia en feeds |
| `performance_analyzer` | Medir y reportar bottlenecks | Identificar donde optimizar |
| `security_auditor` | Auditar seguridad de código | Validar credenciales no en código |
| `pattern_refactorer` | Refactorizar duplicación de código | Mejorar mantenibilidad |

---

### 🧪 @bulma — QA Tester

| Skill | Descripción | Uso |
|-------|-------------|-----|
| `test_case_generator` | Crear tests unitarios e integración | Validar funcionalidad |
| `bug_detector` | Encontrar bugs y comportamientos inesperados | QA |
| `quality_validator` | Validar que cumple criterios de aceptación | Sign-off de features |
| `regression_tester` | Validar que cambios no rompen lo anterior | CI/CD validation |

---

## 🏆 Matríz de Skills Globales

| # | Skill | Agente | Categoría | Estado | Ubicación |
|---|-------|--------|-----------|--------|-----------|
| 1 | ticket_analyzer | @picoro | analysis | ✅ Definido | local/definitions/ |
| 2 | architecture_designer | @picoro | design | ✅ Definido | local/definitions/ |
| 3 | requirement_validator | @picoro | validation | ✅ Definido | local/definitions/ |
| 4 | knowledge_synthesizer | @picoro | documentation | ✅ Definido | local/definitions/ |
| 5 | database_schema_designer | @krillin | database | ✅ Definido | local/definitions/ |
| 6 | database_migrator | @krillin | database | ✅ Definido | local/definitions/ |
| 7 | database_connector | @krillin | database | ✅ Definido | local/definitions/ |
| 8 | react_code_generator | @goku | frontend | ✅ Definido | local/definitions/ |
| 9 | typescript_code_generator | @goku | backend | ✅ Definido | local/definitions/ |
| 10 | vite_code_generator | @goku | tooling | ✅ Definido | local/definitions/ |
| 11 | broker_api_integrator | @goku | integration | ✅ Definido | local/definitions/ |
| 12 | documentation_writer | @goku | documentation | ✅ Definido | local/definitions/ |
| 13 | dependency_manager | @goku | maintenance | ✅ Definido | local/definitions/ |
| 14 | code_structure_organizer | @goku | organization | ✅ Definido | local/definitions/ |
| 15 | code_optimizer | @vegeta | optimization | ✅ Definido | local/definitions/ |
| 16 | performance_analyzer | @vegeta | analysis | ✅ Definido | local/definitions/ |
| 17 | security_auditor | @vegeta | security | ✅ Definido | local/definitions/ |
| 18 | pattern_refactorer | @vegeta | refactoring | ✅ Definido | local/definitions/ |
| 19 | test_case_generator | @bulma | testing | ✅ Definido | local/definitions/ |
| 20 | bug_detector | @bulma | testing | ✅ Definido | local/definitions/ |
| 21 | quality_validator | @bulma | validation | ✅ Definido | local/definitions/ |
| 22 | regression_tester | @bulma | testing | ✅ Definido | local/definitions/ |

---

## 🚀 Cómo Usar Skills

### Global vs Local

**Global** (`ai_global/skills/`):
- Reutilizable en múltiples proyectos
- Mantiene el catálogo central de habilidades
- Se referencia desde proyectos pero no se modifica

**Local** (`projects/<categoria>/<proyecto>/ai_work_flow/skills/`):
- Extensiones o especializaciones del skill global
- Modificaciones específicas del proyecto
- Hereda del global pero personaliza comportamiento

### Asignación en Workflow

En `projects/<categoria>/<proyecto>/ai_work_flow/development/workflow_agents.yaml`:

```yaml
# Asignar skill a agente para ejecutar tarea específica
agents:
  picoro:
    assigned_skills:
      - skill: architecture_designer
        context: "Diseñar arquitectura del módulo de inversiones"
        input:
          specification: "SPECIFICATION.md"
        output:
          - knowledge/local/03_architecture_design.md
          - config.yaml para módulo
```

---

## 📊 Estado Actual de Skills

| Categoría | Total | Definidos | En desarrollo |
|-----------|-------|-----------|---|
| **Analysis** | 2 | 2 | 0 |
| **Design** | 1 | 1 | 0 |
| **Database** | 3 | 3 | 0 |
| **Frontend** | 1 | 1 | 0 |
| **Backend** | 1 | 1 | 0 |
| **Tooling** | 1 | 1 | 0 |
| **Integration** | 1 | 1 | 0 |
| **Optimization** | 1 | 1 | 0 |
| **Documentation** | 2 | 2 | 0 |
| **Testing** | 4 | 4 | 0 |
| **Validation** | 2 | 2 | 0 |
| **Maintenance** | 1 | 1 | 0 |
| **Organization** | 1 | 1 | 0 |
| **Security** | 1 | 1 | 0 |
| **Refactoring** | 1 | 1 | 0 |
| **TOTAL** | **22** | **22** | **0** |

---

## 🔗 Referencias

- **Agentes**: [../agents/README.md](../agents/README.md)
- **Metodología**: Sección 3.2 de AI_SKILL_DEVELOPMENT_METHODOLOGY.md
- **Templates**: [../templates/SKILL_TEMPLATE.md](../templates/SKILL_TEMPLATE.md)

---

**Mantenedor**: Dr. Francisco Ibarra Carlos  
**Versión**: 2.2  
**Última actualización**: March 17, 2026  
**Total Skills**: 22 (todos operativos)
