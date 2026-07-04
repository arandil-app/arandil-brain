---
type: runbook
title: "Arandil — Protocolo Perplexity ↔ Claude Code"
description: "Orden de lectura, autonomía, checklist cierre y anti-patrones en colaboración multiagente"
tags: [protocolo, perplexity, claude-code, workflow]
timestamp: 2026-07-04
refs:
  - STATUS.md
  - DECISIONS.md
---

# Arandil — Protocolo Perplexity ↔ Claude Code

> **Propósito:** Definir flujo de trabajo claro entre Perplexity (investigación/estrategia) y Claude Code (ejecución/implementación).

---

## Orden de Lectura Obligatorio

### Para Claude Code al inicio de sesión:
1. **CURRENT/STATUS.md** — estado actual (atender alertas antes de continuar)
2. **CURRENT/INDEX.md** — índice de archivos disponibles
3. **CURRENT/DECISIONS.md** — últimas DECs vigentes
4. Solo si el tema lo requiere: leer archivo específico (ARCHITECTURE, ROADMAP, etc.)

**STATUS.md es el punto de entrada.** Si hay "Alertas para Perplexity", detener y notificar.

---

## División de Responsabilidades

### Perplexity
- ✅ Investigación (web search, docs, referencias)
- ✅ Estrategia comercial (GROWTH.md)
- ✅ Identidad visual (DESIGN.md)
- ✅ Generación de prompts estructurados para Claude Code
- ✅ Debate arquitectura con Rodhan
- ✅ Escribe en brain (DECISIONS.md, VISION.md — solo docs)
- ❌ **NO escribe código**
- ❌ **NO ejecuta git commands**

### Claude Code
- ✅ Ejecución de código (implementación, refactoring)
- ✅ Git (commits, push, branches, PRs)
- ✅ Diagnóstico (lee estado + reporta sin modificar)
- ✅ Actualiza STATUS.md al finalizar sesión
- ✅ Actualiza DECISIONS.md cuando toma decisión técnica relevante
- ❌ **NO decide arquitectura sin prompt**
- ❌ **NO modifica GROWTH.md salvo instrucción explícita**
- ❌ **NO modifica DESIGN.md salvo instrucción explícita**

### Rodhan
- ✅ Decisiones de negocio (pricing, target, mercados)
- ✅ Aprobación de arquitectura (stack, decisiones DEC-XXX)
- ✅ Dirección de producto (roadmap, prioridades)
- ✅ Revisión y aprobación de resultados

---

## Flujo de Trabajo

```
Rodhan (usuario)
    │
    ├─→ Pregunta/tarea compleja → Perplexity
    │   │
    │   ├─→ Investigación web/docs
    │   ├─→ Análisis de contexto
    │   ├─→ Genera prompt estructurado
    │   └─→ Devuelve prompt a Rodhan
    │
    └─→ Tarea de implementación → Claude Code
        │
        ├─→ Lee STATUS.md + DECISIONS.md
        ├─→ Ejecuta prompt
        ├─→ Implementa código
        ├─→ Commits + push
        ├─→ Actualiza STATUS.md
        └─→ Reporta resultado a Rodhan
```

---

## Checklist de Cierre — Claude Code

Al terminar TODA sesión de trabajo:

1. [ ] Actualizar STATUS.md:
   - [ ] Completado hoy (con detalles específicos)
   - [ ] Pendientes inmediatos
   - [ ] Bloqueantes (si existen)
   - [ ] Commits recientes
   - [ ] Métricas actualizadas
   
2. [ ] Si hubo decisión técnica relevante:
   - [ ] Agregar DEC-XXX a DECISIONS.md
   - [ ] Referenciar en commit message

3. [ ] Git:
   ```bash
   cd /home/rodri/arandil-workspace/arandil-brain
   git add .
   git commit -m "docs: end-of-session sync $(date +%Y-%m-%d)"
   git push
   ```

4. [ ] Si hubo cambios en código (monorepo):
   ```bash
   cd /home/rodri/arandil-workspace/arandil
   git add .
   git commit -m "feat(...): descripción cambios"
   git push
   ```

5. [ ] Sugerencias proactivas:
   - 🔧 Opciones de solución para problemas detectados
   - ⚡ Optimizaciones observadas
   - 📋 Actualizaciones de documentación pendientes
   - ⚠️ Riesgos o inconsistencias para que Perplexity evalúe

**⚠️ Sin push al final de sesión, Perplexity lee datos desactualizados en GitHub la próxima vez.**

---

## Anti-Patrones a Evitar

### ❌ Claude Code decide arquitectura sin consultar
**Mal:**
> "Voy a cambiar el stack de PostgreSQL a MongoDB porque es más flexible."

**Bien:**
> "Detecto que el schema actual tiene muchas queries JOIN complejas. Alternativas: 1) Optimizar indexes, 2) Denormalizar, 3) Evaluar cambio de DB. Requiere decisión de Rodhan."

### ❌ Perplexity escribe código directamente
**Mal:**
> "Aquí está el código completo de auth.ts..."

**Bien:**
> "Prompt para Claude Code: Implementar auth middleware con Supabase JWT. Validar token en header, extraer userId, adjuntar a req.user. Ver patrón en Arandur services/api/src/middleware/auth.ts."

### ❌ Claude Code modifica GROWTH.md sin instrucción
**Mal:**
> "Actualicé GROWTH.md con nueva estrategia de marketing."

**Bien:**
> "Detecto que GROWTH.md no tiene estrategia de beta definida. Sugiero que Perplexity + Rodhan completen esta sección antes de FASE 8."

### ❌ Diagnóstico sin actualizar STATUS.md
**Mal:**
> "Encontré 3 bugs críticos en API. [no documenta]"

**Bien:**
> "Encontré 3 bugs críticos. Documentando en STATUS.md > Bloqueantes con steps de reproducción."

---

## Protocolo Diagnóstico Antes de Fix

Antes de generar cualquier prompt de cambio, evaluar:

1. ¿STATUS.md fue actualizado en las últimas 24h? Si no → pedir diagnóstico
2. ¿La tarea involucra componente cuyo estado exacto no está documentado? → pedir diagnóstico
3. ¿El cambio afecta más de un componente del sistema? → pedir diagnóstico primero

**PASO 1 — Diagnóstico:** Claude Code solo lee + reporta + actualiza STATUS.md (sin modificar código)  
**PASO 2 — Verificación:** Perplexity verifica coherencia con DECISIONS.md  
**PASO 3 — Ejecución:** Solo cuando Perplexity tiene imagen completa del estado real

---

## Regla de Oro

**arandil-brain es la única fuente de verdad.**

- Perplexity debate y genera prompts. 
- Claude Code ejecuta; nunca decide sin prompt.
- Prohibido generar prompts que resuelvan solo el síntoma sin atacar la causa raíz.

---

**Última actualización:** 2026-07-04  
**Próxima revisión:** Al completar primera sesión colaborativa Perplexity ↔ Claude Code
