---
type: reference
title: "Arandil — Visión de Producto"
description: "Plataforma global de aprendizaje de matemáticas con IA, enfocada en mastery learning y falla rápido"
tags: [vision, producto, matematicas, ia, mastery-learning]
timestamp: 2026-07-04
---

# Arandil — Visión de Producto

## ¿Qué es Arandil?

Arandil es una plataforma educativa global para aprendizaje de matemáticas, potenciada por IA, que permite a estudiantes de cualquier nivel dominar conceptos mediante **mastery learning** y repetición espaciada.

A diferencia de apps de drill pasivo, Arandil adapta la dificultad en tiempo real, refuerza conceptos débiles automáticamente, y usa IA para generar problemas únicos y explicaciones personalizadas.

**Filosofía core:** Falla rápido, aprende, reintenta, repite hasta dominar.

---

## El problema que resuelve

### Para estudiantes
- **Frustración por lagunas de conocimiento:** olvidaron un tema básico hace 3 años y ahora no pueden avanzar
- **Drill sin contexto:** apps tradicionales repiten problemas sin diagnosticar la causa raíz del error
- **Feedback genérico:** "incorrecto" sin explicación ni camino de mejora
- **Sin adaptación:** todos reciben los mismos problemas sin importar su nivel real

### Para apoderados (padres, tutores)
- **Falta de visibilidad:** no saben si su hijo/a está realmente progresando o solo "cumpliendo" minutos
- **Dificultad para apoyar:** no saben qué temas reforzar ni cómo ayudar
- **Comunicación asíncrona:** requieren reportes que lleguen a WhatsApp sin tener que abrir otra app

---

## Para quién

### Usuario primario
- **Estudiantes de secundaria y universidad** (14-25 años)
- **Adultos retomando matemáticas** (profesionales, cambio de carrera, certificaciones)
- Acceso a smartphone (Android mayoritario en v1)
- Motivación intrínseca o extrínseca (examen, curso, carrera)

### Usuario secundario
- **Apoderados** (padres, tutores) que reciben reportes por WhatsApp
- Pueden dar contexto ("enfermo hoy", "viaje familiar") para pausar algoritmo de racha

### Mercado inicial
- **LATAM:** Perú, México, Colombia, Chile, Argentina
- Expandir a: Brasil (PT), España, EEUU (latinos)

---

## Arquitectura de producto

### Flujo principal
1. **Onboarding:** estudiante selecciona nivel (secundaria/universitario) y área de enfoque inicial
2. **Diagnóstico IRT:** 15 preguntas adaptativas para mapear lagunas de conocimiento
3. **Sesiones diarias:** 20-30 min de práctica con MCQ generadas por IA, calibradas a su nivel
4. **Feedback inmediato:** explicación paso a paso con modo socrático opcional (IA conversacional)
5. **Repetición espaciada (FSRS-7):** algoritmo trae de vuelta temas olvidados justo antes de que se pierdan
6. **Reportes a apoderado:** resumen diario por WhatsApp (racha, temas del día, tiempo de estudio)

### Componentes clave
- **IA generativa (DeepSeek):** crea problemas únicos basados en currículo estándar de matemáticas
- **FSRS-7:** repetición espaciada científicamente validada, mismo algoritmo que Anki
- **BKT (Bayesian Knowledge Tracing):** modela probabilidad de dominio por subtema
- **IRT (Item Response Theory):** diagnóstico inicial calibrado
- **Offline-first:** sesiones funcionan sin conexión, sync al reconectar

### Diferenciadores vs competencia
| Dimensión | Arandil | Khan Academy | Photomath | Duolingo Math |
|-----------|---------|--------------|-----------|---------------|
| **Generación dinámica de problemas** | ✅ IA crea problemas únicos | ❌ Banco fijo | ⚠️ Solo camera scan | ⚠️ Banco fijo |
| **Repetición espaciada FSRS** | ✅ Científicamente validado | ❌ No usa SRS | ❌ No usa SRS | ⚠️ SRS básico |
| **Diagnóstico IRT** | ✅ 15 preguntas adaptativas | ❌ Manual | ❌ No diagnóstico | ❌ Linear |
| **Offline funcional** | ✅ Sesiones completas offline | ❌ Requiere conexión | ⚠️ Solo scan offline | ❌ Requiere conexión |
| **Reportes apoderado WhatsApp** | ✅ Automático | ❌ Solo email | ❌ No existe | ❌ Solo email |
| **Modo socrático IA** | ✅ DeepSeek conversacional | ⚠️ Videos pre-grabados | ⚠️ Explicación fija | ❌ No existe |
| **Pricing** | S/29/mes (~USD 8) | Gratis + ads | Gratis + ads | Gratis beta |

---

## Principios Claude Code

Estos principios guían cómo trabajar en el proyecto con Claude Code:

1. **Lee antes de escribir.** Nunca sobrescribir archivo sin leerlo primero.
2. **Edita, no reescribas.** Prefiere cambios quirúrgicos sobre rewrites completos.
3. **Sin ORM.** SQL puro con `pg`. Migraciones numeradas.
4. **Sin magia.** Nada de decoradores, auto-wiring ni frameworks que oculten el flujo.
5. **TypeScript estricto.** `strict: true` en todos los `tsconfig.json`. Sin `any`.
6. **Zod en los bordes.** Validar toda entrada externa antes de usarla.
7. **Errores explícitos.** Nunca silenciar errores con `catch() {}` vacío.
8. **`packages/core` es puro.** Cero imports de React, Express, pg. Solo lógica y tipos.
9. **Variables de entorno.** Documentar en `.env.example`. Nunca hardcodear valores.
10. **Piensa antes de actuar.** Si hay ambigüedad, razona primero.

---

## Estado actual

**Fase:** Bootstrap inicial — creación de estructura base  
**Última actualización:** 2026-07-04  
**Próximo hito:** MVP técnico funcional (API + mobile scaffold)

---

## Hitos clave

| Hito | Estado | Fecha objetivo |
|------|--------|----------------|
| Bootstrap proyecto (brain + monorepo) | 🟡 En progreso | 2026-07-04 |
| API base + auth + migraciones | ⚪ Pendiente | 2026-07-05 |
| Mobile scaffold + onboarding matemáticas | ⚪ Pendiente | 2026-07-06 |
| IA integration (DeepSeek + prompts matemáticas) | ⚪ Pendiente | 2026-07-07 |
| MVP funcional local | ⚪ Pendiente | 2026-07-08 |
| Deploy staging | ⚪ Pendiente | 2026-07-15 |
| Beta cerrada (50 usuarios) | ⚪ Pendiente | 2026-08-01 |
| Lanzamiento público v1.0 | ⚪ Pendiente | 2026-09-01 |

---

## Expansión futura

### v1.1 — Lectura veloz (bionic reading)
- Resaltado de letra media en textos largos
- Modo RSVP (Rapid Serial Visual Presentation) palabra por palabra
- Útil para problemas de física/química con enunciados largos

### v1.5 — Otras materias
- Física
- Química
- Comunicación (comprensión lectora, redacción)
- Historia
- Ciencias sociales

### v2.0 — Modo colaborativo
- Sesiones en vivo con amigos
- Ranking semanal por temas
- Desafíos grupales

---

**Documento vivo.** Actualizar cuando cambie visión de producto o estrategia core.
