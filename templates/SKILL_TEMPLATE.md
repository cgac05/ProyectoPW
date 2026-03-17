# Skill Template

## Metadata
```yaml
skill:
  name: <skill_name>
  version: 1.0.0
  description: Breve descripción de lo que hace este skill
  category: data_processing | integration | validation | reporting | automation
  
author:
  name: <Tu nombre>
  created: YYYY-MM-DD
  last_updated: YYYY-MM-DD

dependencies:
  python_packages:
    - package1>=version
    - package2>=version
  system_requirements:
    - Sistema requerido (ej. SAP GUI, Office)
  other_skills:
    - skill_dependency_1

inputs:
  required:
    - input_param_1
    - input_param_2
  optional:
    - optional_param_1
    
outputs:
  - output_1
  - output_2

performance:
  avg_execution_time: <tiempo promedio>
  max_concurrent_instances: <número>
```

---

## 1. Descripción

### Propósito
[Describe en 2-3 oraciones qué hace este skill y por qué existe]

### Funcionalidad Principal
- Función 1
- Función 2
- Función 3

### Ventajas
- ✅ Ventaja 1
- ✅ Ventaja 2
- ✅ Reutilizable en múltiples contextos

---

## 2. Instalación

### Dependencias
```bash
# Instalar dependencias del skill
pip install package1>=version
pip install package2>=version
```

### Requisitos del Sistema
- Python >= 3.8
- Sistema operativo: Windows/Linux/Mac
- Otros requisitos específicos

---

## 3. Uso

### Ejemplo Básico
```python
from <path>.skill_core import <SkillName>

# Crear instancia del skill
skill = <SkillName>()

# Ejecutar skill con parámetros mínimos
result = skill.execute(
    input_param_1="value1",
    input_param_2="value2"
)

# Procesar resultado
print(result)
```

### Ejemplo Avanzado
```python
from <path>.skill_core import <SkillName>

# Configuración avanzada
config = {
    "timeout": 300,
    "retries": 3,
    "log_level": "DEBUG"
}

skill = <SkillName>(config=config)

# Ejecutar con parámetros opcionales
result = skill.execute(
    input_param_1="value1",
    input_param_2="value2",
    optional_param_1="optional_value",
    on_progress=lambda p: print(f"Progreso: {p}%")
)

# Validar resultado
if skill.validate_output(result):
    print("Resultado válido")
```

### Parámetros de Entrada

| Parámetro | Tipo | Requerido | Default | Descripción |
|-----------|------|-----------|---------|-------------|
| `input_param_1` | str | Sí | - | Descripción del parámetro 1 |
| `input_param_2` | int | Sí | - | Descripción del parámetro 2 |
| `optional_param_1` | bool | No | False | Descripción del parámetro opcional |

### Salida

```python
{
    "output_1": "Descripción del output 1",
    "output_2": 123,
    "status": "success",
    "metadata": {
        "execution_time": 1.23,
        "timestamp": "2026-02-10T10:30:00"
    }
}
```

---

## 4. Estructura de Archivos

```
<skill_name>/
├── README.md              # Este archivo
├── skill.yaml             # Metadata y configuración
├── __init__.py
├── skill_core.py          # Implementación principal
├── validators.py          # Validadores (opcional)
├── utils.py               # Utilidades (opcional)
├── config/
│   └── default.yaml       # Configuración por defecto
├── tests/
│   ├── test_skill_core.py
│   ├── test_validators.py
│   └── fixtures/          # Datos de prueba
└── requirements.txt       # Dependencias específicas
```

---

## 5. API del Skill

### Clase Principal: `<SkillName>`

#### Constructor
```python
def __init__(self, config: dict = None):
    """
    Inicializa el skill.
    
    Args:
        config (dict): Configuración opcional
    """
```

#### Método Principal: `execute()`
```python
def execute(self, 
            input_param_1: str,
            input_param_2: int,
            **kwargs) -> dict:
    """
    Ejecuta la funcionalidad principal del skill.
    
    Args:
        input_param_1: Descripción
        input_param_2: Descripción
        **kwargs: Parámetros opcionales
        
    Returns:
        dict: Resultado de la ejecución
        
    Raises:
        SkillException: Si ocurre un error
    """
```

#### Métodos Auxiliares
```python
def validate_input(self, **inputs) -> bool:
    """Valida los parámetros de entrada."""
    
def validate_output(self, output: dict) -> bool:
    """Valida el resultado de la ejecución."""
    
def cleanup(self):
    """Limpieza de recursos."""
```

---

## 6. Configuración

### Archivo skill.yaml
```yaml
skill:
  name: <skill_name>
  version: 1.0.0

settings:
  timeout: 300
  max_retries: 3
  log_level: INFO
  
validation:
  strict_mode: true
  validate_inputs: true
  validate_outputs: true
```

### Variables de Entorno
```bash
# .env
SKILL_<NAME>_PARAM1=value
SKILL_<NAME>_PARAM2=value
```

---

## 7. Testing

### Unit Tests
```bash
# Ejecutar todos los tests
pytest tests/ -v

# Test específico
pytest tests/test_skill_core.py::test_execute_basic -v
```

### Coverage
```bash
pytest --cov=<skill_name> --cov-report=html tests/
```

### Casos de Prueba

1. **Test Básico**: Ejecución exitosa con inputs mínimos
2. **Test Validación**: Inputs inválidos deben fallar
3. **Test Timeout**: Manejo de timeout
4. **Test Retry**: Reintentos ante falla temporal
5. **Test Edge Cases**: Casos límite

---

## 8. Performance

### Benchmarks
- **Promedio de ejecución**: X segundos
- **Casos simples**: Y segundos
- **Casos complejos**: Z segundos

### Optimizaciones
- Optimización 1 implementada
- Optimización 2 implementada

### Limitaciones
- Limitación 1: Descripción y workaround
- Limitación 2: Descripción y workaround

---

## 9. Troubleshooting

### Error: "Input validation failed"
**Causa**: Parámetros de entrada no cumplen validación  
**Solución**: Verificar que los inputs cumplan con el schema esperado

### Error: "Timeout exceeded"
**Causa**: La operación tomó más tiempo del configurado  
**Solución**: Incrementar el valor de `timeout` en la configuración

### Error: "Dependency not found"
**Causa**: Falta instalar una dependencia  
**Solución**: `pip install -r requirements.txt`

---

## 10. Integración

### Uso en Agentes
```python
# En un agente
from general.skills.<skill_name>.skill_core import <SkillName>

class MyAgent:
    def __init__(self):
        self.skill = <SkillName>()
        
    def run(self):
        result = self.skill.execute(param1="value")
        return result
```

### Uso en Otros Skills
```python
# Composición de skills
from general.skills.<skill_name>.skill_core import <SkillName>

class CompositeSkill:
    def __init__(self):
        self.dependency_skill = <SkillName>()
```

---

## 11. Versionado

### Changelog

#### v1.0.0 (YYYY-MM-DD)
- ✨ Funcionalidad inicial
- ✅ Tests básicos implementados

#### v1.1.0 (YYYY-MM-DD)
- ✨ Nueva característica X
- 🐛 Fix: Corrección de bug Y
- 📚 Documentación mejorada

### Breaking Changes
Ninguno en la versión actual.

---

## 12. Referencias

### Knowledge Relacionado
- [Documento técnico 1](../../knowledge/local/doc1.md)
- [API Reference externa](../../knowledge/remote/api_ref.md)

### Tickets
- [TKT-GLOBAL-001](../../tickets/TKT-GLOBAL-001.md): Creación del skill
- [TKT-GLOBAL-005](../../tickets/TKT-GLOBAL-005.md): Mejoras de performance

### Proyectos que Usan este Skill
- `zpp007f_line_dt_rep`
- `otro_proyecto`

---

**Mantenedor**: <Nombre>  
**Última actualización**: YYYY-MM-DD  
**Próxima revisión**: YYYY-MM-DD
