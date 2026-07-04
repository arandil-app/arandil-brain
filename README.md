---
type: reference
title: "Arandil Brain — README"
description: "Overview del brain: qué contiene, cómo usarlo, estructura de archivos"
tags: [readme, brain, overview, estructura]
timestamp: 2026-07-04
---

# Arandil Brain

> **Fuente de verdad documental del proyecto Arandil.**

Este repositorio contiene toda la documentación de arquitectura, decisiones, visión de producto y estado del proyecto Arandil.

---

## ¿Qué es Arandil?

Arandil es una plataforma educativa global de aprendizaje de matemáticas, potenciada por IA, que permite a estudiantes de cualquier nivel dominar conceptos mediante mastery learning y repetición espaciada.

**Filosofía core:** Falla rápido, aprende, reintenta, repite hasta dominar.

**Stack:** React Native + Expo, Express + PostgreSQL, DeepSeek (IA), FSRS-7 (SRS), BullMQ + Redis.

---

## Estructura del Brain

```
arandil-brain/
├── CURRENT/                # Documentación activa
│   ├── STATUS.md           # Estado volátil (actualizado cada sesión)
│   ├── VISION.md           # Visión de producto
│   ├── DECISIONS.md        # Decisiones técnicas (DEC-001, DEC-002...)
│   ├── ARCHITECTURE.md     # Arquitectura del sistema
│   ├── ROADMAP.md          # Roadmap y fases
│   ├── GROWTH.md           # Estrategia comercial (Perplexity-only)
│   ├── DESIGN.md           # Identidad visual (Perplexity-only)
│   ├── INDEX.md            # Índice OKF de archivos
│   ├── AGENTS-PROTOCOL.md  # Protocolo Perplexity ↔ Claude Code
│   └── TROUBLESHOOTING.md  # Errores conocidos
│
├── ARCHIVE/                # Documentación histórica
│   └── SESSIONS.md         # Log de sesiones pasadas
│
├── README.md               # Este archivo
└── CLAUDE.md               # Protocolo Claude Code (en workspace root)
```

---

## Cómo Usar Este Brain

### Para Claude Code
1. **Inicio de sesión:** Leer `CURRENT/STATUS.md` primero
2. **Decisiones técnicas:** Consultar `CURRENT/DECISIONS.md` antes de cambios arquitectónicos
3. **Arquitectura:** Ver `CURRENT/ARCHITECTURE.md` para entender stack y componentes
4. **Cierre de sesión:** Actualizar `STATUS.md` y hacer push (ver `AGENTS-PROTOCOL.md`)

### Para Perplexity
1. **Estrategia:** Editar `CURRENT/GROWTH.md` (comercial) y `DESIGN.md` (identidad)
2. **Decisiones:** Agregar DECs a `DECISIONS.md` cuando se tome decisión arquitectónica
3. **Investigación:** Usar brain para contexto, no para ejecución

### Para Rodhan
- **Vista general:** Leer `STATUS.md` para saber estado actual
- **Visión:** `VISION.md` para recordar norte del producto
- **Roadmap:** `ROADMAP.md` para ver fases y hitos

---

## Convenciones OKF

Todos los archivos Markdown del brain tienen **frontmatter OKF obligatorio:**

```yaml
---
type: concept | runbook | reference | log
title: "Título del documento"
description: "Descripción breve (1-2 líneas)"
tags: [tag1, tag2, tag3]
timestamp: 2026-07-04
volatile: true          # Solo STATUS.md
resource: ../ruta/      # Opcional
refs:                   # Opcional
  - OTRO-DOC.md
---
```

**Tipos OKF:**
- `concept`: Arquitectura, diseño, roadmaps (conocimiento)
- `runbook`: Protocolos, procedimientos (procesos ejecutables)
- `reference`: Registros, schemas, decisiones (datos de consulta)
- `log`: Status, audits, snapshots (temporal, con `volatile: true`)

---

## Archivos Obligatorios

| Archivo | Propósito | Actualizado por |
|---------|-----------|-----------------|
| **STATUS.md** | Estado actual volátil | Claude Code (cada sesión) |
| **VISION.md** | Visión de producto | Rodhan + Perplexity |
| **DECISIONS.md** | Decisiones técnicas (DEC-XXX) | Claude Code + Rodhan |
| **ARCHITECTURE.md** | Arquitectura del sistema | Claude Code + Rodhan |
| **ROADMAP.md** | Roadmap y fases | Rodhan + Perplexity |
| **GROWTH.md** | Estrategia comercial | Perplexity + Rodhan |
| **DESIGN.md** | Identidad visual | Perplexity + Rodhan |
| **INDEX.md** | Índice OKF | Claude Code |
| **AGENTS-PROTOCOL.md** | Protocolo colaboración | Claude Code + Rodhan |
| **TROUBLESHOOTING.md** | Errores conocidos | Claude Code |

---

## Límites y Mantenimiento

- **Tamaño máximo por archivo CURRENT/:** ~15 KB
- **Si supera:** Mover historial a `ARCHIVE/` y mantener solo estado vigente en `CURRENT/`
- **CURRENT/:** Solo documentación activa
- **ARCHIVE/:** Sesiones antiguas, DECs superadas, auditorías pasadas

---

## Regla de Oro

**arandil-brain es la única fuente de verdad.**

- Perplexity debate y genera prompts. 
- Claude Code ejecuta; nunca decide sin prompt.
- Rodhan aprueba arquitectura y decisiones de negocio.

---

## Links Útiles

- **Monorepo:** `/home/rodri/arandil-workspace/arandil/`
- **GitHub Brain:** https://github.com/arandil-app/arandil-brain
- **GitHub Monorepo:** https://github.com/arandil-app/arandil
- **Template de referencia:** `/home/rodri/setup-workspaces/PROJECT-TEMPLATE.md`

---

**Última actualización:** 2026-07-04  
**Próxima revisión:** Al completar FASE 0 (Bootstrap)
