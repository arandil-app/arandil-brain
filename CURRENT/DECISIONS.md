---
type: reference
title: "Arandil — Decisiones Técnicas"
description: "Registro de decisiones de arquitectura (ADR) con contexto, alternativas y consecuencias"
tags: [decisions, adr, arquitectura, stack]
timestamp: 2026-07-04
---

# Arandil — Decisiones Técnicas (DEC)

> **Formato obligatorio por decisión:**
> - Fecha (YYYY-MM-DD)
> - Contexto (por qué se tomó la decisión)
> - Decisión (qué se decidió)
> - Alternativas descartadas (qué más se consideró)
> - Consecuencias (trade-offs, implicaciones)

**Convención de numeración:** DEC-001, DEC-002, ... (secuencial)  
**Convención de anclas:** `<a id="dec-001"></a>` antes de cada heading  
**Referencias cruzadas:** `[DEC-XXX](DECISIONS.md#dec-xxx)`

---

<a id="dec-001"></a>
## DEC-001 — Arandil es proyecto independiente, no fork de Arandur

**Fecha:** 2026-07-04  
**Contexto:**  
Arandur es una app exitosa de preparación para exámenes de admisión peruanos (UNI, UNMSM, Continental, UPLA). Su stack técnico está validado en producción con usuarios reales. Sin embargo, el dominio de negocio es muy específico: exámenes de admisión en Perú.

Arandil tiene un objetivo diferente: ser una plataforma global de aprendizaje de matemáticas, no solo drill para un examen específico. Esto requiere una arquitectura conceptual distinta, aunque la base técnica pueda reutilizarse.

**Decisión:**  
Arandil es un proyecto NUEVO e INDEPENDIENTE, no una rama ni fork de Arandur. Se crea en un workspace separado (`/home/rodri/arandil-workspace/`) con sus propios repositorios en GitHub bajo la org `arandil-app`.

**Alternativas descartadas:**
1. **Fork de Arandur:** Heredaría todo el historial de commits, migraciones y lógica de negocio específica de admisión. Difícil de limpiar.
2. **Rama de Arandur:** Mismo problema de acoplamiento, además de conflictos constantes en merge con main.
3. **Monorepo compartido:** Complejidad innecesaria para dos productos con objetivos distintos.

**Consecuencias:**
- ✅ Libertad arquitectónica total para Arandil
- ✅ Sin migraciones heredadas de Arandur
- ✅ Sin referencias a universidades peruanas en código
- ⚠️ Requiere copiar/adaptar módulos técnicos manualmente
- ⚠️ Dos repos separados para mantener (pero independientes)

---

<a id="dec-002"></a>
## DEC-002 — Reutilizar stack y módulos probados de Arandur

**Fecha:** 2026-07-04  
**Contexto:**  
Arandur tiene un stack técnico validado en producción:
- pnpm workspaces + Turborepo
- Express + PostgreSQL + pg (sin ORM)
- React Native + Expo 52 + Expo Router
- Supabase Auth
- BullMQ + Redis
- Cloudflare R2
- DeepSeek (IA)
- FSRS-7 (repetición espaciada)

Crear Arandil desde cero con un stack diferente tomaría semanas y no aportaría valor técnico real. El problema a resolver no es el stack, sino la lógica de negocio.

**Decisión:**  
Copiar y adaptar la base técnica completa de Arandur:
- Configuración de monorepo (Turborepo + pnpm)
- Estructura de API (Express + PostgreSQL + middleware)
- Estructura de mobile (React Native + Expo Router)
- Integraciones: Supabase Auth, RevenueCat, Twilio/WhatsApp, Cloudflare R2
- Algoritmos: FSRS-7, BKT, IRT (en `packages/core`)

**Alternativas descartadas:**
1. **Stack diferente (Next.js + Prisma + tRPC):** No hay justificación técnica. Solo introduce complejidad y curva de aprendizaje.
2. **Reescribir desde cero:** Pérdida de tiempo. El stack de Arandur ya funciona en producción.
3. **Usar template genérico (create-expo-app, express-generator):** No incluye integraciones críticas (auth, IA, SRS).

**Consecuencias:**
- ✅ Velocidad de desarrollo: semanas en lugar de meses
- ✅ Stack validado en producción real
- ✅ Integraciones ya configuradas (Supabase, RevenueCat, etc.)
- ⚠️ Requiere refactor de lógica de negocio (eliminar conceptos de "admisión")
- ⚠️ Mantener compatibilidad con convenciones de Arandur para facilitar futuros backports

---

<a id="dec-003"></a>
## DEC-003 — Eliminar dependencia conceptual de admisión/universidades

**Fecha:** 2026-07-04  
**Contexto:**  
Arandur tiene lógica específica de exámenes de admisión peruanos hardcoded en:
- Database schema: `exam_target`, `universidad_objetivo`, enums de universidades
- IA prompts: referencias a "prospecto UNI", "examen de admisión"
- UI copy: "postulante", "días hasta examen", "universidad objetivo"
- Curriculum: archivos `.md` organizados por universidad

Arandil necesita ser agnóstico de contexto geográfico y tipo de examen.

**Decisión:**  
Eliminar completamente referencias a:
- Universidades peruanas (UNI, UNMSM, Continental, UPLA)
- Concepto de "examen de admisión" como objetivo único
- Enums hardcoded de universidades en DB
- Copy específico de "postulante", "ingreso a universidad"

Reemplazar con conceptos generales:
- `subject_focus` en lugar de `exam_target`
- `learning_goal` en lugar de `universidad_objetivo`
- Curriculum organizado por materia (álgebra, geometría...) no por universidad

**Alternativas descartadas:**
1. **Mantener enums y agregar "global" como opción:** Técnicamente factible pero conceptualmente confuso. El schema seguiría asumiendo que el objetivo es un examen específico.
2. **Feature flag para alternar entre modo admisión/global:** Complejidad innecesaria. Arandil no necesita modo admisión.

**Consecuencias:**
- ✅ Producto verdaderamente global desde día 1
- ✅ Sin referencias geográficas en código
- ✅ Expansión futura a otros países sin cambios de schema
- ⚠️ Requiere refactor completo de DB migrations
- ⚠️ Requiere refactor completo de IA prompts
- ⚠️ Requiere audit de copy en mobile UI

---

<a id="dec-004"></a>
## DEC-004 — MVP enfocado primero en matemáticas

**Fecha:** 2026-07-04  
**Contexto:**  
Arandil tiene como visión ser una plataforma multi-materia (matemáticas, física, química, comunicación, historia...). Sin embargo, crear contenido de calidad para todas las materias simultáneamente es inviable en fase MVP.

Matemáticas tiene ventajas únicas:
- Evaluación objetiva (1 respuesta correcta, no ensayos)
- Generación IA más confiable (problemas estructurados)
- Currículo estandarizado globalmente (álgebra, geometría, cálculo)
- Demanda universal (todos los niveles educativos)

**Decisión:**  
MVP de Arandil se enfoca SOLO en matemáticas:
- Curriculum inicial: álgebra, geometría, trigonometría, cálculo
- IA entrenada específicamente en generación de problemas matemáticos
- Onboarding pregunta nivel (secundaria/universitario) pero no materia
- Expansión a otras materias en v1.5+

**Alternativas descartadas:**
1. **Multi-materia desde v1:** Requiere 5x el esfuerzo de contenido y prompts IA. Dificulta iteración rápida.
2. **Matemáticas + física en v1:** Física requiere diagramas paramétricos complejos (fuerzas, vectores). Mejor validar el core con matemáticas primero.
3. **Solo álgebra básica:** Demasiado limitado. Usuarios universitarios no encontrarían valor.

**Consecuencias:**
- ✅ Iteración rápida en generación de problemas
- ✅ Prompts IA enfocados (mejor calidad)
- ✅ Curriculum manejable para validar
- ⚠️ Usuarios buscando otras materias deben esperar v1.5
- ⚠️ Arquitectura debe permitir expansión futura (no hardcodear "mathematics")

---

<a id="dec-005"></a>
## DEC-005 — Organización del proyecto sigue PROJECT-TEMPLATE.md

**Fecha:** 2026-07-04  
**Contexto:**  
`/home/rodri/setup-workspaces/PROJECT-TEMPLATE.md` define un estándar de organización para todos los proyectos del ecosistema. Incluye:
- Estructura de carpetas obligatoria (`{proyecto}-brain/CURRENT/`, `ARCHIVE/`)
- Frontmatter OKF en todos los archivos MD
- Archivos obligatorios (STATUS.md, VISION.md, DECISIONS.md, ARCHITECTURE.md, etc.)
- Convenciones de commits, numeración DEC, límites de tamaño

Arandur fue creado antes de que existiera el template, por lo que tiene algunas inconsistencias. Arandil es una oportunidad para aplicar el template desde el inicio.

**Decisión:**  
Arandil sigue estrictamente `PROJECT-TEMPLATE.md`:
- Brain en `/home/rodri/arandil-workspace/arandil-brain/`
- Todos los archivos MD con frontmatter OKF completo
- Archivos obligatorios creados desde día 1
- GROWTH.md y DESIGN.md marcados como "Perplexity + Rodhan only"
- TROUBLESHOOTING.md creado vacío (se puebla con errores recurrentes)
- Commits con formato `feat(DEC-XXX):` cuando aplica

**Alternativas descartadas:**
1. **Copiar estructura de Arandur tal cual:** No cumple template (falta frontmatter OKF, archivos dispersos).
2. **Crear estructura ad-hoc:** Inconsistencia con otros proyectos del ecosistema.

**Consecuencias:**
- ✅ Consistencia con resto del ecosistema
- ✅ Onboarding de nuevos colaboradores más fácil (sigue template conocido)
- ✅ Auditoría automática con checklist de template
- ⚠️ Requiere disciplina para mantener frontmatter actualizado
- ⚠️ Archivos como GROWTH.md y DESIGN.md quedan vacíos hasta que Perplexity los llene

---

<a id="dec-006"></a>
## DEC-006 — DeepSeek como motor IA principal

**Fecha:** 2026-07-04  
**Contexto:**  
Generación de problemas matemáticos y feedback socrático son core de Arandil. Opciones de IA:
- **OpenAI GPT-4:** Excelente calidad, muy caro ($10-30 por 1M tokens input)
- **Anthropic Claude:** Alta calidad, caro ($3-15 por 1M tokens)
- **DeepSeek V3:** Calidad competitiva, formato OpenAI-compatible, mucho más barato ($0.14-0.28 por 1M tokens)

Arandur usa DeepSeek en producción desde Diciembre 2024 sin issues de calidad. La API es estable.

**Decisión:**  
DeepSeek V3 como motor principal para:
- Generación de problemas MCQ
- Feedback socrático (modo tutor)
- Explicaciones paso a paso

Claude como fallback premium (opcional en v1.1) para:
- Explicaciones complejas de cálculo avanzado
- Diagramas paramétricos con descripción natural

**Alternativas descartadas:**
1. **Solo OpenAI GPT-4:** Costo prohibitivo para escalar. 100K usuarios activos = miles de dólares/mes.
2. **Solo Claude:** Mismo problema de costo.
3. **Modelo local (Llama 3, Mistral):** Requiere infraestructura GPU cara, latencia alta, mantenimiento complejo.

**Consecuencias:**
- ✅ Costo operativo bajo (viable para pricing S/29/mes)
- ✅ Formato OpenAI-compatible (fácil cambiar provider si es necesario)
- ✅ Calidad validada en Arandur con usuarios reales
- ⚠️ Dependencia de proveedor único (mitigar con retry logic + fallbacks)
- ⚠️ Si DeepSeek sube precios drásticamente, requiere migración

---

<a id="dec-007"></a>
## DEC-007 — FSRS-7 para repetición espaciada

**Fecha:** 2026-07-04  
**Contexto:**  
Repetición espaciada (SRS) es crítica para mastery learning. Opciones:
- **SM-2 (SuperMemo 2):** Algoritmo clásico, simple, pero no considera dificultad variable del estudiante
- **FSRS-5/7:** Algoritmo moderno basado en research científico, open source, usado por Anki
- **Custom heuristic:** Crear algoritmo propio

Arandur usa `ts-fsrs` (FSRS-7 en TypeScript) en producción. Es puro (sin deps de backend) y corre tanto en API como en mobile offline.

**Decisión:**  
Usar `ts-fsrs` (FSRS-7) en `packages/core/fsrs/`:
- Mismo wrapper que Arandur
- Algoritmo científicamente validado (research papers publicados)
- Corre offline en mobile (pre-calcular next review dates)
- Compatible con Anki (usuarios pueden exportar sus datos)

**Alternativas descartadas:**
1. **SM-2:** Más simple pero menos preciso. FSRS-7 supera SM-2 en todos los benchmarks.
2. **Crear algoritmo propio:** No hay razón para reinventar la rueda. FSRS es open source y peer-reviewed.
3. **No usar SRS:** Sin SRS, no hay mastery learning real. El olvido es inevitable sin refuerzo programado.

**Consecuencias:**
- ✅ Algoritmo validado científicamente
- ✅ Compatible con Anki (facilita migración de usuarios)
- ✅ Corre offline (mobile puede pre-calcular sesiones)
- ⚠️ Requiere almacenar parámetros FSRS por card (difficulty, stability, etc.)
- ⚠️ Migrar a otro algoritmo SRS requeriría re-entrenar parámetros

---

<a id="dec-008"></a>
## DEC-008 — pnpm workspaces + Turborepo

**Fecha:** 2026-07-04  
**Contexto:**  
Arandil es un monorepo con:
- `apps/mobile` (React Native)
- `services/api` (Express)
- `packages/core` (algoritmos compartidos)

Opciones de gestión de monorepo:
- **npm workspaces:** Built-in en npm, simple, pero no tiene caché de builds
- **pnpm workspaces:** Más rápido que npm, caché de dependencias, compatible con Turborepo
- **yarn workspaces:** Similar a pnpm pero más lento
- **Turborepo:** Orquestador de builds con caché incremental

Arandur usa pnpm + Turborepo y funciona sin problemas.

**Decisión:**  
Monorepo gestionado con:
- **pnpm workspaces** para gestión de dependencias
- **Turborepo** para builds paralelos y caché incremental
- Configuración heredada de Arandur (probada en producción)

**Alternativas descartadas:**
1. **npm workspaces:** Más lento que pnpm, sin caché incremental.
2. **yarn workspaces:** No aporta ventajas sobre pnpm.
3. **Lerna:** Más complejo y menos activo que Turborepo.
4. **Nx:** Muy potente pero overkill para 3 packages. Curva de aprendizaje alta.

**Consecuencias:**
- ✅ Builds en paralelo (API + mobile al mismo tiempo)
- ✅ Caché incremental (no rebuilds innecesarios)
- ✅ Comandos desde raíz (`pnpm dev`, `pnpm test`)
- ⚠️ **NUNCA mezclar npm/yarn/pnpm** — solo pnpm en todo el monorepo
- ⚠️ Lockfile (`pnpm-lock.yaml`) debe commitearse siempre

---

<a id="dec-009"></a>
## DEC-009 — AsyncStorage para auth persistence en MVP (migrar a SecureStore en producción)

**Fecha:** 2026-07-04  
**Contexto:**  
Supabase auth requiere almacenamiento persistente para JWT tokens (access token, refresh token). Opciones en React Native:

- **AsyncStorage (@react-native-async-storage/async-storage):** Plain text, filesystem sandboxed, sencillo
- **SecureStore (expo-secure-store):** Encriptado (iOS Keychain, Android EncryptedSharedPreferences), más seguro

Los JWT tokens son sensibles porque permiten acceso a la API en nombre del usuario. Sin embargo:
- Access token expira en 1h
- Refresh token expira en 7d (configurable)
- No hay datos financieros en Arandil MVP (solo práctica matemática)
- Filesystem sandboxed en iOS/Android modernos (requiere root/jailbreak para acceder)

**Decisión:**  
MVP (FASE 0-5) usa **AsyncStorage** para auth persistence. Es aceptable porque:
- Riesgo bajo (no hay PII sensible, no hay datos financieros)
- Tokens expiran pronto (1h access, 7d refresh)
- Filesystem sandboxed protege de acceso casual

**Plan de migración:**  
Migrar a **SecureStore** en FASE 6-7 (antes de RevenueCat + pagos):
1. Instalar `expo-secure-store`
2. Crear `SecureStorageAdapter` compatible con Supabase storage interface
3. Actualizar `apps/mobile/src/lib/supabase.ts` para usar SecureStore
4. Migrar tokens existentes de AsyncStorage a SecureStore (una sola vez)

**Alternativas descartadas:**
1. **SecureStore desde MVP:** Añade complejidad sin beneficio proporcional al riesgo actual. Mejor priorizar features (onboarding, IA generación).
2. **No persistir sesión:** UX pobre (login cada vez que abres la app). Inaceptable.

**Consecuencias:**
- ✅ MVP más rápido de desarrollar (AsyncStorage built-in en Expo)
- ✅ Riesgo aceptable para fase actual
- ⚠️ Tech debt: migración a SecureStore obligatoria antes de pagos/RevenueCat
- ⚠️ JWT en plain text en disco (vulnerable si device rooted/jailbroken)
- ⚠️ No cumple compliance estricta (HIPAA, PCI-DSS) — pero no aplica en app de práctica matemática

**Referencias:**
- Supabase storage docs: https://supabase.com/docs/reference/javascript/auth-api#storage
- Expo SecureStore: https://docs.expo.dev/versions/latest/sdk/securestore/

---

<a id="dec-010"></a>
## DEC-010 — Onboarding wizard con estado en memoria + lazy user provisioning

**Fecha:** 2026-07-04  
**Contexto:**  
FASE 5 requería un onboarding matemático real (4 pasos: nivel, objetivo, tiempo, tema) que se muestre solo en el primer login y bloquee el paso al dashboard hasta completarse. Dos problemas de diseño surgieron:

1. **Dónde vive el estado del wizard mientras el usuario navega entre las 4 pantallas** (nivel → objetivo → tiempo → tema)
2. **Qué pasa si el usuario abandona a mitad del flujo** (cierra la app en la pantalla 2 de 4)

Además, durante la implementación se descubrió un gap pre-existente de FASE 2/3: no existía ninguna lógica que creara la fila en la tabla `users` cuando un usuario se registraba en Supabase Auth. Sin esa fila, `authMiddleware` devolvía 401 "User not found" en cualquier endpoint autenticado — lo cual habría roto el onboarding gate (y de hecho ya rompía silenciosamente `/practice/*` desde FASE 4 para cualquier usuario nuevo real).

**Decisión:**

1. **Estado del wizard en Zustand in-memory** (`onboarding.store.ts`), sin persistir a AsyncStorage. Los datos solo se escriben a la API (PATCH /user/profile) una sola vez, al completar el paso final (topic.tsx), con todos los campos juntos.
   - Si el usuario abandona a mitad → `onboarding_completed` sigue en `false` en el servidor → el auth gate lo vuelve a mandar a `/(onboarding)/level` en el siguiente login → el wizard reinicia desde cero.
   - Esto es "volver al flujo de forma coherente" (sin corrupción de datos, sin estados intermedios inválidos), no "resume exacto del paso donde quedó". Se documenta como trade-off aceptado por simplicidad.

2. **Lazy user provisioning en `authMiddleware`:** si el `supabase_id` del JWT no tiene fila en `users`, se hace `INSERT ... ON CONFLICT DO UPDATE` en el primer request autenticado, con `onboarding_completed` naciendo en `false` por default de columna. Esto reemplaza (temporalmente) la necesidad de un trigger/webhook de Supabase Auth.

**Alternativas descartadas:**
1. **Persistir cada paso del wizard con PATCH incremental:** permite resume exacto, pero agrega 4 round-trips a la API en vez de 1, y complejiza el manejo de errores parciales (¿qué pasa si el PATCH del paso 2 falla pero el 3 no?). Descartado por complejidad desproporcionada al beneficio para un wizard de 4 pasos cortos.
2. **Persistir wizard state en AsyncStorage:** permite resume sin red, pero duplica la fuente de verdad (local vs servidor) y requiere lógica de reconciliación. Descartado — el servidor (`onboarding_completed`) ya es la fuente de verdad suficiente.
3. **Trigger de Supabase Auth (`on_auth_user_created`) para provisioning:** más correcto a largo plazo (provisioning inmediato, no depende del primer request autenticado), pero requiere configurar infraestructura en el dashboard de Supabase fuera de este repo. Pospuesto — lazy provisioning en middleware resuelve el bloqueo inmediato sin esa dependencia externa.

**Consecuencias:**
- ✅ Onboarding funcional con 1 sola llamada de red al completar (no 4)
- ✅ Sin estados corruptos si el usuario abandona el flujo
- ✅ Gap crítico de FASE 2/3 corregido (usuarios nuevos ya no reciben 401 en su primer request)
- ⚠️ Si el usuario abandona a mitad, pierde el progreso del wizard (no hay resume real) — aceptable para un flujo de 4 pasos cortos
- ⚠️ Lazy provisioning es un parche: el trigger de Supabase sigue pendiente para cuando se necesite provisioning inmediato (ej. antes de la primera llamada autenticada, para analytics de signup)

---

## Próximas decisiones a documentar

- DEC-011: Estructura de curriculum matemático (`packages/curriculum/mathematics/`)
- DEC-012: Prompts de sistema DeepSeek para generación de problemas
- DEC-013: Trigger Supabase Auth para user provisioning (reemplazar lazy insert de DEC-010)
- DEC-014: Reportes a apoderado por WhatsApp (comandos y frecuencia)
- DEC-015: Pricing regional (S/29 Perú, MXN 150 México, USD 8 resto)

---

**Última actualización:** 2026-07-04  
**Total decisiones:** 10  
**Próximo número disponible:** DEC-011
