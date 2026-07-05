---
type: log
title: "Arandil — Log de Sesiones"
description: "Historial cronológico de sesiones de trabajo del proyecto"
tags: [sessions, log, historial]
timestamp: 2026-07-04
---

# Arandil — Log de Sesiones

> **Propósito:** Registro histórico de sesiones de trabajo. No borrar, solo agregar al final.

**Formato:** `## Sesión XXX — YYYY-MM-DD — Título`

---

## Sesión 001 — 2026-07-04 — Bootstrap Inicial (FASE 0)

**Objetivo:** Crear estructura base del proyecto siguiendo PROJECT-TEMPLATE.md

**Completado:**
- ✅ Diagnóstico completo (qué copiar de Arandur, riesgos, plan de acción)
- ✅ Brain creado con estructura CURRENT/ARCHIVE
- ✅ 10 archivos obligatorios con frontmatter OKF:
  - VISION.md
  - DECISIONS.md (DEC-001 a DEC-008)
  - ARCHITECTURE.md
  - ROADMAP.md
  - STATUS.md
  - GROWTH.md (placeholder Perplexity-only)
  - DESIGN.md (placeholder Perplexity-only)
  - INDEX.md
  - AGENTS-PROTOCOL.md
  - TROUBLESHOOTING.md
- ✅ README.md del brain
- ✅ ARCHIVE/SESSIONS.md (este archivo)

**Pendiente:**
- [ ] CLAUDE.md en workspace root
- [ ] Git init + remote setup brain
- [ ] Primer commit + push brain
- [ ] Comenzar FASE 1 (monorepo base)

**Notas:**
- Seguimiento estricto de PROJECT-TEMPLATE.md
- Todos los archivos con frontmatter OKF válido
- GROWTH.md y DESIGN.md marcados como Perplexity-only según template

**Duración:** ~1h

---

## Sesión 002 — 2026-07-04 — FASE 1 a FASE 5 (Monorepo, API, Mobile, FSRS, Onboarding)

**Objetivo:** Llevar Arandil de bootstrap a un producto con práctica matemática
FSRS end-to-end + onboarding real, en una sola sesión extendida.

**Completado (resumen — detalle completo en STATUS.md):**
- ✅ FASE 1: Monorepo pnpm + Turborepo, Docker (Postgres + Redis)
- ✅ FASE 2: API Express base, migraciones 001-002, health check
- ✅ FASE 3: Mobile scaffold (Expo Router, Supabase Auth, dashboard, tabs)
- ✅ FASE 4: packages/core con ts-fsrs v5, endpoints /practice/*, seed 10 preguntas, pantalla práctica mobile
- ✅ FASE 4.5: hardening — tests API reales (21/21), streak_days + accuracy_percent reales, validación robusta, mobile retry/error states
- ✅ FASE 5: onboarding matemático (4 pasos), API perfil (GET/PATCH /user/profile), auth gate condicional, personalización dashboard/practice por preferred_topic

**Bug crítico encontrado y corregido en FASE 5:**
- No existía creación de fila `users` al registrarse en Supabase Auth
  (gap desde FASE 2/3). Cualquier usuario nuevo real habría recibido 401 en
  su primer request autenticado. Corregido con lazy provisioning en
  `authMiddleware` — ver [DEC-010](../CURRENT/DECISIONS.md#dec-010).

**Decisiones documentadas:** DEC-001 a DEC-010 (ver DECISIONS.md)

**Limitación conocida:**
- Proyecto Supabase configurado en `.env` es un placeholder (dummy). No fue
  posible ejercitar el flujo completo mobile↔API sobre la red real con un
  JWT genuino. Se verificó la lógica de negocio ejecutando el SQL exacto de
  cada endpoint directamente contra Postgres real (docker). Pendiente
  configurar Supabase real antes de FASE 6.

**Tests finales de la sesión:**
- Core: 5/5 ✅
- API: 21/21 ✅ (9 practice + 10 profile + 2 health)
- Mobile typecheck: 0 errores ✅

**Git:**
- Monorepo: commits b2a8c39 → 4db3107 (7 commits), pendiente 1 más (FASE 5)
- Brain: commits 57d50ae → ec8ff8b (3 commits), pendiente 1 más (FASE 5)

**Duración:** sesión extendida (múltiples fases, ~6-8h efectivas de trabajo)

**Notas:**
- ROADMAP.md quedó desalineado en numeración de fases tras insertar
  Onboarding como FASE 5 propia — documentado como nota de desviación,
  no corregido en esta sesión (requiere decisión de Perplexity/Rodhan)
- ARCHITECTURE.md superó el límite de 15KB — flag para archivar contenido
  aspiracional no implementado (BKT/IRT/WhatsApp) en sesión futura

---

**Próxima sesión:** FASE 6 — IA Generación de Preguntas (requiere resolver
Supabase real primero para pruebas E2E genuinas)
