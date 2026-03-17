# ESPECIFICACIÓN TÉCNICA

## Proyecto: [Nombre Descriptivo del Proyecto]

**Código del Proyecto**: `<project_code>`  
**Ticket Externo**: REQ-XXXX-YYYY (si aplica)  
**Solicitante**: [Nombre (Área)]  
**Transacción SAP**: [Transacción] (si aplica)  
**Programa SAP**: [Programa] (si aplica)  
**Fecha**: YYYY-MM-DD  
**Estado**: 🟡 En Especificación / ✅ Aprobado / 🚧 En Desarrollo / ✅ Completado  

---

## 1. Visión General

### Contexto
[Describe la situación actual y el problema que se quiere resolver]

### Objetivo
[Qué se quiere lograr con este proyecto - debe ser claro y medible]

**Ejemplo**: Automatizar la extracción de datos del Reporte de Paros desde SAP ECC, exportarlos a Excel y transformarlos según plantilla estándar para su distribución.

### Flujo Principal

```
[Describe el flujo end-to-end con diagrama ASCII]

Sistema Origen
    ↓ [Acción 1]
Sistema/Archivo Intermedio
    ↓ [Acción 2]
Sistema/Archivo Final
    ↓ [Acción 3]
Resultado Final
```

### Resultado Esperado

- **Input**: [Qué entra al sistema]
- **Output**: [Qué sale del sistema]
- **Frecuencia**: Manual (on-demand) / Programada (diaria, semanal, etc.)
- **Usuarios**: [Quiénes usarán este sistema]

### Beneficios
- Beneficio 1
- Beneficio 2
- Beneficio 3

---

## 2. Entrada (Input)

### 2.1 Parámetros de Ejecución

**Parámetros Requeridos**:
```yaml
parametro_1:
  tipo: string
  descripción: "Descripción del parámetro"
  valores_posibles: "A, B, C"
  requerido: true
  ejemplo: "valor_ejemplo"

parametro_2:
  tipo: integer
  descripción: "Descripción del parámetro"
  requerido: false
  default: 10
  ejemplo: 15
```

**Preguntas para el Usuario**:
- [ ] ¿Qué filtros/parámetros se deben usar?
- [ ] ¿Estos parámetros son fijos o variables por ejecución?
- [ ] ¿Quién proporciona estos valores?
- [ ] ¿Hay validaciones específicas?

### 2.2 Archivos de Entrada (si aplica)

**Archivo 1**: [Nombre del archivo]
- **Ubicación**: [Ruta]
- **Formato**: Excel / CSV / JSON / XML
- **Estructura**:
  - Columna 1: [Nombre y tipo]
  - Columna 2: [Nombre y tipo]
  - ...
- **Validaciones**:
  - [ ] Archivo debe existir
  - [ ] Formato debe ser válido
  - [ ] Columnas requeridas presentes
  - [ ] Datos no vacíos

**Plantillas** (si aplica):
- [ ] ¿Dónde está la plantilla? (ruta)
- [ ] ¿Tiene fórmulas o solo estructura?
- [ ] ¿Qué campos mapean a qué columnas?

### 2.3 Credenciales y Conexiones

**Sistema 1**: [Nombre del sistema]
- **Tipo**: SAP / API / Base de datos / Servicio web
- **Credenciales**: [Dónde se obtienen]
- **Ambiente**: PRD / TEST / DEV
- **Timeout**: [segundos]
- **Permisos Requeridos**: [Listar permisos]

---

## 3. Proceso (Lógica de Negocio)

### 3.1 Paso 1: [Nombre del Paso]

```python
PASO 1: [Descripción breve]
├─ Acción 1: [Detalle]
├─ Acción 2: [Detalle]
│  ├─ Sub-acción 2.1
│  └─ Sub-acción 2.2
├─ Validación: [Qué validar]
└─ Error handling: [Qué hacer si falla]
```

**Detalles Técnicos**:
- Input: [Qué recibe]
- Proceso: [Qué hace]
- Output: [Qué produce]
- Tiempo estimado: [segundos/minutos]

**Casos Especiales**:
- Si [condición], entonces [acción alternativa]
- Si [error], entonces [manejo]

---

### 3.2 Paso 2: [Nombre del Paso]

[Repetir estructura del Paso 1]

---

### 3.3 Paso 3: [Nombre del Paso]

[Repetir estructura]

---

### 3.X Validaciones de Negocio

**Validación 1**: [Nombre]
- **Descripción**: [Qué se valida]
- **Regla**: [Condición que debe cumplirse]
- **Acción si falla**: [Qué hacer]

**Validación 2**: [Nombre]
- [Repetir estructura]

---

### 3.Y Manejo de Errores

| Tipo de Error | Causa | Acción |
|---------------|-------|--------|
| Error Tipo 1 | [Causa] | [Acción correctiva] |
| Error Tipo 2 | [Causa] | [Acción correctiva] |
| Error Tipo 3 | [Causa] | [Acción correctiva] |

---

## 4. Salida (Output)

### 4.1 Archivos Generados

```
📁 data/raw/                    # Datos brutos sin procesar
└─ archivo_bruto_YYYYMMDD_HHMMSS.xlsx

📁 data/output/                 # Archivos finales procesados
└─ archivo_final_YYYYMMDD_HHMMSS.xlsx

📁 logs/                        # Logs de ejecución
├─ proceso_YYYYMMDD.log         # Log general
└─ proceso_YYYYMMDD_errors.log  # Solo errores
```

### 4.2 Formato de Salida

**Archivo Principal**: [Nombre]
- **Formato**: Excel / CSV / PDF / JSON
- **Ubicación**: [Ruta]
- **Nombre**: [Patrón de nombre]
- **Estructura**:
  - Sheet/Sección 1: [Contenido]
  - Sheet/Sección 2: [Contenido]

**Columnas/Campos**:
| Campo | Tipo | Descripción | Obligatorio |
|-------|------|-------------|-------------|
| Campo1 | string | Descripción | Sí |
| Campo2 | integer | Descripción | No |
| Campo3 | date | Descripción | Sí |

### 4.3 Notificaciones (si aplica)

- [ ] ¿Se envía email al completar?
  - Destinatarios: [emails]
  - Asunto: [template]
  - Contenido: [qué incluir]
- [ ] ¿Se actualiza algún sistema?
- [ ] ¿Se genera algún reporte adicional?

---

## 5. Requisitos Técnicos

### 5.1 Tecnologías Requeridas

**Lenguaje**: Python 3.x (especificar versión)

**Librerías Python**:
```txt
# Requirements principales
libreria1>=2.0.0
libreria2>=1.5.0
pandas>=1.3.0
openpyxl>=3.0.0  # Si maneja Excel
pywin32>=301     # Si conecta a SAP GUI
requests>=2.25.0 # Si consume APIs
```

**Sistemas Externos**:
- Sistema 1: [Nombre y versión]
- Sistema 2: [Nombre y versión]

### 5.2 Configuración

**Variables de Entorno** (`.env`):
```bash
# Conexiones
SISTEMA_URL=https://...
SISTEMA_USER=usuario
SISTEMA_PASSWORD=password

# Rutas
DIR_INPUT=C:\path\to\input
DIR_OUTPUT=C:\path\to\output
DIR_TEMPLATES=C:\path\to\templates

# Configuración
MAX_RETRIES=3
TIMEOUT_SECONDS=300
DEBUG_MODE=false
```

### 5.3 Infraestructura

**Servidor/Máquina**:
- SO: Windows / Linux
- RAM mínima: X GB
- Disco: Y GB disponible
- Acceso a red: [requisitos]

**Permisos Necesarios**:
- [ ] Lectura en [ruta/sistema]
- [ ] Escritura en [ruta/sistema]
- [ ] Ejecución de [proceso]
- [ ] Acceso a [API/servicio]

---

## 6. Casos de Uso

### Caso de Uso 1: [Nombre]

**Actor**: [Usuario tipo X]

**Precondiciones**:
- Condición 1 debe estar cumplida
- Condición 2 debe estar cumplida

**Flujo Principal**:
1. Usuario hace X
2. Sistema valida Y
3. Sistema procesa Z
4. Sistema genera W
5. Sistema notifica resultado

**Postcondiciones**:
- Estado del sistema después
- Archivos generados
- Notificaciones enviadas

**Flujos Alternativos**:
- **3a. Si validación falla**:
  - Sistema muestra error
  - Sistema registra en log
  - Sistema termina ejecución

**Flujos de Excepción**:
- **2e. Si conexión falla**:
  - Sistema reintenta 3 veces
  - Si falla después de 3 intentos, notifica error
  - Se registra en log de errores

---

### Caso de Uso 2: [Nombre]

[Repetir estructura]

---

## 7. Casos de Prueba (Criterios de Aceptación)

### Test Case 1: [Escenario Normal - Happy Path]

**Objetivo**: Validar que el proceso completo funciona correctamente

**Precondiciones**:
- Sistema disponible
- Datos de prueba preparados
- Credenciales válidas

**Pasos**:
1. Ejecutar script con parámetros válidos
2. Verificar conexión exitosa
3. Validar procesamiento de datos
4. Verificar archivo de salida generado
5. Validar contenido del archivo

**Resultado Esperado**:
- Proceso completa sin errores
- Archivo generado en ubicación correcta
- Datos en archivo son correctos
- Log no contiene errores

**Criterio de Aceptación**: ✅ Todos los pasos se completan exitosamente

---

### Test Case 2: [Escenario con Error]

**Objetivo**: Validar manejo de error cuando [condición]

**Pasos**:
1. [Simular condición de error]
2. Ejecutar proceso
3. Verificar que error se maneja correctamente

**Resultado Esperado**:
- Sistema detecta el error
- Sistema registra error en log
- Sistema no genera archivo corrupto
- Sistema notifica error (si aplica)

**Criterio de Aceptación**: ✅ Error se maneja sin crashes

---

### Test Case 3: [Escenario Edge Case]

[Repetir estructura]

---

## 8. Arquitectura de Componentes

### 8.1 Vista General

```
┌─────────────────────────────────────────┐
│         MÓDULO ORQUESTADOR              │
│    (modules/main_processor/)            │
└────────┬────────────────────┬───────────┘
         │                    │
    ┌────▼─────┐         ┌────▼─────┐
    │ Servicio1│         │ Servicio2│
    │ (connect)│         │ (extract)│
    └────┬─────┘         └────┬─────┘
         │                    │
    ┌────▼────────────────────▼─────┐
    │   Servicio3 (transform)       │
    └────┬──────────────────────────┘
         │
    ┌────▼─────┐
    │ Servicio4│
    │ (output) │
    └──────────┘
```

### 8.2 Componentes Principales

#### Módulo: [Nombre del Módulo]
- **Ubicación**: `modules/<nombre>/`
- **Responsabilidad**: [Qué hace]
- **Servicios que usa**:
  - Servicio 1
  - Servicio 2
  - ...

#### Servicio: [Nombre del Servicio]
- **Ubicación**: `services/<nombre>/`
- **Responsabilidad**: [Qué hace]
- **Inputs**: [Qué recibe]
- **Outputs**: [Qué produce]
- **Dependencias**: [Librerías externas]

---

### 8.3 Skills de IA Necesarios

**Skills del Proyecto** (a documentar en `ai_global/skills/` si no existen):
- `<dominio>_<accion>_skill`: [Descripción]
- `<dominio>_<accion>_skill`: [Descripción]
- ...

**Agentes que los usarán**:
- Picoro: [skills para análisis/diseño]
- Goku: [skills para implementación]
- Vegeta: [skills para optimización]
- Bulma: [skills para testing]

---

## 9. Decisiones Técnicas Pendientes

### Decisión 1: [Tema a decidir]

**Opciones**:

**Opción A**: [Nombre]
- **Pros**:
  - ✅ Ventaja 1
  - ✅ Ventaja 2
- **Contras**:
  - ❌ Desventaja 1
  - ❌ Desventaja 2
- **Esfuerzo**: Alto / Medio / Bajo

**Opción B**: [Nombre]
- **Pros**:
  - ✅ Ventaja 1
  - ✅ Ventaja 2
- **Contras**:
  - ❌ Desventaja 1
  - ❌ Desventaja 2
- **Esfuerzo**: Alto / Medio / Bajo

**Recomendación Inicial**: [Opción X] por [razones]

**Investigación Requerida**: [Qué necesita Picoro investigar]

---

### Decisión 2: [Tema a decidir]

[Repetir estructura]

---

## 10. Dependencias y Restricciones

### Dependencias
- **Sistemas Externos**:
  - Sistema X debe estar disponible
  - API Y debe responder en < 5 segundos
  
- **Archivos/Datos**:
  - Archivo plantilla debe existir en [ubicación]
  - Datos de entrada deben estar actualizados
  
- **Otros Proyectos**:
  - [Si depende de otro proyecto, especificar]

### Restricciones
- **Técnicas**:
  - Debe ejecutarse en Windows (por SAP GUI)
  - No puede usar más de X GB de RAM
  
- **Negocio**:
  - Debe completarse en menos de Y minutos
  - No puede ejecutarse entre [horario]
  
- **Seguridad**:
  - Credenciales deben estar en .env (no hardcoded)
  - Logs no deben contener información sensible

---

## 11. Plan de Implementación

### Estimación de Esfuerzo

| Fase | Tareas | Estimación | Asignado |
|------|--------|-----------|----------|
| FASE 2.3 (Investigación) | Picoro investiga tecnologías | X horas | Picoro |
| FASE 2.4 (Diseño) | Picoro diseña arquitectura | Y horas | Picoro |
| FASE 3 (Implementación) | Goku implementa servicios | Z horas | Goku |
| FASE 3 (Optimización) | Vegeta optimiza código | W horas | Vegeta |
| FASE 3 (Testing) | Bulma crea tests | V horas | Bulma |
| **TOTAL** | | **## horas** | |

### Tickets a Generar (Provisionales)

Una vez que Picoro complete el diseño, se generarán aproximadamente:

- TKT-<CODE>-001: Setup inicial del proyecto
- TKT-<CODE>-002: Implementar servicio [nombre]
- TKT-<CODE>-003: Implementar servicio [nombre]
- TKT-<CODE>-004: Implementar servicio [nombre]
- TKT-<CODE>-005: Implementar módulo orquestador
- TKT-<CODE>-006: Crear tests de integración
- TKT-<CODE>-007: Documentación y deployment

---

## 12. Riesgos

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|---------|------------|
| Sistema externo no disponible | Media | Alto | Implementar retry logic + notificaciones |
| Cambio en estructura de datos | Baja | Alto | Validación robusta de estructura + tests |
| Performance insuficiente | Media | Medio | Optimización + procesamiento por lotes |
| Credenciales inválidas | Baja | Alto | Validación previa + manejo de errores |

---

## 13. Documentación de Referencia

### Scripts Existentes (si aplica)
- `docs/scripts/script_referencia.vbs`: [Descripción de qué hace]
- [Otro script]: [Descripción]

### Documentación Externa
- [Sistema X API Docs](url): Documentación del API
- [Librería Y Docs](url): Manual de uso
- [SAP Transacción Z](url): Documentación SAP

### Knowledge Base (a generar por Picoro)
Picoro deberá crear durante FASE 2.3:
- `knowledge/local/01_<tema>_research.md`
- `knowledge/local/02_<tema>_patterns.md`
- `knowledge/local/03_<tema>_decisions.md`
- `knowledge/remote/<sistema>_api_reference.md`

---

## 14. Aprobaciones

### Desarrollador
- **Nombre**: [Nombre]
- **Fecha**: YYYY-MM-DD
- **Comentarios**: [Comentarios técnicos]
- **Aprobación**: ✅ / ⏳ Pendiente / ❌ Rechazado

### Tech Lead / Arquitecto
- **Nombre**: [Nombre]
- **Fecha**: YYYY-MM-DD
- **Comentarios**: [Comentarios de arquitectura]
- **Aprobación**: ✅ / ⏳ Pendiente / ❌ Rechazado

### Usuario / Cliente / Product Owner
- **Nombre**: [Nombre]
- **Fecha**: YYYY-MM-DD
- **Comentarios**: [Feedback de negocio]
- **Aprobación**: ✅ / ⏳ Pendiente / ❌ Rechazado

---

## 15. Control de Cambios

| Versión | Fecha | Autor | Cambios |
|---------|-------|-------|---------|
| 1.0 | YYYY-MM-DD | [Nombre] | Creación inicial |
| 1.1 | YYYY-MM-DD | [Nombre] | [Descripción de cambios] |

---

## 16. Notas Adicionales

### Observaciones del Desarrollador
[Cualquier observación técnica importante que no encaje en otras secciones]

### Preguntas Pendientes al Usuario
- [ ] Pregunta 1
- [ ] Pregunta 2
- [ ] Pregunta 3

### Decisiones Tomadas Durante Especificación
- **[Fecha]**: Decidió usar X en lugar de Y porque [razón]
- **[Fecha]**: Acordó con usuario que [decisión]

---

**Próximos Pasos**:
1. ✅ Aprobar esta especificación
2. ⏳ Picoro investiga y crea knowledge base (FASE 2.3)
3. ⏳ Picoro diseña arquitectura y crea workflow_agents.yaml (FASE 2.4)
4. ⏳ Crear tickets internos de desarrollo
5. ⏳ Iniciar FASE 3 (Implementación)

---

**Creado por**: [Nombre]  
**Fecha**: YYYY-MM-DD  
**Última actualización**: YYYY-MM-DD  
**Versión**: 1.0
