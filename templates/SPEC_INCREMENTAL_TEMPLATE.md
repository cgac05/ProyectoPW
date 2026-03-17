# SPEC_###: [Título de la Funcionalidad Nueva]

## 📋 Metadata

| Campo | Valor |
|-------|-------|
| **ID Spec** | SPEC_### |
| **Ticket Externo** | REQ-XXXX-YYYY |
| **Solicitante** | Nombre (Área) |
| **Fecha Solicitud** | YYYY-MM-DD |
| **Prioridad** | 🔴 Alta / 🟡 Media / 🟢 Baja |
| **Estado** | 🟡 En Especificación / ✅ Aprobado / 🚧 En Desarrollo / ✅ Completado |
| **Relacionado con** | SPECIFICATION.md (original) |
| **Proyecto** | <nombre_proyecto> |
| **Categoría** | Feature Nueva / Integración / Ampliación |

---

## 1. Visión General

### Contexto
[Describe el estado actual del proyecto y por qué se necesita esta funcionalidad]

### Objetivo
[Qué se quiere lograr con esta nueva funcionalidad]

### Justificación de Negocio
[Por qué es necesario este cambio]

**Beneficios Esperados**:
- Beneficio 1
- Beneficio 2
- Beneficio 3

---

## 2. Alcance del Cambio

### En Alcance ✅
- Funcionalidad 1
- Funcionalidad 2
- Funcionalidad 3

### Fuera de Alcance ❌
- Lo que NO se va a hacer
- Razón por la cual no está incluido

### Impacto en Sistema Existente
[Qué componentes actuales se ven afectados]

**Componentes Afectados**:
- ⚠️ Módulo X: [Descripción del cambio]
- ⚠️ Servicio Y: [Descripción del cambio]
- ✅ Módulo Z: Sin cambios

---

## 3. Arquitectura

### Diagrama de Arquitectura

```
[Diagrama ASCII o Mermaid mostrando cómo se integra la nueva funcionalidad]

Sistema Actual
    ↓
[Nuevo Componente A]
    ↓
[Nuevo Componente B]
    ↓
Sistema Actual
```

### Nuevos Componentes

#### Componente 1: [Nombre]
- **Propósito**: [Para qué sirve]
- **Tecnología**: [Qué se usará]
- **Ubicación**: `ruta/al/componente/`

#### Componente 2: [Nombre]
- **Propósito**: [Para qué sirve]
- **Tecnología**: [Qué se usará]
- **Ubicación**: `ruta/al/componente/`

### Componentes Modificados

#### Componente Existente: [Nombre]
- **Cambios Necesarios**: [Qué hay que modificar]
- **Razón**: [Por qué hay que modificarlo]

---

## 4. Entrada y Salida (Input/Output)

### 4.1 Nuevos Inputs

```yaml
nuevo_parametro_1:
  tipo: string
  descripción: "Descripción del parámetro"
  requerido: true
  valores_posibles: "A, B, C"

nuevo_parametro_2:
  tipo: integer
  descripción: "Descripción del parámetro"
  requerido: false
  default: 10
```

### 4.2 Nuevos Outputs

```yaml
nuevo_output_1:
  tipo: JSON
  descripción: "Datos estructurados de resultado"
  formato: |
    {
      "campo1": "valor",
      "campo2": 123
    }

nuevo_output_2:
  tipo: archivo
  descripción: "Reporte generado"
  formato: "Excel (.xlsx)"
```

---

## 5. Flujo de Proceso

### Flujo Actual (Antes)
```
Paso A → Paso B → Paso C
```

### Flujo Nuevo (Después)
```
Paso A → [NUEVO: Paso B1] → [NUEVO: Paso B2] → Paso C
```

### Descripción Detallada

**1. [NUEVO] Paso B1: [Nombre]**
- Input: [Qué recibe]
- Proceso: [Qué hace]
- Output: [Qué produce]
- Error handling: [Cómo maneja errores]

**2. [NUEVO] Paso B2: [Nombre]**
- Input: [Qué recibe]
- Proceso: [Qué hace]
- Output: [Qué produce]
- Error handling: [Cómo maneja errores]

---

## 6. Requisitos Técnicos

### 6.1 Nuevas Dependencias

```txt
# Python packages
nueva-libreria>=2.0.0
otra-dependencia>=1.5.0
```

### 6.2 Configuración Nueva

```yaml
# Nuevas variables de entorno
NUEVA_CONFIG_VAR: "valor"
API_KEY_SERVICIO: "secreto"
```

### 6.3 Infraestructura

**Requisitos Nuevos**:
- [ ] Base de datos adicional
- [ ] Servicio externo (API)
- [ ] Almacenamiento adicional
- [ ] Recursos de CPU/RAM

---

## 7. Investigación Requerida

### Decisiones Técnicas Pendientes

1. **Decisión 1**: [Qué hay que investigar]
   - Opción A: [Descripción]
   - Opción B: [Descripción]
   - **Recomendación**: [Cuál parece mejor y por qué]

2. **Decisión 2**: [Qué hay que investigar]
   - Opción A: [Descripción]
   - Opción B: [Descripción]
   - **Recomendación**: [Cuál parece mejor y por qué]

### Knowledge Base a Generar

- `knowledge/local/XX_[tema]_research.md`
- `knowledge/local/YY_[tema]_decisions.md`
- `knowledge/remote/[referencia]_docs.md`

---

## 8. Casos de Uso

### Caso de Uso 1: [Nombre]

**Actor**: Usuario tipo X

**Precondiciones**:
- Condición 1
- Condición 2

**Flujo Principal**:
1. Usuario hace X
2. Sistema hace Y
3. Sistema muestra Z

**Postcondiciones**:
- Resultado 1
- Resultado 2

**Casos Alternativos**:
- Si ocurre A, entonces B

---

### Caso de Uso 2: [Nombre]

[Repetir estructura]

---

## 9. Casos de Prueba

### Test Case 1: [Nombre]

**Objetivo**: Validar [qué]

**Pasos**:
1. Paso 1
2. Paso 2
3. Paso 3

**Resultado Esperado**: [Qué debería pasar]

**Criterio de Aceptación**: [Cómo saber que funciona]

---

### Test Case 2: [Nombre]

[Repetir estructura]

---

## 10. Estimación y Plan

### Estimación de Esfuerzo

| Tarea | Estimación | Asignado |
|-------|-----------|----------|
| Investigación (Picoro) | X horas | Picoro |
| Diseño (Picoro) | Y horas | Picoro |
| Implementación Componente A (Goku) | Z horas | Goku |
| Implementación Componente B (Goku) | W horas | Goku |
| Optimización (Vegeta) | V horas | Vegeta |
| Testing (Bulma) | U horas | Bulma |
| **TOTAL** | **## horas** | |

### Tickets a Generar

Una vez aprobada esta spec, se generarán los siguientes tickets:

- TKT-<CODE>-###: [Descripción del ticket 1]
- TKT-<CODE>-###: [Descripción del ticket 2]
- TKT-<CODE>-###: [Descripción del ticket 3]

---

## 11. Riesgos y Mitigación

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|---------|------------|
| Riesgo 1 | Alta/Media/Baja | Alto/Medio/Bajo | Estrategia de mitigación |
| Riesgo 2 | Alta/Media/Baja | Alto/Medio/Bajo | Estrategia de mitigación |

---

## 12. Criterios de Aceptación (Global)

- [ ] Todos los casos de uso funcionan correctamente
- [ ] Todos los casos de prueba pasan
- [ ] Performance no se degrada
- [ ] Sistema existente sigue funcionando sin problemas
- [ ] Documentación actualizada
- [ ] Usuario/Cliente aprueba en UAT

---

## 13. Aprobaciones

### Desarrollador
- **Nombre**: [Nombre]
- **Fecha**: YYYY-MM-DD
- **Firma/Aprobación**: ✅

### Tech Lead / Arquitecto
- **Nombre**: [Nombre]
- **Fecha**: YYYY-MM-DD
- **Firma/Aprobación**: ✅

### Usuario / Cliente
- **Nombre**: [Nombre]
- **Fecha**: YYYY-MM-DD
- **Firma/Aprobación**: ✅

---

## 14. Referencias

### Documentación Relacionada
- [SPECIFICATION.md original](../SPECIFICATION.md)
- [Ticket Externo REQ-XXXX-YYYY](link)

### Knowledge Base
- [Knowledge note relevante](../../knowledge/local/nota.md)

### Enlaces Externos
- [Documentación de API](url)
- [Referencia técnica](url)

---

## 15. Notas y Comentarios

### Comentarios del Desarrollador
[Observaciones técnicas importantes]

### Comentarios del Usuario
[Feedback o aclaraciones del usuario]

### Decisiones Tomadas
[Registro de decisiones importantes durante la especificación]

---

**Creado por**: [Nombre]  
**Fecha**: YYYY-MM-DD  
**Versión**: 1.0  
**Última actualización**: YYYY-MM-DD
