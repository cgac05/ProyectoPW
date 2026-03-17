# TKT-<CODIGO>-###: [Título Corto y Descriptivo]

## 📋 Metadata

| Campo | Valor |
|-------|-------|
| **ID** | TKT-<CODIGO>-### |
| **Ticket Externo** | REQ-XXXX-YYYY (si aplica) |
| **Solicitante** | Nombre (Área) |
| **Tipo** | Nuevo Proyecto / Feature / Bug / Refactor / Mejora / Corrección |
| **Prioridad** | 🔴 Alta / 🟡 Media / 🟢 Baja |
| **Estado** | 🆕 Abierto / 🟡 En Desarrollo / 🔵 En Revisión / 🚫 Bloqueado / ✅ Completado |
| **Proyecto** | <nombre_proyecto> o GLOBAL |
| **Categoría** | <categoria_del_proyecto> |
| **Creado** | YYYY-MM-DD |
| **Actualizado** | YYYY-MM-DD |
| **Asignado a** | <nombre> |
| **Estimación** | X horas/días |
| **Sprint/Milestone** | Sprint ## / Milestone X |
| **Relacionado con** | SPECIFICATION.md o incremental/SPEC_00X.md |

---

## 🚦 Gate de Estado (Obligatorio)

- ✅ **Completado**: solo si hay evidencia de ejecución y validación.
- 🔵 **En Revisión**: código listo, pero pendiente de ejecutar/validar.
- 🚫 **Bloqueado**: dependencia externa impide validar.

**No permitido**: mover a ✅ Completado sin pruebas ejecutadas y registradas.

## 🗄️ Gate de Modelo de Datos (Solo si aplica)

Completar esta seccion cuando el ticket modifica esquema/modelo/persistencia:

| Campo | Valor |
|-------|-------|
| **Motor(es)** | Supabase / MongoDB / Otro |
| **Estado del modelo** | draft / candidate / approved |
| **Trazabilidad SPEC -> datos** | Ruta del archivo de trazabilidad |
| **Knowledge relacionado** | Rutas knowledge/local y knowledge/remote |
| **Aprobacion humana** | Nombre + fecha |

Regla:
- Un ticket de datos no puede cerrarse en ✅ Completado si el modelo queda en `draft`.

---

## 📝 Descripción

### Contexto
[Describe el contexto o situación que motiva este ticket]

**Justificación**: [Por qué es necesario este cambio]

### Problema/Necesidad
[Si es un bug, describe el problema. Si es feature, describe la necesidad]

### Solución Propuesta
[Describe cómo se resolverá o implementará]

---

## 🔍 Análisis de Impacto

**Archivos a Modificar/Crear**:
- `ruta/archivo1.py`
- `ruta/archivo2.py`

**Componentes Afectados**:
- Módulo X
- Servicio Y

**Riesgos**:
- [Identificar riesgos potenciales]

---

## 🤖 Workflow de Agentes

### Picoro analiza:
- [ ] Ticket revisado y entendido
- [ ] Impacto identificado
- [ ] Plan de implementación definido
- [ ] Skills necesarios confirmados
- [ ] Aprobación para continuar

### Goku implementa:
- [ ] Código implementado
- [ ] Documentación inline agregada con estándar `FIC` bilingüe (EN/ES)
- [ ] README actualizado (si aplica)
- [ ] Listo para optimización

### Vegeta optimiza (si aplica):
- [ ] Performance revisado
- [ ] Seguridad auditada
- [ ] Patrones refactorizados
- [ ] Code review completado

### Bulma valida:
- [ ] Tests unitarios creados
- [ ] Tests de integración creados (si aplica)
- [ ] Tests ejecutados exitosamente
- [ ] Bugs detectados y reportados
- [ ] Validación funcional completada

---

## ✅ Criterios de Aceptación

1. **Criterio 1**
   - Dada [condición inicial]
   - Cuando [acción]
   - Entonces [resultado esperado]

2. **Criterio 2**
   - Dada [condición inicial]
   - Cuando [acción]
   - Entonces [resultado esperado]

3. **Criterio 3**
   - Dada [condición inicial]
   - Cuando [acción]
   - Entonces [resultado esperado]

---

## 🧪 Plan de Testing

### Unit Tests
- [ ] Test para funcionalidad A
- [ ] Test para funcionalidad B
- [ ] Test para casos edge

### Integration Tests
- [ ] Test de integración X
- [ ] Test de integración Y

### Manual Testing / UAT
- [ ] Escenario 1: [Descripción]
- [ ] Escenario 2: [Descripción]
- [ ] Usuario valida (si aplica)

---

## 🧾 Evidencia de Validación (Requerida para ✅ Completado)

- [ ] Fecha de ejecución registrada
- [ ] Entorno registrado (DEV/QAS/PROD)
- [ ] Responsable de validación registrado
- [ ] Resultado registrado (PASS/FAIL)
- [ ] Comando(s) o pasos ejecutados documentados
- [ ] Evidencia adjunta (output/log/captura o resumen verificable)

---

## 📚 Documentación Requerida

- [ ] Actualizar README del proyecto
- [ ] Actualizar documentación técnica
- [ ] Crear/actualizar knowledge note
  - Ubicación: `knowledge/local/<nota>.md`
- [ ] Actualizar comentarios en código
- [ ] Actualizar CHANGELOG

### Consideraciones Técnicas
- Consideración 1
- Consideración 2
- Consideración 3

### Skills/Agentes Involucrados
- **Skill**: `<skill_name>` (global/local)
  - Cambios requeridos: [descripción]
- **Agente**: `<agent_name>` (global/local)
  - Cambios requeridos: [descripción]

---

## ⚠️ Riesgos y Mitigación

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|---------|------------|
| Riesgo 1 | Alta/Media/Baja | Alto/Medio/Bajo | Estrategia de mitigación |
| Riesgo 2 | Alta/Media/Baja | Alto/Medio/Bajo | Estrategia de mitigación |

---

## 🔍 Notas de Desarrollo

### YYYY-MM-DD
**Autor**: <nombre>  
[Notas sobre decisiones tomadas, problemas encontrados, soluciones implementadas]

### YYYY-MM-DD
**Autor**: <nombre>  
[Más notas...]

---

## 📊 Review Checklist

### Código
- [ ] Código sigue estándares del proyecto
- [ ] No hay código duplicado
- [ ] Manejo apropiado de errores
- [ ] Logging implementado correctamente
- [ ] Sin warnings del linter

### Testing
- [ ] Todos los tests pasan
- [ ] Coverage mínimo alcanzado (X%)
- [ ] Tests de casos edge incluidos
- [ ] Tests de regresión considerados

### Documentación
- [ ] Código documentado (docstrings)
- [ ] Comentarios `FIC` en inglés/español en bloques clave
- [ ] README actualizado
- [ ] Knowledge notes creadas/actualizadas
- [ ] Ejemplos de uso incluidos

### Performance
- [ ] Sin degradación de performance
- [ ] Optimizaciones implementadas (si aplica)
- [ ] Recursos liberados apropiadamente

---

## 🎉 Cierre

**Fecha Cierre**: YYYY-MM-DD  
**Cerrado por**: [Nombre]

### Resumen del Trabajo Realizado
[Completar al cerrar el ticket]

### Archivos Modificados (Final)
- `archivo1.py`: [Cambios realizados]
- `archivo2.py`: [Cambios realizados]

### Knowledge Generado
- [Link a knowledge note](path/to/note.md)

### Commit
```bash
git commit -m "tipo(scope): descripción (#TKT-XXX-###)"
```

**Ejemplo**: `feat(zpp007f): add shift filter to report (#TKT-ZPP007F-008)`

### Ticket Externo Cerrado
**REQ-XXXX-YYYY**: ✅ RESUELTO (si aplica)

### Métricas
- Tiempo estimado: X horas
- Tiempo real invertido: Y horas
- Tests creados: # tests
- Coverage: X%
- Líneas de código: +X -Y

---

## 📌 Referencias

### Documentación
- [Documento 1](link)
- [Documento 2](link)

### Knowledge
- [Knowledge Note 1](../../knowledge/local/note1.md)
- [Knowledge Note 2](../../knowledge/remote/note2.md)

### Enlaces Externos
- [API Docs](url)
- [Stack Overflow](url)

---

## 🏷️ Tags

`<tag1>` `<tag2>` `<tag3>` `python` `rpa` `automation`

---

**Estado Final**: [Closed/Rejected/Deferred]  
**Razón de Cierre**: [Completado exitosamente / Duplicado / No relevante / Pospuesto]  
**Fecha de Cierre**: YYYY-MM-DD
