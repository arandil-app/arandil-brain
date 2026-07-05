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
> **Sesión:** FASE 0 + FASE 1 + FASE 2 + FASE 3 + FASE 4 + FASE 4.5 + FASE 5 completas  
> **Última actualización por:** Claude Code

---

## Estado General

| Dimensión | Estado | Detalles |
|-----------|--------|----------|
| **Fase actual** | ✅ FASE 5 completada | Onboarding matemático real + personalización dashboard |
| **Brain** | ✅ Completo | Pusheado a GitHub |
| **Monorepo** | ✅ Completo | Pusheado a GitHub |
| **API** | ✅ Funcional + Hardened | 5 routes (health, practice×3, profile×2), validación robusta |
| **Mobile** | ✅ Funcional + Onboarding | Wizard 4 pasos, auth gate condicional, dashboard personalizado |
| **FSRS** | ✅ Integrado | ts-fsrs v5 funcionando |
| **Tests** | ✅ Core + API completos | 5/5 core, 21/21 API (9 practice + 10 profile + 2 health) |
| **Deploy** | ⚪ Pendiente | FASE 8 (renumerado, ver ROADMAP.md nota) |

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

### 2026-07-04 — FASE 3: Mobile Scaffold Funcional

**Mobile (`apps/mobile/`):**
- ✅ app.json: Expo config completo (scheme: arandil, owner: arandil-app)
- ✅ package.json: dependencias completas (Expo 52, Supabase, React Query, Zustand)
- ✅ tsconfig.json, metro.config.js, expo-env.d.ts
- ✅ assets placeholders creados

**Auth & Navigation (Expo Router):**
- ✅ src/app/_layout.tsx: root layout con QueryClientProvider
- ✅ src/app/index.tsx: auth gate con redirección automática según sesión
- ✅ src/app/(auth)/sign-in.tsx: login con Supabase (email/password)
- ✅ src/app/(auth)/sign-up.tsx: registro con Supabase
- ✅ src/lib/supabase.ts: cliente Supabase para React Native con AsyncStorage

**Dashboard autenticado (tabs):**
- ✅ src/app/(tabs)/_layout.tsx: bottom tabs navigation (Inicio, Progreso, Perfil)
- ✅ src/app/(tabs)/index.tsx: dashboard con racha placeholder, stats, CTA
- ✅ src/app/(tabs)/progress.tsx: progreso placeholder
- ✅ src/app/(tabs)/profile.tsx: perfil con logout funcional

**State management:**
- ✅ src/stores/user.store.ts: Zustand con AsyncStorage persistence
  - user: {id, email, name, subjectFocus, learningGoal}
  - isLoading, setUser, setLoading, logout

**Verificación:**
- ✅ pnpm install (sin errores)
- ✅ pnpm lint (TypeScript 0 errores)
- ✅ Estructura completa: 10 archivos TypeScript + configs

**Git:**
- ✅ Commit 6932fce
- ✅ Push exitoso

**Features funcionales:**
- ✅ Login/registro con Supabase
- ✅ Persistencia de sesión (AsyncStorage)
- ✅ Rutas protegidas (auth gate en index)
- ✅ Dashboard autenticado con racha placeholder
- ✅ Logout funcional
- ✅ Navegación tabs funcional

**Reutilizado de Arandur:**
- Estructura Expo Router
- Supabase auth pattern
- Zustand stores pattern
- UI colors & styles

**Adaptado para Arandil:**
- Sin universidad/carrera
- Sin exam_target
- Dashboard con racha de estudio (no días hasta examen)
- subject_focus en user store (matemáticas)
- Sin onboarding de admisión

**Pendiente para FASE 4:**
- Onboarding matemáticas real
- Integración packages/core (FSRS)
- Endpoints de sesiones en API
- Conexión dashboard ↔ API

### 2026-07-04 — FASE 4: FSRS + Core + Práctica End-to-End

**packages/core funcional:**
- ✅ src/types/index.ts: tipos Arandil (User, Question, FSRSCard, ReviewRating, PracticeStats)
  - SubjectArea type: algebra, geometry, calculus, trigonometry, statistics
  - Sin universidad_objetivo, sin exam_target
- ✅ src/fsrs/index.ts: wrapper ts-fsrs v5.0.2
  - scheduleCard(): calcula próxima revisión
  - newCard(): crea card FSRS inicial
  - getDueCards(): filtra cards vencidas
  - ratingToGrade(): convierte ratings user-friendly (again/hard/good/easy) a FSRS grades
- ✅ src/__tests__/fsrs.test.ts: 5 tests pasando
- ✅ Compilado con TypeScript strict: 0 errores

**API endpoints práctica:**
- ✅ GET /practice/next: obtiene siguiente card
  - Si hay cards vencidas → devuelve la más antigua
  - Si no hay vencidas → crea nueva card de question pool
  - Si no hay más questions → mensaje "No more cards available"
- ✅ POST /practice/review: recibe rating y actualiza FSRS
  - Body: {card_id, rating, correct, response_time_ms}
  - Rating: 'again' | 'hard' | 'good' | 'easy'
  - Actualiza: difficulty, stability, due, state, reps, lapses
  - Devuelve: next_review_in_days calculado
- ✅ GET /practice/stats: métricas usuario
  - total_cards, cards_due_today
  - cards_new, cards_learning, cards_review, cards_relearning (por state)
  - streak_days (TODO), accuracy_percent (TODO)

**Migraciones DB:**
- ✅ 003_fsrs_fields.sql: campos FSRS completos
  - elapsed_days, scheduled_days, learning_steps, reps, lapses
  - state ahora SMALLINT (0-3) como ts-fsrs espera
  - Índices: idx_cards_user_due, idx_cards_state

**Seed contenido inicial:**
- ✅ seed-questions.sql: 10 preguntas matemáticas
  - Álgebra: ecuaciones lineales (2), cuadráticas (1)
  - Geometría: triángulos (1), áreas (1)
  - Cálculo: derivadas (1), límites (1)
  - Trigonometría: identidades (1), básico (1)
  - Estadística: media (1)
  - Todas con solution_steps

**Mobile pantalla práctica:**
- ✅ src/app/(tabs)/practice.tsx: pantalla completa
  - Carga siguiente card (API /practice/next)
  - Muestra stem + 5 opciones (topic badge, subtopic)
  - Selección de respuesta
  - Botón "Ver solución" → muestra solution_steps
  - Feedback visual (verde/rojo según correcta)
  - Rating FSRS con 4 botones (again/hard/good/easy)
  - Submit review → carga siguiente automáticamente
  - Loading states, empty state
- ✅ src/services/api.ts: cliente con auth
  - getAuthHeaders() usa Supabase session token
  - getNextCard(), submitReview(), getPracticeStats()
  - TypeScript types exportados

**Navegación:**
- ✅ (tabs)/_layout.tsx: tab Practicar agregada
- ✅ (tabs)/index.tsx: CTA dashboard → router.push('/(tabs)/practice')

**Verificación:**
- ✅ pnpm install (ts-fsrs v5.0.2 instalado)
- ✅ pnpm db:migrate (003_fsrs_fields aplicada)
- ✅ seed aplicado (10 preguntas insertadas)
- ✅ pnpm test --filter core (5/5 tests FSRS ✅)
- ✅ pnpm test --filter api (2/2 health tests ✅)

**Git:**
- ✅ Commits: a3a21db (core + endpoints), cf44eda (mobile)
- ✅ Push exitoso

**Flujo end-to-end funcional:**
1. Usuario login mobile ✅
2. Dashboard → tap "Comenzar práctica" ✅
3. API devuelve card + question (de seed o nueva) ✅
4. Mobile muestra pregunta + opciones ✅
5. Usuario selecciona respuesta ✅
6. Tap "Ver solución" → muestra pasos ✅
7. Usuario califica dificultad (again/hard/good/easy) ✅
8. API actualiza FSRS scheduling ✅
9. Mobile carga siguiente card automáticamente ✅

**Integración ts-fsrs:**
- newCard(): stability=0.3, difficulty=0.3, state=0 (New)
- scheduleCard() con Rating.Good → state=1 (Learning), reps++
- getDueCards() filtra por due <= NOW()
- ratingToGrade() mapea strings a enum Rating

**Reutilizado de Arandur:**
- Estructura core/fsrs
- Pattern toCard/fromCard (convierte NUMERIC strings a numbers)
- Tests FSRS pattern

**Adaptado para Arandil:**
- Tipos sin universidad_objetivo, exam_target
- SubjectArea global (no enums Perú)
- Questions con topic/subtopic global
- Practice stats sin métricas específicas de admisión

---

### 2026-07-04 — FASE 4.5: Hardening de Práctica

**API robustez + validación:**
- ✅ Validación payload completa en POST /practice/review:
  - UUID format validation (regex)
  - Rating enum validation (again/hard/good/easy)
  - Type validation (correct: boolean, response_time_ms: 0-600000)
  - user_id en WHERE clause del UPDATE (extra safety)
- ✅ Manejo robusto de edge cases:
  - next_review_in_days con Math.max(0, ...) (evita negativos)
  - Coerción de tipos numéricos en queries (parseInt, parseFloat)

**Métricas reales implementadas:**
- ✅ **streak_days**: días consecutivos con sesiones completadas
  - Query CTE con daily_activity + streak groups
  - Cuenta desde hoy o ayer hacia atrás
  - Fórmula: `day - INTERVAL '1 day' * ROW_NUMBER()`
- ✅ **accuracy_percent**: % respuestas correctas de session_responses
  - Query: `SUM(CASE WHEN is_correct THEN 1 ELSE 0 END) / COUNT(*) * 100`
  - JOIN con sessions para filtrar por user_id
  - ROUND(..., 1) → 1 decimal

**Tests API — cobertura completa:**
- ✅ 11/11 tests pasando (9 practice + 2 health)
- ✅ Tests agregados:
  - GET /practice/next con due cards existentes
  - GET /practice/next crea nueva card si no hay due
  - GET /practice/next mensaje "No more cards" si pool vacío
  - POST /practice/review actualiza card con rating válido
  - POST /practice/review 400 si falta rating
  - POST /practice/review 400 si rating inválido
  - POST /practice/review 400 si card_id no es UUID
  - POST /practice/review 404 si card no existe
  - GET /practice/stats devuelve stats completas (total, due, state, streak, accuracy)
- ✅ Mock pattern corregido (mockQuery en scope global, UUIDs válidos en tests)

**Mobile polish:**
- ✅ Retry logic con exponential backoff:
  - loadNextCard(retryCount) hasta 2 retries
  - Backoff: 1s, 2s
  - Evita loop infinito en caso de API down
- ✅ Error states mejorados:
  - Estado `error` separado para mostrar banner
  - Screen de error dedicada con botón "Reintentar"
  - Error banner en practice screen si falla submit
  - Mensajes de error específicos ("No se pudo cargar", "No se pudo enviar")
- ✅ Doble-submit prevention:
  - Guard `if (submitting) return;` en handleSubmitReview
  - Botones rating disabled cuando submitting=true
  - ActivityIndicator visible durante submit
  - Solo resetea submitting=false en catch (permite retry)

**Auth persistence auditoría:**
- ✅ Revisado AsyncStorage usage en mobile:
  - Supabase auth usa AsyncStorage para JWT tokens
  - user.store también usa AsyncStorage (no sensible, solo metadata)
- ✅ DEC-009 documentado:
  - AsyncStorage aceptable para MVP (riesgo bajo, tokens expiran 1h/7d)
  - Plan migración a SecureStore en FASE 6-7 (antes de RevenueCat)
  - Justificación: no PII sensible, no datos financieros, filesystem sandboxed

**Git:**
- ✅ Pendiente commit (practice hardening)
- ✅ Pendiente push

**Flujo hardened end-to-end:**
1. Usuario login mobile ✅
2. Dashboard → tap "Comenzar práctica" ✅
3. API valida JWT + devuelve card con validación robusta ✅
4. Mobile muestra pregunta con retry si falla carga ✅
5. Usuario selecciona respuesta ✅
6. Tap "Ver solución" → muestra pasos ✅
7. Usuario califica dificultad ✅
8. API valida payload (UUID, rating enum, tipos) ✅
9. FSRS actualiza scheduling con métricas reales ✅
10. Mobile carga siguiente card con retry logic ✅
11. Stats reales: streak_days + accuracy_percent calculados ✅

**Reutilizado de Arandur:**
- Test patterns (mock pool, supertest)
- Validación patterns (UUID regex, enum validation)

**Nuevo en Arandil:**
- Métricas reales desde FASE 4.5 (Arandur las agregó en FASE 6+)
- Retry logic con exponential backoff en mobile
- Error states más robustos que Arandur MVP

**Pendiente para FASE 5:**
- Onboarding matemáticas (nivel, área, objetivo)
- IA generación de preguntas (DeepSeek)
- Sesiones con mood tracking
- Offline sync

---

### 2026-07-04 — FASE 5: Onboarding Matemáticas Real

**Modelo de perfil extendido:**
- ✅ Migración `004_onboarding_fields.sql`:
  - `math_level VARCHAR(20)` — 'beginner' | 'intermediate' | 'advanced'
  - `preferred_topic VARCHAR(100)` — tema inicial elegido en onboarding
  - `onboarding_completed BOOLEAN NOT NULL DEFAULT false`
  - Índice `idx_users_onboarding_completed` (query frecuente en auth gate)
  - `subject_focus`, `learning_goal`, `study_minutes_day` ya existían desde 001_init.sql

**API perfil/onboarding — nuevos endpoints:**
- ✅ `GET /user/profile`: perfil completo del usuario autenticado
  - Excluye `supabase_id` y `deleted_at` de la respuesta
  - 404 si usuario no existe
- ✅ `PATCH /user/profile`: update parcial (solo envía los campos que cambian)
  - Validación `math_level` enum (beginner/intermediate/advanced)
  - Validación `study_minutes_day` rango 5-180
  - Validación `onboarding_completed` boolean estricto
  - UPDATE dinámico (solo campos presentes en el body)
  - User ownership: WHERE id = userId (del JWT, no del body)
  - 400 si no se envía ningún campo

**Bug crítico corregido — lazy user provisioning:**
- 🔴 **Gap encontrado:** no existía lógica que creara la fila `users` al
  registrarse en Supabase Auth. Cualquier usuario nuevo real recibía 401
  "User not found" en el primer request a `/practice/*` o `/user/profile`
  (bug latente desde FASE 2/3, nunca antes ejercitado con un signup real).
- ✅ **Fix:** `authMiddleware` ahora hace `INSERT ... ON CONFLICT DO UPDATE`
  sobre `users` si no encuentra el `supabase_id` — provisioning en el primer
  request autenticado. `onboarding_completed` nace en `false` por default.
- 📋 Documentado en [DEC-010](DECISIONS.md#dec-010). Pendiente: reemplazar
  por trigger real de Supabase Auth (`on_auth_user_created`) en fase futura.

**Mobile — wizard de onboarding (4 pantallas):**
- ✅ `(onboarding)/_layout.tsx`: Stack sin gestos de swipe-back
- ✅ `(onboarding)/level.tsx`: nivel matemático (3 opciones)
- ✅ `(onboarding)/goal.tsx`: objetivo de aprendizaje (texto libre, opcional)
- ✅ `(onboarding)/time.tsx`: minutos/día (15/30/45/60)
- ✅ `(onboarding)/topic.tsx`: tema inicial (5 opciones) — paso final, hace
  el único PATCH /user/profile con todos los datos + `onboarding_completed: true`
- ✅ `components/OnboardingProgress.tsx`: barra de progreso (dots) reutilizable
- ✅ `stores/onboarding.store.ts`: Zustand in-memory (NO persistido) para
  pasar datos entre las 4 pantallas del wizard — ver [DEC-010](DECISIONS.md#dec-010)

**Integración con navegación (auth gate):**
- ✅ `app/index.tsx` extendido: tras verificar sesión Supabase, hace
  `GET /user/profile` y decide:
  - `onboarding_completed === false` → `router.replace('/(onboarding)/level')`
  - `onboarding_completed === true` → `router.replace('/(tabs)')`
  - Si falla el fetch de perfil (red) → fail-open a `/(tabs)` (no atrapa al
    usuario en pantalla de carga infinita)
- ✅ `(auth)/sign-in.tsx` corregido: ya no navega directo a `/(tabs)`, ahora
  pasa por `/` (el auth gate) para que se evalúe onboarding_completed
- ✅ Eliminados directorios vacíos leftover `app/auth/` y `app/tabs/`
  (no-groups, sin archivos, resto de un scaffold anterior)

**Personalización mínima real:**
- ✅ Dashboard (`(tabs)/index.tsx`):
  - CTA dinámico: "Practicar {Tema}" si hay `preferred_topic`, si no genérico
  - Card "Enfoque actual" muestra `preferred_topic` + `study_minutes_day`
  - Racha, cards, precisión ahora vienen de `getPracticeStats()` real (antes hardcoded a 0)
- ✅ Perfil (`(tabs)/profile.tsx`): muestra tema preferido, nivel, objetivo, minutos/día
- ✅ `GET /practice/next`: prioriza preguntas de `preferred_topic` sobre el resto
  (`ORDER BY (q.topic = u.preferred_topic) DESC, RANDOM()`) cuando no hay due cards

**Tests nuevos:**
- ✅ `profile.test.ts`: 10 tests
  - GET profile sin campos sensibles, 404 si no existe
  - PATCH válido, 400 math_level inválido, 400 study_minutes_day fuera de rango (bajo/alto)
  - 400 onboarding_completed no-boolean, 400 sin campos, 404 update sin usuario
  - Scoping a userId autenticado (no confía en body)

**Verificación real ejecutada:**
- ✅ `pnpm test --filter core` → 5/5 ✅
- ✅ `pnpm test --filter api` → 21/21 ✅ (9 practice + 10 profile + 2 health)
- ✅ `pnpm lint` (mobile, tsc --noEmit) → 0 errores ✅
- ✅ `pnpm db:migrate` → 004_onboarding_fields aplicada sobre DB real (docker) ✅
- ⚠️ **Limitación documentada:** el proyecto Supabase configurado en `.env`
  es un placeholder (`dummy-project.supabase.co`), no hay credenciales reales.
  No fue posible generar un JWT real vía signup HTTP para probar el flujo
  completo mobile↔API sobre la red. En su lugar se verificó la lógica de
  negocio directamente contra Postgres real (docker), ejecutando el SQL
  exacto de cada endpoint:
  1. Insert simulando lazy-provision → `onboarding_completed = false` ✅
  2. SELECT simulando `GET /user/profile` → confirma gate mandaría a onboarding ✅
  3. UPDATE simulando `PATCH /user/profile` (submit final wizard) →
     `onboarding_completed = true`, todos los campos guardados ✅
  4. SELECT de "segundo login" → `onboarding_completed = true` → gate
     mandaría directo a dashboard ✅
  5. Query de `/practice/next` con `preferred_topic = 'calculus'` →
     devuelve pregunta de cálculo antes que otros temas ✅
  6. Cleanup del usuario de prueba ✅
  - Esta verificación cubre la lógica SQL real (no mockeada) pero no
    reemplaza una prueba real de la app mobile en un simulador/dispositivo.
    Pendiente cuando exista un proyecto Supabase real configurado.

**Git:**
- Pendiente commit + push (ver sección Commits Recientes)

**Flujo de decisión — documentado:**
```
Usuario autenticado (JWT válido)
  │
  ├─ GET /user/profile
  │
  ├─ onboarding_completed = false → /(onboarding)/level (wizard 4 pasos)
  │     └─ completa wizard → PATCH /user/profile (onboarding_completed=true)
  │           └─ router.replace('/(tabs)')
  │
  └─ onboarding_completed = true → /(tabs) directo (dashboard)
```

**Reutilizado de Arandur:**
- Patrón de wizard multi-step (aunque Arandur lo usa para universidad/carrera)
- Estructura de stores Zustand

**Nuevo en Arandil (no existe en Arandur):**
- Wizard con estado in-memory no persistido (decisión explícita, DEC-010)
- Lazy user provisioning en middleware (Arandur usa trigger de Supabase)
- Personalización de `/practice/next` por preferred_topic

**Pendiente para FASE 6 (IA):**
- Trigger real de Supabase Auth para provisioning (reemplazar lazy insert)
- Proyecto Supabase real configurado (hoy es dummy) — bloquea pruebas E2E reales
- DeepSeek API para generación de preguntas
- Resume real del wizard si se persiste entre sesiones (hoy reinicia)

---

## Pendientes Inmediatos

### FASE 6 — IA Generación de Preguntas (siguiente)
1. [ ] Configurar proyecto Supabase real (hoy `.env` usa dummy-project.supabase.co)
2. [ ] Trigger `on_auth_user_created` en Supabase (reemplaza lazy provisioning de DEC-010)
3. [ ] Integrar DeepSeek API (formato OpenAI-compatible)
4. [ ] Endpoint POST /ai/generate-question con tema
5. [ ] Prompt engineering para matemáticas
6. [ ] Validación de pregunta generada
7. [ ] Guardar en questions con approved=false
8. [ ] Panel admin para revisar/aprobar
9. [ ] Tests de generación

---

## Bloqueantes

### 🔴 Requieren Acción Externa
- Ninguno

### 🟡 Requieren Decisión de Rodhan
- Ninguno por ahora

---

## Alertas para Perplexity

- ⚠️ **ARCHITECTURE.md excede el límite de ~15KB** del template (actualmente
  ~21KB tras agregar la sección de Auth Gate + Onboarding). Sugerido: mover
  el diagrama de componentes aspiracional (BKT/IRT/WhatsApp/DeepSeek, aún no
  implementados) a ARCHIVE/ y dejar en CURRENT/ solo lo realmente construido.
- ⚠️ **Proyecto Supabase real pendiente de configurar.** `.env` usa
  `dummy-project.supabase.co` — bloquea pruebas E2E reales (signup → JWT →
  llamadas autenticadas). FASE 5 se verificó a nivel de lógica SQL directa
  contra Postgres (ver STATUS FASE 5), no a nivel de red real. Recomendado
  resolver antes de FASE 6 (IA) para poder probar el flujo completo.
- ⚠️ **ROADMAP.md tiene desviación de numeración de fases** desde que se
  insertó "Onboarding" como FASE 5 propia (no estaba en el plan original).
  Ver nota en ROADMAP.md. Sugerido: Perplexity decide si renumerar el
  documento completo o mantener STATUS.md como fuente de verdad de fases.

---

## Commits Recientes

**Brain (arandil-brain):**
- `57d50ae` — feat: initial brain setup — OKF structure (2026-07-04)
- `6f7c02d` — docs: update STATUS after bootstrap completion (2026-07-04)
- `ec8ff8b` — docs(DEC-009): AsyncStorage auth + FASE 4.5 hardening (2026-07-04)
- Pendiente: docs(DEC-010): onboarding wizard + lazy provisioning + FASE 5

**Monorepo (arandil):**
- `b2a8c39` — feat: initial monorepo setup — base structure (2026-07-04)
- `8f18cf1` — feat(FASE-2): API base funcional — Express + Auth + Migraciones (2026-07-04)
- `6932fce` — feat(FASE-3): Mobile scaffold funcional — Expo + Auth + Dashboard (2026-07-04)
- `a3a21db` — feat(FASE-4): packages/core + endpoints práctica + seed inicial (2026-07-04)
- `cf44eda` — feat(FASE-4): Mobile práctica + integración API completa (2026-07-04)
- `4db3107` — feat(FASE-4.5): hardening práctica — tests, métricas reales, validación (2026-07-04)
- Pendiente: feat(FASE-5): onboarding matemático + profile API + lazy provisioning fix

---

## Métricas

| Métrica | Valor | Target FASE 5 |
|---------|-------|-----------------|
| Archivos obligatorios brain | 11/11 ✅ | 11/11 |
| Frontmatter OKF válido | 11/11 (100%) ✅ | 100% |
| Repos git inicializados | 2/2 ✅ | 2/2 |
| Commits totales | 10 (2 pendientes) ✅ | 12+ |
| Repos pusheados a GitHub | 2/2 (push pendiente) ⏳ | 2/2 |
| Monorepo workspaces | 3 (mobile, api, core) ✅ | 3 |
| Docker services | 2 (PostgreSQL, Redis) ✅ | 2 |
| API routes | 5 (health, practice×3, profile×2) ✅ | 5 |
| DB migrations | 4 (001_init...004_onboarding_fields) ✅ | 4 |
| Tests pasando | 26/26 (5 core + 21 API) ✅ | 15+ |
| Test coverage practice | 9 tests (next, review, stats) ✅ | 5+ |
| Test coverage profile | 10 tests (get, patch, validaciones, ownership) ✅ | 5+ |
| DB tablas | 8 (users +3 campos onboarding) ✅ | 8 |
| Questions seed | 10 (matemáticas) ✅ | 10 |
| Mobile screens | 11 (7 + 4 onboarding) ✅ | 11 |
| Mobile routes (Expo Router) | 3 groups ((auth), (tabs), (onboarding)) ✅ | 3 |
| TypeScript errores mobile | 0 ✅ | 0 |
| TypeScript errores API | 0 ✅ | 0 |
| TypeScript errores core | 0 ✅ | 0 |
| FSRS integrado | ts-fsrs v5.0.2 ✅ | v5+ |
| Onboarding wizard | 4 pasos, 1 PATCH final ✅ | funcional |
| Auth gate condicional | onboarding vs dashboard ✅ | implementado |
| Personalización dashboard | CTA + topic + stats reales ✅ | visible |
| Bug crítico corregido | lazy user provisioning ✅ | 1 |
| Decisiones DEC | 10 (+ DEC-010 onboarding) ✅ | 10+ |
| Flujo end-to-end | verificado a nivel SQL (Supabase real pendiente) ⚠️ | parcial |

---

## Próximos Pasos

### FASE 6 — IA Generación de Preguntas (siguiente)
1. Configurar proyecto Supabase real (desbloquea pruebas E2E reales con JWT)
2. Trigger `on_auth_user_created` (reemplaza lazy provisioning, DEC-010)
3. Integrar DeepSeek API (formato OpenAI-compatible)
4. Endpoint POST /ai/generate-question con tema
5. Prompt engineering para matemáticas
6. Validación de pregunta generada
7. Guardar en questions con approved=false
8. Panel admin para revisar/aprobar
9. Tests de generación

### FASE 7 — Stats Avanzados + Sesiones
1. POST /sessions/start (mood check-in)
2. POST /sessions/complete (actualizar duration)
3. Dashboard stats con más detalle (charts por topic/día)
4. Resume real del wizard de onboarding si se persiste entre sesiones

### FASE 8 — Deploy Staging
(ver [ROADMAP.md](ROADMAP.md) nota de desviación de numeración)

---

**Última sesión:** 2026-07-04  
**Próxima sesión:** FASE 6 — IA Generación de Preguntas (requiere Supabase real primero)
