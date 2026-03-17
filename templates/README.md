# 📋 Templates

Templates para crear agentes, skills, tickets, conocimiento y configuraciones de proyectos.

---

## 📖 Propósito

Los templates aseguran:
- ✅ Consistencia en la documentación
- ✅ Calidad estandarizada
- ✅ Onboarding más rápido
- ✅ Nada importante se olvida
- ✅ Reutilización de mejores prácticas

---

## 📂 Templates Disponibles

### 📄 SPECIFICATION_TEMPLATE.md
**Uso**: Crear la especificación técnica ORIGINAL de un proyecto nuevo

**Incluye**:
- Metadata (código proyecto, ticket externo, solicitante, estado)
- Visión general (contexto, objetivo, flujo, resultado esperado)
- Entradas (parámetros, archivos, credenciales)
- Proceso completo paso a paso (lógica de negocio)
- Salidas (archivos generados, notificaciones)
- Requisitos técnicos (tecnologías, configuración, infraestructura)
- Casos de uso detallados
- Casos de prueba (criterios de aceptación)
- Arquitectura de componentes
- Skills de IA necesarios
- Decisiones técnicas pendientes
- Dependencias y restricciones
- Plan de implementación
- Riesgos y mitigación
- Aprobaciones y control de cambios

**Cuándo usar**: 
- Al iniciar un proyecto NUEVO desde cero
- Cuando el usuario te da un ticket externo para un nuevo desarrollo
- ANTES de que Picoro comience la investigación (FASE 2.1)

**Ubicación**: [SPECIFICATION_TEMPLATE.md](./SPECIFICATION_TEMPLATE.md)

**Nota**: Este es el documento que TÚ (desarrollador) creas después de recibir el ticket externo del usuario

---

### 🤖 AGENT_TEMPLATE.md
**Uso**: Documentar agentes (globales o de proyecto)

**Incluye**:
- Metadata del agente (nombre, versión, skills requeridos)
- Descripción y responsabilidades
- Configuración
- Ejemplos de uso
- API del agente
- Testing
- Troubleshooting

**Cuándo usar**: Al crear cualquier agente nuevo

**Ubicación**: [AGENT_TEMPLATE.md](./AGENT_TEMPLATE.md)

---

### 🎯 SKILL_TEMPLATE.md
**Uso**: Documentar skills (globales o de proyecto)

**Incluye**:
- Metadata del skill (versión, dependencias, I/O)
- Propósito y funcionalidad
- Instalación
- Ejemplos básicos y avanzados
- API completa
- Testing y performance
- Troubleshooting

**Cuándo usar**: Al crear cualquier skill nuevo

**Ubicación**: [SKILL_TEMPLATE.md](./SKILL_TEMPLATE.md)

---

### 🎫 TICKET_TEMPLATE.md
**Uso**: Crear tickets internos de desarrollo

**Incluye**:
- Metadata (ID, ticket externo, solicitante, prioridad, estado)
- Descripción y justificación
- Análisis de impacto
- Workflow de agentes (Picoro → Goku → Vegeta → Bulma)
- Criterios de aceptación
- Plan de testing
- Documentación requerida
- Sección de cierre con commit y ticket externo

**Cuándo usar**: 
- Para cada tarea de implementación derivada del diseño de Picoro
- Para cambios menores sobre proyectos existentes
- Para correcciones de bugs

**Ubicación**: [TICKET_TEMPLATE.md](./TICKET_TEMPLATE.md)

---

### 📄 SPEC_INCREMENTAL_TEMPLATE.md
**Uso**: Crear especificaciones para cambios GRANDES sobre proyectos existentes

**Incluye**:
- Metadata (ticket externo, solicitante, relación con SPEC original)
- Visión general y justificación
- Alcance del cambio e impacto
- Arquitectura (nuevos componentes, modificaciones)
- Input/Output nuevos
- Flujo de proceso modificado
- Requisitos técnicos y dependencias
- Investigación requerida
- Casos de uso y prueba
- Estimación de esfuerzo y tickets a generar
- Riesgos y mitigación
- Criterios de aceptación
- Aprobaciones

**Cuándo usar**: 
- Feature nueva que requiere nuevos servicios/módulos
- Cambios que afectan arquitectura existente
- Estimación > 20 horas
- Requiere investigación técnica nueva

**Ubicación**: [SPEC_INCREMENTAL_TEMPLATE.md](./SPEC_INCREMENTAL_TEMPLATE.md)

**Nota**: Para cambios pequeños (bug fix, mejora menor), crear ticket directo sin nueva SPEC

---

**Ubicación**: [SKILL_TEMPLATE.md](./SKILL_TEMPLATE.md)

---

### 🎫 TICKET_TEMPLATE.md
**Uso**: Crear tickets de control de cambios

**Incluye**:
- Metadata (tipo, prioridad, estado)
- Descripción y contexto
- Objetivos
- Archivos afectados
- Trazabilidad
- Criterios de aceptación
- Plan de testing
- Notas de desarrollo
- Review checklist

**Cuándo usar**: Para trackear cualquier cambio o feature

**Ubicación**: [TICKET_TEMPLATE.md](./TICKET_TEMPLATE.md)

---

### 📚 KNOWLEDGE_NOTE_TEMPLATE.md
**Uso**: Documentar conocimiento (local o remoto)

**Incluye**:
- Metadata y tags
- Propósito
- Contenido adaptable (lesson, solution, pattern, reference)
- Ejemplos de código
- Aplicabilidad
- Referencias
- Historial de cambios

**Cuándo usar**: Al documentar lecciones, soluciones, patrones o referencias

**Ubicación**: [KNOWLEDGE_NOTE_TEMPLATE.md](./KNOWLEDGE_NOTE_TEMPLATE.md)

---

### 📋 README_KNOWLEDGE_TEMPLATE.md
**Uso**: Crear README para directorios knowledge/ de proyectos

**Incluye**:
- Estado actual del conocimiento (para IA/metodología)
- Métricas de contenido generado
- Historial de estado con trazabilidad temporal
- Estructura de carpetas
- Enlaces a documentación relacionada
- Guía de próximos pasos

**Cuándo usar**: Al crear estructura de knowledge/ en nuevo proyecto

**Por qué es importante**:
- ✅ IA/metodología saben en qué fase está el proyecto
- ✅ Auditoría temporal de cuándo se generó conocimiento
- ✅ Trazabilidad de decisiones arquitectónicas
- ✅ Facilita onboarding de nuevos desarrolladores

**Ubicación**: [README_KNOWLEDGE_TEMPLATE.md](./README_KNOWLEDGE_TEMPLATE.md)

---

### ⚙️ PROJECT_CONFIG_TEMPLATE.yaml
**Uso**: Configurar nuevos proyectos

**Incluye**:
- Información del proyecto
- Dependencias (skills, agentes, packages)
- Configuración de entornos
- Ejecución y notificaciones
- Logging
- Paths
- Testing
- Versionado
- Integraciones

**Cuándo usar**: Al iniciar cualquier proyecto nuevo

**Ubicación**: [PROJECT_CONFIG_TEMPLATE.yaml](./PROJECT_CONFIG_TEMPLATE.yaml)

---

## 🚀 Cómo Usar los Templates

### Método 1: Copiar y Renombrar

```bash
# Para agente
cp ai_skill_dev1/ai_global/templates/AGENT_TEMPLATE.md ./mi_agente/README.md

# Para skill
cp ai_skill_dev1/ai_global/templates/SKILL_TEMPLATE.md ./mi_skill/README.md

# Para ticket
cp ai_skill_dev1/ai_global/templates/TICKET_TEMPLATE.md ./tickets/TKT-XXX-001.md

# Para knowledge note (archivo individual)
cp ai_skill_dev1/ai_global/templates/KNOWLEDGE_NOTE_TEMPLATE.md ./knowledge/local/lesson_algo.md

# Para README de knowledge (estructura del proyecto)
cp ai_skill_dev1/ai_global/templates/README_KNOWLEDGE_TEMPLATE.md ./knowledge/README.md

# Para config de proyecto
cp ai_skill_dev1/ai_global/templates/PROJECT_CONFIG_TEMPLATE.yaml ./config.yaml
```

### Método 2: Referencias Directas

En tu editor, abre el template relevante y úsalo como guía mientras creas tu documento.

---

## ✏️ Personalizar Templates

Los templates son **guías flexibles**, no camisas de fuerza:

### ✅ Está bien:
- Agregar secciones específicas a tu contexto
- Reorganizar para mayor claridad
- Omitir secciones no aplicables (marcándolas como "N/A")
- Adaptar ejemplos a tu caso de uso

### ❌ Evita:
- Eliminar metadata crítico (versión, tipo, fecha)
- Omitir secciones importantes sin justificación
- Cambiar formatos de manera inconsistente
- No documentar decisiones de personalización

---

## 🎯 Qué Template Usar

### Creando un Agente
→ **AGENT_TEMPLATE.md**

### Creando un Skill
→ **SKILL_TEMPLATE.md**

### Trackeando un Cambio
→ **TICKET_TEMPLATE.md**

### Documentando Conocimiento Individual
→ **KNOWLEDGE_NOTE_TEMPLATE.md** (lecciones, patrones, referencias)

### Creando Estructura de Knowledge
→ **README_KNOWLEDGE_TEMPLATE.md** (README de proyecto con historial)

### Configurando un Proyecto
→ **PROJECT_CONFIG_TEMPLATE.yaml**

---

## 📊 Secciones Clave por Template

### AGENT_TEMPLATE.md
```
1. Metadata (yaml)
2. Descripción
3. Skills Requeridos ⭐
4. Configuración
5. Uso (ejemplos)
6. Estructura de archivos
7. Flujo de ejecución
8. Testing
9. Troubleshooting
```

### SKILL_TEMPLATE.md
```
1. Metadata (yaml)
2. Descripción
3. Instalación
4. Uso (básico y avanzado) ⭐
5. API del skill ⭐
6. Testing
7. Performance
8. Integración
9. Versionado
```

### TICKET_TEMPLATE.md
```
1. Metadata
2. Descripción
3. Objetivos ⭐
4. Archivos afectados
5. Criterios de aceptación ⭐
6. Plan de testing
7. Documentación requerida
8. Notas de desarrollo
9. Review checklist
```

### KNOWLEDGE_NOTE_TEMPLATE.md
```
1. Metadata y tags
2. Propósito
3. Contenido principal ⭐
4. Ejemplos de código
5. Aplicabilidad ⭐
6. Referencias
7. Casos de uso reales
8. FAQ
```

⭐ = Secciones más críticas

---

## 💡 Best Practices

### Al Usar Templates

1. **Lee primero todo el template** antes de empezar a llenar
2. **Completa metadata desde el inicio** (versión, fecha, autor)
3. **No dejes secciones vacías** - Usa "N/A" o "Pendiente"
4. **Sé específico** en descripciones y ejemplos
5. **Mantén consistencia** con otros documentos del mismo tipo

### Al Mantener Templates

- ✅ Revisa templates cada trimestre
- ✅ Incorpora mejoras de experiencia real
- ✅ Documenta cambios importantes
- ✅ Mantén versionado de templates
- ✅ Comunica cambios al equipo

---

## 🔄 Evolución de Templates

Los templates evolucionan. Cuando encuentres mejoras:

### Paso 1: Crear Ticket
```markdown
# TKT-GLOBAL-XXX: Mejorar template de [tipo]

Propuesta de mejora: [descripción]
Razón: [por qué es mejor]
Impacto: [qué documentos se afectan]
```

### Paso 2: Implementar
- Actualiza el template
- Documenta cambios en CHANGELOG
- Incrementa versión si es significativo

### Paso 3: Comunicar
- Notifica al equipo
- Actualiza READMEs relevantes
- Considera migrar docs existentes

---

## 📈 Métricas de Uso

| Template | Usos | Última Actualización |
|----------|------|---------------------|
| AGENT_TEMPLATE.md | 0 | 2026-02-10 |
| SKILL_TEMPLATE.md | 0 | 2026-02-10 |
| TICKET_TEMPLATE.md | 0 | 2026-02-10 |
| KNOWLEDGE_NOTE_TEMPLATE.md | 0 | 2026-02-10 |
| PROJECT_CONFIG_TEMPLATE.yaml | 0 | 2026-02-10 |

---

## 🔗 Ver También

- **Metodología**: [AI_SKILL_DEVELOPMENT_METHODOLOGY.md](../AI_SKILL_DEVELOPMENT_METHODOLOGY.md)
- **Agentes**: [../agents/README.md](../agents/README.md)
- **Skills**: [../skills/README.md](../skills/README.md)
- **Knowledge**: [../knowledge/README.md](../knowledge/README.md)
- **Tickets**: [../tickets/README.md](../tickets/README.md)

---

## 📝 CHANGELOG

### v1.0.0 (2026-02-10)
- ✨ Templates iniciales creados
  - AGENT_TEMPLATE.md
  - SKILL_TEMPLATE.md
  - TICKET_TEMPLATE.md
  - KNOWLEDGE_NOTE_TEMPLATE.md
  - PROJECT_CONFIG_TEMPLATE.yaml

---

**Versión actual**: 1.0.0  
**Última actualización**: 2026-02-10  
**Mantenedor**: Dr. Francisco Ibarra Carlos
