---
type: concept
title: "Arandil — Arquitectura del Sistema"
description: "Stack tecnológico, módulos core, flujo principal y diagrama de componentes"
tags: [arquitectura, stack, componentes, flujo]
timestamp: 2026-07-04
resource: ../../arandil/
refs:
  - DECISIONS.md
  - VISION.md
---

# Arandil — Arquitectura del Sistema

## Stack Tecnológico

| Capa | Tecnología | Versión | Justificación |
|------|------------|---------|---------------|
| **Monorepo** | pnpm workspaces + Turborepo | pnpm 9.x, turbo 2.x | Builds paralelos, caché incremental ([DEC-008](DECISIONS.md#dec-008)) |
| **App mobile** | React Native + Expo | RN 0.76, Expo 52 | Cross-platform, iteración rápida |
| **Navegación mobile** | Expo Router | 4.x | File-based routing, type-safe |
| **Backend** | Node.js + Express | Node 20 LTS, Express 4.x | Simple, probado en Arandur |
| **Base de datos** | PostgreSQL | 15+ | Relacional, transacciones ACID, JSON support |
| **Driver DB** | pg | 8.x | SQL puro, sin ORM ([DEC-002](DECISIONS.md#dec-002)) |
| **Auth** | Supabase Auth | SDK 2.x | JWT, OAuth providers, manejo de sesiones |
| **Storage** | Cloudflare R2 | S3-compatible | Multimedia (imágenes, audio TTS) |
| **IA principal** | DeepSeek V3 | API | Generación MCQ, modo socrático ([DEC-006](DECISIONS.md#dec-006)) |
| **IA premium** | Anthropic Claude | Sonnet 4.5 | Explicaciones complejas (v1.1+) |
| **SRS** | FSRS-7 (ts-fsrs) | 4.x | Repetición espaciada científica ([DEC-007](DECISIONS.md#dec-007)) |
| **Jobs asíncronos** | BullMQ + Redis | BullMQ 5.x, Redis 7.x | Generación IA, TTS en background |
| **WhatsApp** | Twilio API | SDK 5.x | Reportes a apoderado |
| **Suscripciones** | RevenueCat | SDK | In-app purchases, webhooks |
| **Testing** | Vitest + Jest | Vitest 2.x (API), Jest 29.x (mobile) | Rápido, type-safe |
| **Linting** | TypeScript strict | TS 5.7 | Sin `any`, null safety |
| **Validación** | Zod | 3.x | Schemas type-safe |

---

## Estructura del Monorepo

```
arandil-workspace/
├── CLAUDE.md                    # Protocolo Claude Code (raíz workspace)
│
├── arandil-brain/               # Documentación (brain)
│   ├── CURRENT/
│   │   ├── STATUS.md            # Estado volátil (actualizado cada sesión)
│   │   ├── VISION.md            # Visión de producto
│   │   ├── DECISIONS.md         # Decisiones técnicas (DEC-001, DEC-002...)
│   │   ├── ARCHITECTURE.md      # ← Este archivo
│   │   ├── ROADMAP.md           # Roadmap y fases
│   │   ├── GROWTH.md            # Estrategia comercial (Perplexity-only)
│   │   ├── DESIGN.md            # Identidad visual (Perplexity-only)
│   │   ├── INDEX.md             # Índice OKF
│   │   ├── AGENTS-PROTOCOL.md   # Protocolo Perplexity ↔ Claude Code
│   │   └── TROUBLESHOOTING.md   # Errores recurrentes
│   ├── ARCHIVE/
│   │   └── SESSIONS.md          # Log de sesiones pasadas
│   └── README.md
│
└── arandil/                     # Monorepo principal
    ├── apps/
    │   └── mobile/              # React Native + Expo app
    │       ├── app/             # Expo Router (file-based routing)
    │       ├── components/
    │       ├── stores/          # Zustand (user, session, fsrs, theme)
    │       ├── services/        # API client (React Query)
    │       └── package.json
    │
    ├── services/
    │   └── api/                 # Express backend
    │       ├── src/
    │       │   ├── routes/      # Solo define rutas
    │       │   ├── controllers/ # Lógica de endpoints
    │       │   ├── services/    # IA, FSRS, TTS, WhatsApp
    │       │   ├── workers/     # BullMQ jobs (question-gen, tts)
    │       │   ├── db/
    │       │   │   ├── client.ts         # Pool pg singleton
    │       │   │   └── migrations/       # 001_init.sql, 002_...
    │       │   ├── middleware/  # auth, rateLimit, errorHandler
    │       │   └── index.ts
    │       └── package.json
    │
    ├── packages/
    │   ├── core/                # Algoritmos compartidos (puro, sin deps)
    │   │   ├── fsrs/            # Wrapper ts-fsrs
    │   │   ├── bkt/             # Bayesian Knowledge Tracing
    │   │   ├── irt/             # Item Response Theory (diagnóstico)
    │   │   └── types/           # Interfaces User, Question, Card, Session
    │   │
    │   └── curriculum/          # Curriculum matemático en .md
    │       └── mathematics/
    │           ├── algebra/
    │           ├── geometry/
    │           ├── trigonometry/
    │           └── calculus/
    │
    ├── infra/
    │   ├── docker-compose.yml   # PostgreSQL + Redis local
    │   └── scripts/             # db-migrate.sh, seed.sh
    │
    ├── turbo.json
    ├── pnpm-workspace.yaml
    ├── package.json
    └── .env.example
```

---

## Diagrama de Componentes

```
┌─────────────────────────────────────────────────────────────────┐
│                         ESTUDIANTE                              │
│                    (Mobile App — React Native)                  │
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
│  │  Onboarding │  │   Sesión    │  │  Progreso   │            │
│  │  (nivel +   │  │   (MCQ +    │  │  (curvas    │            │
│  │  enfoque)   │  │  feedback)  │  │   BKT)      │            │
│  └─────────────┘  └─────────────┘  └─────────────┘            │
│                                                                 │
│  ┌──────────────────────────────────────────────────┐          │
│  │  Estado Local (Zustand + AsyncStorage)           │          │
│  │  - user, session, fsrs (offline-first)           │          │
│  └──────────────────────────────────────────────────┘          │
└────────────────────┬────────────────────────────────────────────┘
                     │ HTTP (React Query)
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                     BACKEND API (Express)                       │
│                                                                 │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌──────────┐ │
│  │   /auth    │  │ /questions │  │ /sessions  │  │  /fsrs   │ │
│  │ (Supabase) │  │ (MCQ CRUD) │  │ (historial)│  │ (update) │ │
│  └────────────┘  └────────────┘  └────────────┘  └──────────┘ │
│                                                                 │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐               │
│  │    /ai     │  │ /whatsapp  │  │ /schedule  │               │
│  │ (DeepSeek  │  │ (Twilio    │  │ (cron      │               │
│  │  proxy)    │  │  webhook)  │  │  jobs)     │               │
│  └────────────┘  └────────────┘  └────────────┘               │
│                                                                 │
│  ┌──────────────────────────────────────────────────┐          │
│  │  BullMQ Workers (Redis)                          │          │
│  │  - question-gen: genera MCQ con DeepSeek         │          │
│  │  - tts: convierte texto a audio → R2             │          │
│  └──────────────────────────────────────────────────┘          │
└────────────────────┬────────────────────────────────────────────┘
                     │ pg driver
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                    PostgreSQL (Render)                          │
│                                                                 │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐       │
│  │  users   │  │questions │  │  cards   │  │ sessions │       │
│  │          │  │ (MCQ     │  │ (FSRS    │  │ (history)│       │
│  │ - id     │  │  banco)  │  │  params) │  │          │       │
│  │ - email  │  │          │  │          │  │          │       │
│  │ - level  │  │          │  │          │  │          │       │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘       │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                      SERVICIOS EXTERNOS                         │
│                                                                 │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌──────────┐ │
│  │ Supabase   │  │ DeepSeek   │  │ Cloudflare │  │ Twilio   │ │
│  │ Auth       │  │ V3 API     │  │ R2         │  │ WhatsApp │ │
│  │ (JWT)      │  │ (IA)       │  │ (storage)  │  │ API      │ │
│  └────────────┘  └────────────┘  └────────────┘  └──────────┘ │
│                                                                 │
│  ┌────────────┐  ┌────────────┐                                │
│  │ RevenueCat │  │ Redis      │                                │
│  │ (subs)     │  │ (Upstash)  │                                │
│  └────────────┘  └────────────┘                                │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                         APODERADO                               │
│                      (WhatsApp — Twilio)                        │
│                                                                 │
│  ┌──────────────────────────────────────────────────┐          │
│  │  Reportes diarios automáticos:                   │          │
│  │  - Racha de estudio                              │          │
│  │  - Temas del día                                 │          │
│  │  - Tiempo total                                  │          │
│  │  - Problemas resueltos                           │          │
│  └──────────────────────────────────────────────────┘          │
│                                                                 │
│  ┌──────────────────────────────────────────────────┐          │
│  │  Comandos:                                       │          │
│  │  - BIEN: confirma recepción                      │          │
│  │  - MAL: alerta para revisar con estudiante       │          │
│  │  - ENFERMO / VIAJE: pausa algoritmo de racha     │          │
│  └──────────────────────────────────────────────────┘          │
└─────────────────────────────────────────────────────────────────┘
```

---

## Flujo Principal — Sesión de Estudio

```
1. Usuario abre app mobile
   │
   ├─→ Si no hay token JWT → Redirige a /auth/login (Supabase)
   │
   └─→ Si hay token → Carga Dashboard

2. Dashboard muestra:
   - Racha de estudio (días consecutivos)
   - Cards pendientes de repaso (FSRS)
   - Progreso por tema (BKT)

3. Usuario inicia sesión
   │
   ├─→ App consulta FSRS local (offline): ¿qué cards revisar hoy?
   │
   └─→ Si no hay cards locales: fetch next 30 desde API

4. Sesión activa (loop):
   │
   ├─→ Renderiza MCQCard con:
   │   - Stem (enunciado)
   │   - 4-5 opciones
   │   - Timer visible
   │
   ├─→ Usuario selecciona opción
   │
   ├─→ Feedback inmediato (300ms):
   │   - Color verde/rojo en opción
   │   - Ícono ✓ / ✗
   │
   ├─→ Explicación paso a paso (si activado)
   │   - Modo socrático: IA genera hint, no respuesta directa
   │
   ├─→ Actualiza FSRS local:
   │   - Calcula next review date
   │   - Ajusta difficulty, stability
   │
   └─→ Next question (loop hasta tiempo o límite)

5. Sesión completa
   │
   ├─→ Muestra resumen:
   │   - Problemas resueltos
   │   - Tasa de acierto
   │   - Temas reforzados
   │
   ├─→ Animación de logro (Rive) si racha continúa
   │
   └─→ Sync con API (background):
       - POST /sessions (historial)
       - PATCH /fsrs (actualizar cards)
       - GET /questions/next (pre-descargar 30 nuevas)

6. Cron job diario (6 PM):
   │
   └─→ API envía reporte WhatsApp a apoderado
       - Racha, temas, tiempo, problemas resueltos
```

---

## Mapa de Módulos Heredados vs Nuevos

| Módulo | Origen | Estado | Cambios requeridos |
|--------|--------|--------|-------------------|
| **Monorepo config** | Arandur | ✅ Copiar directo | Renombrar `@arandur/*` a `@arandil/*` en package.json |
| **Express API base** | Arandur | ✅ Copiar + adaptar | Eliminar rutas específicas de admisión |
| **Auth (Supabase)** | Arandur | ✅ Copiar directo | Ninguno |
| **DB client (pg)** | Arandur | ✅ Copiar directo | Ninguno |
| **Middleware** | Arandur | ✅ Copiar directo | Ninguno (auth, rateLimit, errorHandler) |
| **BullMQ workers** | Arandur | ✅ Copiar + adaptar | Cambiar prompts IA |
| **FSRS-7 (core)** | Arandur | ✅ Copiar directo | Ninguno (algoritmo puro) |
| **BKT (core)** | Arandur | ✅ Copiar directo | Ninguno (algoritmo puro) |
| **IRT (core)** | Arandur | ✅ Copiar directo | Ninguno (algoritmo puro) |
| **Mobile nav (Expo Router)** | Arandur | ✅ Copiar + adaptar | Cambiar copy de pantallas |
| **Zustand stores** | Arandur | ✅ Copiar + adaptar | Eliminar `exam_target`, `universidad_objetivo` |
| **React Query** | Arandur | ✅ Copiar directo | Ninguno (pattern) |
| **Onboarding flow** | Arandur | ❌ Crear nuevo | Eliminar flujo universidad/carrera |
| **Dashboard UI** | Arandur | 🔄 Adaptar | Cambiar "días hasta examen" → "racha de estudio" |
| **IA prompts** | Arandur | ❌ Crear nuevo | Prompts matemáticas (no admisión) |
| **Curriculum** | Arandur | ❌ Crear nuevo | Estructura `/mathematics/` |
| **DB migrations** | Arandur | ❌ Crear nuevo | Schema sin enums de universidades |
| **RevenueCat** | Arandur | ✅ Copiar + adaptar | Cambiar pricing regional |
| **Twilio/WhatsApp** | Arandur | ✅ Copiar + adaptar | Adaptar copy de reportes |
| **Cloudflare R2** | Arandur | ✅ Copiar directo | Ninguno (storage) |

**Leyenda:**
- ✅ Copiar directo: 0 cambios de lógica
- 🔄 Adaptar: cambios menores (copy, naming)
- ❌ Crear nuevo: módulo no reutilizable

---

## Dependencias Críticas

### Backend (services/api/package.json)
```json
{
  "dependencies": {
    "@arandil/core": "workspace:*",
    "@supabase/supabase-js": "^2.46.2",
    "express": "^4.21.2",
    "pg": "^8.13.1",
    "zod": "^3.24.1",
    "bullmq": "^5.76.5",
    "ioredis": "^5.4.1",
    "openai": "^4.77.0",
    "twilio": "^5.3.7",
    "@aws-sdk/client-s3": "^3.1063.0"
  }
}
```

### Mobile (apps/mobile/package.json)
```json
{
  "dependencies": {
    "@arandil/core": "workspace:*",
    "expo": "~52.0.0",
    "expo-router": "~4.0.9",
    "react-native": "0.76.9",
    "zustand": "^5.0.2",
    "@tanstack/react-query": "^5.62.16",
    "rive-react-native": "^9.0.0"
  }
}
```

### Core (packages/core/package.json)
```json
{
  "dependencies": {
    "ts-fsrs": "^4.x",
    "zod": "^3.24.1"
  }
}
```

**Regla crítica:** `packages/core` NUNCA debe importar Express, React, pg ni ningún runtime específico. Solo lógica pura + tipos.

---

## Próximos pasos arquitectónicos

1. **Crear DB schema inicial** (DEC-009 pendiente)
   - Migrations: 001_init.sql sin enums de universidades
   - Tabla `users` con `subject_focus` en lugar de `exam_target`
   
2. **Definir curriculum matemático** (DEC-010 pendiente)
   - Estructura `/mathematics/algebra/`, `/geometry/`, etc.
   - Archivos .md por tema (DeepSeek los lee para generar MCQ)

3. **Refactor prompts DeepSeek** (DEC-011 pendiente)
   - Eliminar referencias a "prospecto UNI", "examen admisión"
   - Crear prompts para generación de problemas matemáticos

4. **Nuevo onboarding flow** (DEC-012 pendiente)
   - Pantalla 1: Nivel (secundaria / universitario)
   - Pantalla 2: Área de enfoque inicial (álgebra / geometría / cálculo)
   - Pantalla 3: Diagnóstico IRT (15 preguntas adaptativas)

---

**Última actualización:** 2026-07-04  
**Próxima revisión:** Al completar bootstrap de monorepo (FASE 2-3)
