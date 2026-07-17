---
type: reference
title: "Federación — Arandil bajo Zymbiotek SACS"
description: "Declara la pertenencia de Arandil a la federación corporativa Zymbiotek SACS: qué es corporativo (referenciado) y qué es local. Federar, no mezclar."
tags: [federacion, corporativo, sacs, gobierno]
timestamp: 2026-07-17
---

# Federación — Arandil bajo Zymbiotek SACS

> Arandil es un **producto independiente** federado bajo **Zymbiotek** (nombre paraguas / holding
> Zymbiotek SACS). Mapa corporativo canónico: `setup-workspaces/FEDERATION-ZYMBIOTEK-SACS.md`.
> Decisión de federación: `setup-workspaces/DECS-CORP.md` → SACS-001 (propuesta).
> Nota: "Zymbiotek" = nombre de la empresa, no un producto; el producto de bots es **Zymbiobots**.

## Identidad de producto (no se comparte, no se mezcla)
- **Repos**: `arandil-app/arandil` (app) · `arandil-app/arandil-brain` (este brain).
- **Producto**: plataforma educativa de **matemáticas** (mastery learning + repetición espaciada +
  generación IA de problemas y explicaciones). Target global, secundaria/universidad/adultos.
- **Hermano educativo**: Arandur (prep. admisión, repos `arandur-dev/*`) — **federado, NO fusionado**.
  Relación vía capa corporativa; sin dependencia directa de repos.

## Qué es CORPORATIVO (se referencia, no se decide aquí)
Vive en `SACS-*` (setup-workspaces/DECS-CORP.md): persona jurídica Zymbiotek SACS · arquitectura de
marca entre productos (Arandil figura como división educativa) · infra/cuentas compartidas · política
base Ley 29733 transversal · procesador de pagos a nivel holding · núcleo FSRS/repetición espaciada
compartido (si se libra como librería común). Arandil **hereda**; no duplica.

## Qué es LOCAL (se decide en este brain)
Features, roadmap, stack, pricing (freemium S/15/mes planificado), pedagogía mastery, generación de
problemas por IA. DECs locales siguen `DEC-NNN` en [DECISIONS.md](DECISIONS.md) (próximo: DEC-016).

## zymskull (registro documental)
`[[project]] arandil` con `org="zymbiotek-sacs"` [alta documental pendiente]. Zymnodes iniciales
(estado `incubating`, mapear en este brain cuando se aborde): `arandil-core`, `arandil-mobile`.

## Cross-references
- ⬆️ Corporativo: `setup-workspaces/FEDERATION-ZYMBIOTEK-SACS.md`, `DECS-CORP.md`.
- ↔️ Arandur: relación documentada en el mapa corporativo — **prohibida dependencia directa** de repos.
- Prioridad de despliegue de Arandil: `setup-workspaces/PRIORITIZATION-2026-07-17.md` (grupo "preparar").
