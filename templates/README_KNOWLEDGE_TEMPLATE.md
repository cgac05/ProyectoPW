# 📚 Knowledge - [Nombre del Proyecto]

> Template para README principal de knowledge de proyectos.  
> Ubicación: `ai_skill_dev1/projects/<category>/<project_code>/knowledge/README.md`

---

## 📋 Estado Actual

| Aspecto | Estado | Última Actualización |
|---------|--------|----------------------|
| **Investigación Local** | 🚧 Pendiente / ✅ Generado | YYYY-MM-DD HH:MM |
| **Referencias Externas** | 🚧 Pendiente / ✅ Generado | YYYY-MM-DD HH:MM |
| **Tickets Generados** | ⏳ No iniciado / ✅ Completado | YYYY-MM-DD HH:MM |
| **Fase del Proyecto** | FASE 1 / FASE 2 / FASE 3 | - |

**Propósito**: El estado actual permite a IA y desarrolladores saber rápidamente en qué fase está el proyecto.

---

## 📖 Descripción del Proyecto

**Código**: `<project_code>`  
**Categoría**: `<rpa|web|data|ai>`  
**Descripción**: [Descripción breve de 1-2 líneas]

**Objetivo del Knowledge**: Documentar decisiones técnicas, patrones, investigaciones y referencias que informan la implementación del proyecto.

---

## 🏗️ Estructura de Conocimiento

```
knowledge/
├── README.md (este archivo)
├── local/
│   ├── README.md
│   ├── 01_<topic>_research.md
│   ├── 02_<topic>_patterns.md
│   ├── 03_<topic>_decisions.md
│   ├── 04_<topic>_strategy.md
│   ├── 05_<topic>_approach.md
│   └── lesson_*.md (durante desarrollo)
└── remote/
    ├── README.md
    ├── internal_reference_*.md
    ├── <library>_documentation.md
    ├── <api>_reference.md
    └── notebooklm_*.md (opcional)
```

---

## 📊 Métricas de Conocimiento

| Métrica | Valor |
|---------|-------|
| Archivos de investigación | 0 |
| Lecciones aprendidas | 0 |
| Referencias externas | 0 |
| NotebookLM creados | 0 |
| Tickets informados | 0 |

**Actualizar**: Después de cada generación de conocimiento

---

## 📅 Historial de Estado

| Fecha | Hora | Estado Anterior | Estado Nuevo | Evento | Notas |
|-------|------|-----------------|--------------|--------|-------|
| YYYY-MM-DD | HH:MM | - | 🚧 Estructura creada | Setup inicial | Directorios knowledge/ creados |
| YYYY-MM-DD | HH:MM | 🚧 Estructura | ✅ Knowledge generado | Investigación completada | X archivos local/ + Y archivos remote/ |
| YYYY-MM-DD | HH:MM | ✅ Knowledge | ⏳ Tickets generados | Planificación completada | Z tickets creados informados por knowledge |

**Cómo interpretar**:
- **Estado Actual** (arriba): Para que IA/metodología sepan fase actual del proyecto
- **Historial**: Auditoría completa de cambios con contexto temporal
- **Eventos importantes**: Setup, generación de knowledge, tickets, inicio desarrollo

**Convenciones**:
- 🚧 Pendiente: Trabajo no iniciado o en progreso
- ✅ Generado/Completado: Trabajo finalizado y validado
- ⏳ En proceso: Trabajo activo en este momento
- ❌ Bloqueado: Trabajo detenido por dependencias

---

## 📁 Contenido del Conocimiento

### 🔍 Knowledge Local (Investigaciones)

Ver: [knowledge/local/README.md](./local/README.md)

**Propósito**: Investigación profunda generada por IA **ANTES** de implementación

**Archivos Generados**:
- [ ] `01_<topic>_research.md` - Investigación técnica de opciones
- [ ] `02_<topic>_patterns.md` - Patrones y mejores prácticas
- [ ] `03_<topic>_decisions.md` - Decisiones arquitectónicas justificadas
- [ ] `04_<topic>_strategy.md` - Estrategia de implementación
- [ ] `05_<topic>_approach.md` - Enfoque técnico recomendado

**Estado**: 🚧 Pendiente de generación / ✅ X archivos generados

---

### 🌐 Knowledge Remote (Referencias)

Ver: [knowledge/remote/README.md](./remote/README.md)

**Propósito**: Referencias externas + código interno que sustenta decisiones

**Tipos de Referencias**:
- 📘 Documentación oficial (APIs, librerías)
- 📄 Código interno reutilizable (internal_reference_*.md)
- 🧠 NotebookLM (análisis profundo opcional)

**Estado**: 🚧 Pendiente / ✅ Y archivos generados

---

## 🎯 Aplicación del Conocimiento

### Principio: Knowledge Before Implementation

El conocimiento generado en este directorio **informa** la creación de tickets:

1. **IA analiza** SPECIFICATION.md
2. **IA genera** investigaciones profundas (knowledge/local/)
3. **IA documenta** referencias externas/internas (knowledge/remote/)
4. **IA crea** tickets informados por el conocimiento
5. **Equipo implementa** siguiendo decisiones documentadas

**Beneficios**:
- ✅ Evita re-investigar durante desarrollo
- ✅ Decisiones técnicas pre-justificadas
- ✅ Tickets con contexto completo
- ✅ Onboarding rápido de nuevos desarrolladores
- ✅ Trazabilidad de decisiones arquitectónicas

---

## 🔗 Enlaces Rápidos

### Documentación del Proyecto
- [SPECIFICATION.md](../docs/specs/SPECIFICATION.md) - Especificación técnica completa
- [README del Proyecto](../README.md) - Información general
- [Tickets](../tickets/README.md) - Plan de implementación

### Knowledge
- [Local Knowledge](./local/README.md) - Investigaciones internas
- [Remote Knowledge](./remote/README.md) - Referencias externas

### Metodología
- [AI Skill Development](../AI_SKILL_DEVELOPMENT_METHODOLOGY.md) - Metodología completa
- [Master Guide](../../../_drfic_ai_skill_dev_teaching/MASTER_GUIDE.md) - Guía de uso

---

## 🚀 Próximos Pasos

### Si Knowledge NO está generado (🚧)
**Acción**: Ejecutar investigación profunda con IA

**Prompt sugerido**:
```
Lee SPECIFICATION.md del proyecto <project_code> y genera:
1. Investigaciones profundas en knowledge/local/
2. Referencias externas en knowledge/remote/
3. Actualiza este README con métricas y historial

Usa templates de KNOWLEDGE_NOTE_TEMPLATE.md
```

**Ver**: [MASTER_GUIDE.md - Paso 4.2.5](../../_drfic_ai_skill_dev_teaching/MASTER_GUIDE.md)

---

### Si Knowledge está generado (✅)
**Acción**: Generar tickets informados por el conocimiento

**Prompt sugerido**:
```
Tengo knowledge completo generado. Ahora crea tickets
en tickets/ informados por las investigaciones.

Cada ticket debe referenciar:
- Archivos de knowledge/local/ relevantes
- Referencias de knowledge/remote/ aplicables
- Decisiones técnicas ya tomadas
```

---

## 📝 Notas de Mantenimiento

### Cuándo Actualizar este README

**Obligatorio**:
- ✅ Después de generar archivos de investigación
- ✅ Al completar referencias externas
- ✅ Cuando se generan tickets del proyecto
- ✅ Al cambiar de FASE (1 → 2 → 3)

**Opcional**:
- Durante desarrollo si surgen insights importantes
- Al agregar NotebookLM
- Para documentar decisiones arquitectónicas mayores

### Formato del Historial

**Estructura**:
```
| Fecha      | Hora  | Anterior     | Nuevo        | Evento         | Notas                    |
|------------|-------|--------------|--------------|----------------|--------------------------|
| 2026-02-23 | 15:20 | 🚧 Pendiente | ✅ Generado  | Investigación  | 6 local + 9 remote       |
```

**Campo "Evento"**: Describe QUÉ pasó (Setup, Investigación, Tickets, etc.)  
**Campo "Notas"**: Contexto adicional relevante (cantidad archivos, decisiones tomadas, etc.)

---

## 🤝 Contribución

Este knowledge es colaborativo:
- **IA**: Genera investigación inicial automatizada
- **Equipo**: Agrega lecciones durante desarrollo
- **Ambos**: Mantienen referencias actualizadas

**Principio**: Documentar **mientras** se trabaja, no después.

---

**Proyecto**: `<project_code>`  
**Última actualización**: `YYYY-MM-DD HH:MM`  
**Mantenido por**: [Nombre del responsable]  
**Template version**: 1.0.0
