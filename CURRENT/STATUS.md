---
type: log
title: "Arandil — Estado Actual"
description: "Estado volátil del proyecto, actualizado cada sesión"
tags: [status, estado, sesion, alertas]
timestamp: 2026-07-04
volatile: true
---

# Arandil — Estado Actual

> **Actualizado:** 2026-07-04  
> **Sesión:** FASE 0 + FASE 1 + FASE 2 completas  
> **Última actualización por:** Claude Code

---

## Estado General

| Dimensión | Estado | Detalles |
|-----------|--------|----------|
| **Fase actual** | ✅ FASE 2 completada | API base funcional, migraciones aplicadas |
| **Brain** | ✅ Completo | Pusheado a GitHub |
| **Monorepo** | ✅ Completo | Pusheado a GitHub (commit 8f18cf1) |
| **API** | ✅ Funcional | GET /health OK, tests 2/2 pasando |
| **Mobile** | ⚪ Pendiente | FASE 3 (siguiente) |
| **Tests** | ✅ API tests OK | 2/2 pasando (health route) |
| **Deploy** | ⚪ Pendiente | FASE 7 |

---

## Completado Hoy

### 2026-07-04 — Bootstrap inicial

**Brain:**
- ✅ Diagnóstico completo (qué copiar de Arandur, riesgos)
- ✅ VISION.md creado con visión de producto
- ✅ DECISIONS.md creado con DEC-001 a DEC-008
- ✅ ARCHITECTURE.md creado con stack y diagrama de componentes
- ✅ ROADMAP.md creado con fases 0-9
- ✅ STATUS.md creado (este archivo)
- ✅ GROWTH.md creado (placeholder Perplexity-only)
- ✅ DESIGN.md creado (placeholder Perplexity-only)
- ✅ INDEX.md creado
- ✅ AGENTS-PROTOCOL.md creado
- ✅ TROUBLESHOOTING.md creado (vacío)
- ✅ README.md creado
- ✅ ARCHIVE/SESSIONS.md creado

**Workspace:**
- ✅ CLAUDE.md creado en workspace root

**Git:**
- ✅ Brain git init
- ✅ Commits brain: 57d50ae, 6f7c02d
- ✅ Remote configurado
- ✅ Push exitoso a https://github.com/arandil-app/arandil-brain

### 2026-07-04 — FASE 1: Monorepo Base

**Monorepo (`arandil/`):**
- ✅ Estructura creada: apps/mobile, services/api, packages/core, infra/
- ✅ Configuración Turborepo copiada y adaptada de Arandur
- ✅ Docker Compose: PostgreSQL 16 + Redis 7 (adaptado arandil)
- ✅ package.json root con scripts dev/build/test/lint
- ✅ pnpm-workspace.yaml configurado
- ✅ turbo.json con tasks build/dev/test/lint
- ✅ .gitignore copiado de Arandur
- ✅ .env.example con todas las vars necesarias
- ✅ README.md con setup instructions
- ✅ package.json placeholders para mobile/api/core

**Git:**
- ✅ Repo inicializado
- ✅ Primer commit (b2a8c39)
- ✅ Repo creado en GitHub: https://github.com/arandil-app/arandil
- ✅ Push exitoso

### 2026-07-04 — FASE 2: API Base Funcional

**API (`services/api/src/`):**
- ✅ index.ts: servidor Express con helmet, cors, error handling
- ✅ lib/env.ts: validación Zod de env vars (19 vars requeridas)
- ✅ lib/logger.ts: Pino logger con pino-pretty en dev
- ✅ db/client.ts: Pool pg singleton
- ✅ middleware/errorHandler.ts: error handler + notFound middleware
- ✅ middleware/auth.ts: Supabase JWT validation middleware
- ✅ routes/health.ts: GET /health con DB check

**Migraciones (`db/migrations/`):**
- ✅ 001_init.sql: users, cards, sessions, questions, session_responses, usage_counters
  - Schema global sin enums de universidades
  - `subject_focus` en lugar de `exam_target`
  - `topic` en questions (algebra, geometry, calculus)
- ✅ 002_subscriptions.sql: subscriptions + subscription_events (RevenueCat)

**Tests:**
- ✅ __tests__/health.test.ts: 2 tests pasando (200 OK, 503 error)
- ✅ vitest.config.ts configurado

**Scripts:**
- ✅ infra/scripts/db-migrate.sh copiado de Arandur

**Verificación local:**
- ✅ pnpm install (sin errores)
- ✅ pnpm test (2/2 tests pasando)
- ✅ pnpm docker:dev (PostgreSQL + Redis corriendo)
- ✅ pnpm db:migrate (migraciones aplicadas)
- ✅ pnpm dev:plain (API corriendo en :3000)
- ✅ curl /health → status: ok ✅

**Git:**
- ✅ Commit 8f18cf1
- ✅ Push exitoso

**Reutilizado de Arandur:**
- Estructura API (lib, middleware, routes, db)
- Validación env con Zod
- Pino logger pattern
- Pool pg singleton
- Health check pattern

**Adaptado para Arandil:**
- Sin enums universidades (UNI, UNMSM, etc)
- Schema global (subject_focus, topic)
- Sin PDF upload feature
- Sin workers BullMQ por ahora
- Sin cron jobs por ahora

---

## Pendientes Inmediatos

### FASE 3 — Mobile Scaffold (siguiente)
1. [ ] Copiar estructura base mobile de Arandur
2. [ ] Adaptar onboarding (eliminar universidad/carrera)
3. [ ] Crear onboarding matemáticas (nivel + materia inicial)
4. [ ] Adaptar dashboard (racha de estudio, NO "días hasta examen")
5. [ ] Conectar mobile con API local
6. [ ] Verificar login/registro funciona
7. [ ] Commit + push

### FASE 4 — FSRS + Core Algorithms
1. [ ] Copiar packages/core/ de Arandur
2. [ ] Adaptar types (eliminar exam_target, universidad_objetivo)
3. [ ] Endpoints FSRS en API (POST /sessions, PATCH /fsrs)
4. [ ] Tests FSRS
5. [ ] Commit + push

---

## Bloqueantes

### 🔴 Requieren Acción Externa
- Ninguno

### 🟡 Requieren Decisión de Rodhan
- Ninguno por ahora

---

## Alertas para Perplexity

- Ninguna por ahora

---

## Commits Recientes

**Brain (arandil-brain):**
- `57d50ae` — feat: initial brain setup — OKF structure (2026-07-04)
- `6f7c02d` — docs: update STATUS after bootstrap completion (2026-07-04)

**Monorepo (arandil):**
- `b2a8c39` — feat: initial monorepo setup — base structure (2026-07-04)
- `8f18cf1` — feat(FASE-2): API base funcional — Express + Auth + Migraciones (2026-07-04)

---

## Métricas

| Métrica | Valor | Target FASE 0+1+2 |
|---------|-------|-------------------|
| Archivos obligatorios brain | 11/11 ✅ | 11/11 |
| Frontmatter OKF válido | 11/11 (100%) ✅ | 100% |
| Repos git inicializados | 2/2 ✅ | 2/2 |
| Commits totales | 5 ✅ | 2+ |
| Repos pusheados a GitHub | 2/2 ✅ | 2/2 |
| Monorepo workspaces | 3 (mobile, api, core) | 3 |
| Docker services | 2 (PostgreSQL, Redis) ✅ | 2 |
| API routes | 1 (GET /health) ✅ | 1+ |
| DB migrations | 2 (001_init, 002_subscriptions) ✅ | 2+ |
| Tests pasando | 2/2 (health route) ✅ | 1+ |
| DB tablas | 8 (users, cards, sessions, questions, etc) ✅ | 5+ |

---

## Próximos Pasos

### FASE 3 — Mobile Scaffold (siguiente)
1. Copiar estructura base mobile de Arandur:
   - `app/` (Expo Router navigation)
   - `components/` (sin copy específico de admisión)
   - `stores/` (Zustand: user, session, fsrs, theme)
   - `services/api.ts` (React Query)

2. Adaptar onboarding:
   - Eliminar flujo universidad/carrera
   - Crear flujo nivel (secundaria/universitario) + materia inicial

3. Adaptar dashboard:
   - Eliminar "días hasta examen"
   - Agregar "racha de estudio"
   - Mantener curvas BKT (pero por temas matemáticos)

4. Conectar mobile con API local
5. Verificar login/registro funciona
6. Commit + push

### FASE 4 — FSRS + Core Algorithms
1. Copiar `packages/core/` de Arandur
2. Adaptar types (eliminar exam_target, universidad_objetivo)
3. Endpoints FSRS en API:
   - POST /sessions (crear sesión)
   - PATCH /fsrs (actualizar cards tras respuesta)
   - GET /sessions/history
4. Tests FSRS
5. Conectar mobile con endpoints FSRS

---

**Última sesión:** 2026-07-04  
**Próxima sesión:** FASE 3 — Mobile Scaffold (React Native + Expo + Onboarding matemáticas)
