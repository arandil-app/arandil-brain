---
type: concept
title: "Arandil — Roadmap y Fases"
description: "Fases del proyecto, hitos y estado actual del desarrollo"
tags: [roadmap, fases, hitos, mvp]
timestamp: 2026-07-04
refs:
  - VISION.md
  - DECISIONS.md
  - ARCHITECTURE.md
---

# Arandil — Roadmap y Fases

> **Estado actual:** FASE 0 (Bootstrap) — creando estructura base del proyecto

---

## FASE 0 — Bootstrap (2026-07-04 → 2026-07-05)

**Objetivo:** Estructura base del proyecto lista para desarrollo

### Tareas
- [x] Leer PROJECT-TEMPLATE.md y Arandur para diagnóstico
- [x] Crear `arandil-brain/` con estructura CURRENT/ARCHIVE
- [x] Crear archivos obligatorios con frontmatter OKF:
  - [x] VISION.md
  - [x] DECISIONS.md
  - [x] ARCHITECTURE.md
  - [x] ROADMAP.md (este archivo)
  - [ ] STATUS.md
  - [ ] GROWTH.md (placeholder Perplexity-only)
  - [ ] DESIGN.md (placeholder Perplexity-only)
  - [ ] INDEX.md
  - [ ] AGENTS-PROTOCOL.md
  - [ ] TROUBLESHOOTING.md (vacío)
  - [ ] README.md
- [ ] Git init + remote setup brain
- [ ] Primer commit + push brain

### Entregables
- ✅ Diagnóstico documentado (qué copiar de Arandur, riesgos, plan)
- 🟡 Brain completo con todos los archivos obligatorios
- ⚪ Brain pusheado a GitHub `arandil-app/arandil-brain`

**Duración estimada:** 1 día

---

## FASE 1 — Monorepo Base (2026-07-05 → 2026-07-06)

**Objetivo:** Monorepo técnico funcional sin lógica de negocio

### Tareas
- [ ] Crear estructura monorepo:
  ```
  arandil/
  ├── apps/mobile/
  ├── services/api/
  ├── packages/core/
  ├── infra/
  ├── turbo.json
  ├── pnpm-workspace.yaml
  └── package.json
  ```
- [ ] Copiar y adaptar configuración Turborepo de Arandur
- [ ] Copiar Docker Compose (PostgreSQL + Redis)
- [ ] Copiar scripts de infraestructura (`infra/scripts/`)
- [ ] Crear .gitignore adaptado
- [ ] Crear .env.example limpio (sin vars específicas de Arandur)
- [ ] Verificar `pnpm install` funciona
- [ ] Verificar `pnpm build` funciona
- [ ] Git init + remote setup monorepo
- [ ] Primer commit + push monorepo

### Entregables
- ⚪ Monorepo corriendo localmente
- ⚪ Builds pasando (empty apps por ahora)
- ⚪ Docker Compose levanta PostgreSQL + Redis
- ⚪ Monorepo pusheado a GitHub `arandil-app/arandil`

**Duración estimada:** 1 día

---

## FASE 2 — API Base (2026-07-06 → 2026-07-07)

**Objetivo:** Backend Express funcional con auth y DB

### Tareas
- [ ] Copiar estructura base API de Arandur:
  - [ ] `src/index.ts` (servidor Express)
  - [ ] `src/db/client.ts` (pool pg)
  - [ ] `src/middleware/` (auth, rateLimit, errorHandler)
  - [ ] `src/routes/auth.ts` (Supabase login/registro)
- [ ] Crear migraciones NUEVAS (no copiar de Arandur):
  - [ ] 001_init.sql (users, sessions, cards, questions — sin enums universidades)
  - [ ] 002_subscriptions.sql (RevenueCat integration)
- [ ] Script de migración: `infra/scripts/migrate.sh`
- [ ] Tests base:
  - [ ] Vitest config
  - [ ] Test de health check (`GET /health`)
  - [ ] Test de auth middleware
- [ ] Verificar `pnpm dev` en API corre sin errores
- [ ] Verificar migraciones aplican correctamente
- [ ] Commit + push

### Entregables
- ⚪ API corriendo en `localhost:3000`
- ⚪ Auth funcional (Supabase JWT)
- ⚪ DB schema base aplicado
- ⚪ Tests base pasando

**Duración estimada:** 1 día

---

## FASE 3 — Mobile Scaffold (2026-07-07 → 2026-07-08)

**Objetivo:** App mobile con navegación y autenticación

### Tareas
- [ ] Copiar estructura base mobile de Arandur:
  - [ ] `app/` (Expo Router navigation)
  - [ ] `components/` (sin copy específico de admisión)
  - [ ] `stores/` (Zustand: user, session, fsrs, theme)
  - [ ] `services/api.ts` (React Query)
- [ ] Adaptar onboarding:
  - [ ] Eliminar flujo universidad/carrera
  - [ ] Crear flujo nivel (secundaria/universitario) + materia inicial
- [ ] Adaptar dashboard:
  - [ ] Eliminar "días hasta examen"
  - [ ] Agregar "racha de estudio"
  - [ ] Mantener curvas BKT (pero por temas matemáticos)
- [ ] Conectar mobile con API local
- [ ] Verificar login/registro funciona
- [ ] Commit + push

### Entregables
- ⚪ App mobile corriendo en simulador/dispositivo
- ⚪ Login/registro con Supabase funcional
- ⚪ Dashboard muestra datos mock

**Duración estimada:** 1 día

---

## FASE 4 — FSRS + Core Algorithms (2026-07-08 → 2026-07-09)

**Objetivo:** Algoritmos de repetición espaciada funcionando

### Tareas
- [ ] Copiar `packages/core/` de Arandur:
  - [ ] `fsrs/` (wrapper ts-fsrs)
  - [ ] `bkt/` (Bayesian Knowledge Tracing)
  - [ ] `irt/` (Item Response Theory)
  - [ ] `types/` (interfaces)
- [ ] Adaptar types:
  - [ ] Eliminar `exam_target`, `universidad_objetivo` de User
  - [ ] Agregar `subject_focus`, `learning_goal`
- [ ] Endpoints API:
  - [ ] POST /sessions (crear sesión)
  - [ ] PATCH /fsrs (actualizar cards tras respuesta)
  - [ ] GET /sessions/history
- [ ] Tests de FSRS:
  - [ ] Verificar cálculo de next review date
  - [ ] Verificar actualización de difficulty/stability
- [ ] Conectar mobile con endpoints FSRS
- [ ] Commit + push

### Entregables
- ⚪ FSRS funcionando end-to-end
- ⚪ Sesión de estudio básica funcional (sin IA aún)
- ⚪ Tests de algoritmos pasando

**Duración estimada:** 1 día

---

## FASE 5 — IA Integration (2026-07-09 → 2026-07-11)

**Objetivo:** DeepSeek generando problemas matemáticos

### Tareas
- [ ] Crear prompts DeepSeek para matemáticas:
  - [ ] Prompt generación MCQ (álgebra, geometría, cálculo)
  - [ ] Prompt modo socrático (hints, no respuestas directas)
  - [ ] Prompt explicaciones paso a paso
- [ ] Crear `packages/curriculum/mathematics/`:
  - [ ] algebra/ (ecuaciones, polinomios, funciones...)
  - [ ] geometry/ (triángulos, círculos, áreas...)
  - [ ] trigonometry/ (identidades, funciones...)
  - [ ] calculus/ (límites, derivadas, integrales...)
- [ ] Copiar y adaptar `services/api/src/services/ai.service.ts`
- [ ] Copiar BullMQ worker: `question-gen.worker.ts`
- [ ] Endpoint:
  - [ ] POST /ai/generate (genera MCQ por tema)
  - [ ] POST /ai/explain (explicación paso a paso)
- [ ] Tests:
  - [ ] Validación Zod de respuestas IA
  - [ ] Retry logic si JSON inválido
  - [ ] Fallback a banco estático si IA falla
- [ ] Commit + push

### Entregables
- ⚪ IA genera problemas matemáticos válidos
- ⚪ Sesión de estudio con problemas IA funcional
- ⚪ Modo socrático funcional

**Duración estimada:** 2 días

---

## FASE 6 — MVP Completo (2026-07-11 → 2026-07-15)

**Objetivo:** MVP técnico funcional end-to-end

### Tareas pendientes
- [ ] RevenueCat integration:
  - [ ] Webhook handler
  - [ ] Tier-based access control
  - [ ] Free tier: 10 sesiones/mes
  - [ ] Premium: ilimitado
- [ ] Reportes WhatsApp:
  - [ ] Cron job diario 6 PM
  - [ ] Comando parser (BIEN, MAL, ENFERMO, VIAJE)
  - [ ] Pausa de racha por contexto apoderado
- [ ] Offline support:
  - [ ] Pre-descarga de 30 preguntas tras sesión
  - [ ] Queue de respuestas offline → sync al reconectar
- [ ] UI polish:
  - [ ] Animaciones Rive (logros)
  - [ ] Dark mode
  - [ ] Feedback visual de respuesta (300ms color)
- [ ] Tests e2e:
  - [ ] Flujo completo: onboarding → sesión → FSRS update
  - [ ] Offline → online sync
- [ ] Documentación:
  - [ ] .env.example completo
  - [ ] README con setup local
  - [ ] TROUBLESHOOTING.md con errores conocidos
- [ ] Commit + push

### Entregables
- ⚪ MVP técnico completo funcional
- ⚪ Todos los tests pasando
- ⚪ Documentación actualizada

**Duración estimada:** 4 días

---

## FASE 7 — Deploy Staging (2026-07-15 → 2026-07-18)

**Objetivo:** App funcionando en staging con usuarios de prueba

### Tareas
- [ ] Deploy API:
  - [ ] Render (backend)
  - [ ] PostgreSQL managed (Render o Supabase)
  - [ ] Redis (Upstash)
- [ ] Deploy mobile:
  - [ ] EAS Build (Expo Application Services)
  - [ ] Internal testing track (TestFlight iOS, Google Play beta)
- [ ] Configurar secretos en producción:
  - [ ] Infisical (secrets management)
  - [ ] Supabase API keys
  - [ ] DeepSeek API key
  - [ ] Twilio credentials
  - [ ] RevenueCat webhook secret
- [ ] Smoke tests en staging:
  - [ ] Auth funciona
  - [ ] Sesión genera preguntas
  - [ ] FSRS actualiza correctamente
  - [ ] Reporte WhatsApp se envía
- [ ] Commit + push

### Entregables
- ⚪ API en staging accesible
- ⚪ Mobile app en TestFlight/Google Play beta
- ⚪ 5 usuarios de prueba confirmados

**Duración estimada:** 3 días

---

## FASE 8 — Beta Cerrada (2026-07-18 → 2026-08-01)

**Objetivo:** Validación con 50 usuarios reales

### Tareas
- [ ] Reclutamiento:
  - [ ] 50 usuarios beta (estudiantes secundaria/universitarios)
  - [ ] Mix: Perú, México, Colombia
  - [ ] Onboarding manual (video call o doc)
- [ ] Monitoreo:
  - [ ] Sentry (error tracking)
  - [ ] Logs en Pino (API)
  - [ ] Analytics básicos (sesiones/día, tasa de retención)
- [ ] Iteración rápida:
  - [ ] Fix bugs críticos en <24h
  - [ ] Ajustar prompts IA según feedback
  - [ ] Ajustar dificultad FSRS si necesario
- [ ] Entrevistas:
  - [ ] 10 usuarios: feedback profundo
  - [ ] ¿Qué les gustó? ¿Qué falta?
  - [ ] ¿Pagarían S/29/mes?
- [ ] Commit + push fixes

### Entregables
- ⚪ 50 usuarios activos en beta
- ⚪ Tasa de retención D7 > 40%
- ⚪ 0 bugs críticos pendientes
- ⚪ Feedback documentado en brain

**Duración estimada:** 2 semanas

---

## FASE 9 — Lanzamiento Público v1.0 (2026-08-01 → 2026-09-01)

**Objetivo:** Producto en producción con modelo de negocio activo

### Tareas
- [ ] Landing page:
  - [ ] Astro + Tailwind
  - [ ] Copy: visión, features, pricing
  - [ ] CTA: descarga app
  - [ ] Deploy: Netlify
- [ ] Pricing:
  - [ ] Free: 10 sesiones/mes
  - [ ] Premium: S/29/mes Perú, MXN 150 México, USD 8 resto
  - [ ] RevenueCat configurado con pricing regional
- [ ] Marketing inicial:
  - [ ] Anuncio en redes (TikTok, Instagram)
  - [ ] Post en ProductHunt
  - [ ] Comunidades: Discord de estudiantes, grupos WhatsApp
- [ ] Soporte:
  - [ ] Email: hola@arandil.app
  - [ ] FAQ en landing
  - [ ] Discord community (v1.1)
- [ ] Monitoreo producción:
  - [ ] Uptime monitoring (BetterUptime)
  - [ ] Error tracking (Sentry)
  - [ ] Analytics (PostHog o Mixpanel)

### Entregables
- ⚪ App en producción pública
- ⚪ Landing page live
- ⚪ 100 usuarios en primeras 2 semanas
- ⚪ 10 suscripciones pagadas

**Duración estimada:** 1 mes

---

## Post-v1.0 — Roadmap Futuro

### v1.1 — Optimizaciones (Sep 2026)
- [ ] Modo socrático mejorado (Claude fallback)
- [ ] Lectura veloz (bionic reading en enunciados)
- [ ] Modo RSVP (palabra por palabra)
- [ ] Discord community integration
- [ ] Leaderboard semanal (opcional, no competitivo)

### v1.5 — Expansión Materias (Nov 2026)
- [ ] Física (cinemática, dinámica, energía)
- [ ] Química (estequiometría, ácido-base, orgánica)
- [ ] Comunicación (comprensión lectora, redacción)

### v2.0 — Modo Colaborativo (Ene 2027)
- [ ] Sesiones en vivo con amigos
- [ ] Desafíos grupales
- [ ] Chat de estudio
- [ ] Modo profesor (crear sesiones para alumnos)

---

## Métricas de Éxito por Fase

| Fase | Métrica | Target |
|------|---------|--------|
| FASE 6 (MVP) | Tests pasando | 100% |
| FASE 7 (Staging) | Deploy exitoso | ✅ |
| FASE 8 (Beta) | Usuarios activos D7 | 50 |
| FASE 8 (Beta) | Retención D7 | >40% |
| FASE 8 (Beta) | Bugs críticos | 0 |
| FASE 9 (v1.0) | Usuarios primeras 2 semanas | 100 |
| FASE 9 (v1.0) | Conversión free→paid | >10% |
| FASE 9 (v1.0) | Churn mes 1 | <20% |

---

**Última actualización:** 2026-07-04  
**Próxima revisión:** Al completar FASE 0 (Bootstrap)
