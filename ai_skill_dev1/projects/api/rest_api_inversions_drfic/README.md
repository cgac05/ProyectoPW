# Backend REST API — Proyecto pwa_inversions_drfic

## Metadata

```yaml
project:
  code: rest_api_inversions_drfic
  name: REST API de Inversiones
  category: api
  version: 0.1.0
  status: initial_setup
  
description: Backend REST API que expone persistencia real. Conecta con base de datos, valida reglas de negocio, y expone endpoints para la PWA.

date_initialized: 2026-03-17
```

---

## 📁 Estructura

```
rest_api_inversions_drfic/
├── src/
│   ├── models/              # Modelos de datos (Prisma, Mongoose, etc.)
│   ├── migrations/          # Migraciones versionadas
│   ├── services/            # Lógica de negocio
│   ├── routes/              # Definición de rutas
│   ├── controllers/         # Handlers de endpoints
│   ├── middleware/          # Autenticación, validación
│   ├── types/               # Tipos TypeScript
│   └── config/              # Configuración
│
├── DATABASE_CONFIG.yaml     # Configuración multi-BD
├── .env.example             # Variables sin secretos
├── package.json
├── tsconfig.json
└── README.md
```

---

## 🚀 Responsabilidades

- ✅ Conectar a base de datos (Supabase, MongoDB, etc.)
- ✅ Ejecutar migraciones de forma segura
- ✅ Exponer endpoints REST
- ✅ Validar reglas de negocio
- ✅ Manejo de errores
- ✅ Seguridad (autenticación, autorización)

---

## 🎯 Próximos Pasos

1. @krillin: Diseña schema en DATABASE_CONFIG.yaml
2. @krillin: Implementa migraciones
3. @krillin: Expone servicios REST
4. @goku: Integra con PWA

---

**Proyecto**: rest_api_inversions_drfic  
**Fecha Inicialización**: March 17, 2026
